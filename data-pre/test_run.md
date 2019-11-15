Construir imagen de docker: 

```
docker build -t "regcm:4.7.0" .
```


Correr contenedor Docker:

```
guillermo@compu:~/DATOS/RegCM$ sudo docker run -d -v ~/DATOS/RegCM:/data --name regcm regcm:4.7.0

```


Esto corre en el contenedor docker

```
export ICTP_DATASITE=http://clima-dods.ictp.it/regcm4
export REGCM_GLOBEDAT=/data
export REGCM_ROOT=/opt/RegCM-4.7.0

cd $REGCM_GLOBEDAT
mkdir __RUNS
cd __RUNS
mkdir input output
cp $REGCM_ROOT/Testing/test_001.in .
chmod  -R 777 __RUNS/
ln -sf $REGCM_ROOT/bin .

export REGCM_RUN=/data/__RUNS/
```

Editar test_001.in


```
./bin/terrain test_001.in
```
