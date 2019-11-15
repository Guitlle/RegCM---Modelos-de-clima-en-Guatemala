Construir imagen de docker: 

```
docker build -t "regcm:4.7.6" .
```


Correr contenedor Docker:

```
docker run -it -v ~/DATOS/RegCM:/data --name regcm regcm:4.7.6 bash
```

Esto crea un contenedor docker llamado "regcm" corriendo bash en modo interactivo. Allí se puede hacer uso del software. Se puede salir con "exit" y para volver a entrar a la consola en el contenedor se puede utilizar este comando:

```
docker start -ai regcm
```


Este script está descrito en una guía de usuario encontrada [aquí](https://gforge.ictp.it/gf/download/docmanfileversion/97/1690/UserGuide.pdf). Antes de correr esto es necesario descargar los datos, como se describe en test_data.md. 

```
export REGCM_GLOBEDAT=/data
export REGCM_ROOT=/opt/RegCM-4.7.6

cd $REGCM_GLOBEDAT

mkdir RUNS
cd RUNS
mkdir RUN01
export REGCM_RUN=/data/RUNS/RUN01/

cd $REGCM_RUN
mkdir input output
cp $REGCM_ROOT/Testing/test_001.in .

chmod  -R 777 $REGCM_RUN
```

Se debe editar test_001.in, poner los directorios de datos, de input y output, como se describe en la guía. Se debe comentar la línea con el parámetro "cftomax" en test_001.in.


```
...
 dirter = 'input/',
 inpter = '/data/',
...
 dirglob = 'input/',
 inpglob = '/data/',
...
 dirout='output/'
...
```

Estando en el directorio $REGCM_RUN Preparar los datos:

```
$REGCM_ROOT/bin/terrain test_001.in
$REGCM_ROOT/bin/sst test_001.in
$REGCM_ROOT/bin/icbc test_001.in
```

Ahora para correr deshabilito MCA en openmpi, para evitar errores y corro con 4 cpus (si tu computadora tiene distinta cantidad de cpus, ponerla en lugar del 4).

```
export OMPI_MCA_btl_vader_single_copy_mechanism=none
mpirun -np 4 $REGCM_ROOT/bin/regcmMPI test_001.in   
```
