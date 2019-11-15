# Docker container for runing RegCM
# It is based on alpine and some testing packages.
# Had to compile netcdf-c and netcdf-fortran before compiling RegCM 4.7.0
# Author: Guillermo Ambrosio <lgar89@gmail.com>
# Date: 2019/11/13

FROM alpine:3.10

RUN apk add --no-cache bash bash-doc bash-completion
RUN apk add --no-cache musl-dev 
RUN apk add --no-cache gfortran gdb make
RUN apk add --no-cache g++

RUN cd /opt/ && wget http://gforge.ictp.it/gf/download/frsrelease/259/1845/RegCM-4.7.0.tar.gz && tar xvfz RegCM-4.7.0.tar.gz && cd RegCM-4.7.0 
RUN cd /opt/ && mkdir netcdf && cd netcdf && wget https://github.com/Unidata/netcdf-c/archive/v4.7.2.tar.gz 

RUN apk add --no-cache curl-dev m4 
RUN apk add --no-cache -X http://dl-cdn.alpinelinux.org/alpine/edge/testing hdf5-dev

RUN cd /opt/netcdf && tar xfz v4.7.2.tar.gz && cd netcdf-c-4.7.2/ && ./configure && make && make install
RUN cd /opt/netcdf && wget https://github.com/Unidata/netcdf-fortran/archive/v4.5.2.tar.gz && tar xfz v4.5.2.tar.gz && ls 

RUN apk add --no-cache -X http://dl-cdn.alpinelinux.org/alpine/edge/testing openmpi openmpi-dev
RUN cd /opt/netcdf/netcdf-fortran-4.5.2/ && ./configure && make && make install

RUN cd /opt/RegCM-4.7.0 && ./configure && make && make install

RUN apk add openssh nano
