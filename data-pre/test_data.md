Siguiendo la guía de usuario, en el capítulo 4, se desribe los datos necesarios para correr una simulación de prueba. 

Ese capítulo describe el siguiente script para descargar esos datos.

```
export ICTP_DATASITE=http://clima-dods.ictp.it/regcm4
export REGCM_GLOBEDAT=~/DATOS/RegCM/

cd $REGCM_GLOBEDAT
mkdir SURFACE CLM CLM45 SST EIN15

cd $REGCM_GLOBEDAT
cd SURFACE
curl -o GTOPO_DEM_30s.nc ${ICTP_DATASITE}/SURFACE/GTOPO_DEM_30s.nc
curl -o GLCC_BATS_30s.nc ${ICTP_DATASITE}/SURFACE/GLCC_BATS_30s.nc
curl -o GLZB_SOIL_30s.nc ${ICTP_DATASITE}/SURFACE/GLZB_SOIL_30s.nc
curl -o GMTED_DEM_30s.nc ${ICTP_DATASITE}/SURFACE/GMTED_DEM_30s.nc

cd $REGCM_GLOBEDAT
cd SURFACE
curl -o ETOPO_BTM_30s.nc ${ICTP_DATASITE}/SURFACE/ETOPO_BTM_30s.nc

cd $REGCM_GLOBEDAT
cd SST
CDCSITE="ftp.cdc.noaa.gov/pub/Datasets/noaa.oisst.v2"
curl -o sst.wkmean.1981-1989.nc ftp://$CDCSITE/sst.wkmean.1981-1989.nc
curl -o sst.wkmean.1990-present.nc ftp://$CDCSITE/sst.wkmean.1990-present.nc

cd $REGCM_GLOBEDAT
cd EIN15
mkdir 1990
cd 1990
export __hhs="00 06 12 18"
export __types="air hgt rhum uwnd vwnd"
for type in $__types
  do
  for hh in $__hhs
    do
      echo "downloading" ${ICTP_DATASITE}/EIN15/1990/${type}.1990.${hh}.nc
      curl -o ${type}.1990.${hh}.nc -C - ${ICTP_DATASITE}/EIN15/1990/${type}.1990.${hh}.nc
  done
done

cd $REGCM_GLOBEDAT
cd CLM
curl -o mksrf_fmax.nc ${ICTP_DATASITE}/CLM/mksrf_fmax.nc
curl -o mksrf_glacier.nc ${ICTP_DATASITE}/CLM/mksrf_glacier.nc
curl -o mksrf_lai.nc ${ICTP_DATASITE}/CLM/mksrf_lai.nc
curl -o mksrf_lanwat.nc ${ICTP_DATASITE}/CLM/mksrf_lanwat.nc
curl -o mksrf_navyoro_20min.nc ${ICTP_DATASITE}/CLM/mksrf_navyoro_20min.nc
curl -o mksrf_pft.nc ${ICTP_DATASITE}/CLM/mksrf_pft.nc
curl -o mksrf_soicol_clm2.nc ${ICTP_DATASITE}/CLM/mksrf_soicol_clm2.nc
curl -o mksrf_soitex.10level.nc ${ICTP_DATASITE}/CLM/mksrf_soitex.10level.nc
curl -o mksrf_urban.nc ${ICTP_DATASITE}/CLM/mksrf_urban.nc
curl -o pft-physiology.c070207 ${ICTP_DATASITE}/CLM/pft-physiology.c070207
curl -o pft-physiology.c070207.readme ${ICTP_DATASITE}/CLM/pft-physiology.c070207.readme
curl -o rdirc.05.061026 ${ICTP_DATASITE}/CLM/rdirc.05.061026


cd $REGCM_GLOBEDAT
cd CLM45
mkdir megan pftdata snicardata surface
export __types="megan pftdata snicardata surface"
for dir in $__types; do 
	cd ..
	cd $dir 
	wget ${ICTP_DATASITE}/CLM45/$dir/ -O - | \
	wget -A ".nc" -l1 --no-parent --base=${ICTP_DATASITE}/CLM45/$dir/ -nd -Fri -; 
done
```
