# SETUP in GADI-NCI:
  module load: 1) pbs   2) netcdf/4.9.0   3) intel-compiler/2021.8.0   4) intel-mpi/2021.8.0   5) hdf5/1.10.7(default)   6) intel-mkl/2021.4.0
  go to /scratch/ds2/MOM6-examples folder:
  
  ## Must do/FMS:
    go to /scratch/ds2/MOM6-examples folder:
    ```mkdir -p build/gnu/shared/repro/
    cd  build/gnu/shared/repro/ 
    ../../../../src/mkmf/bin/list_paths -l ../../../../src/FMS
    ../../../../src/mkmf/bin/mkmf -t ../../../../src/mkmf/templates/theia-intel.mk -p libfms.a -c "-Duse_libMPI -Duse_netCDF" path_names
    ```
    ```
    make NETCDF=3 REPRO=1 FC=mpiifort CC=mpiicc LD=mpiifort libfms.a -j
    ```
    
  ## Ocean only:
    go to /scratch/ds2/MOM6-examples folder:
    ```
    mkdir -p build/gnu/ocean_only/repro/
    cd build/gnu/ocean_only/repro/; rm -f path_names;
    ../../../../src/mkmf/bin/list_paths -l ./ ../../../../src/MOM6/{config_src/infra/FMS1,config_src/memory/dynamic_symmetric,config_src/drivers/solo_driver,config_src/external,src/{*,*/*}}/
    ../../../../src/mkmf/bin/mkmf -t ../../../../src/mkmf/templates/theia-intel.mk -o '-I../../shared/repro' -p MOM6 -l '-L../../shared/repro -lfms' path_names
    ```
    ```
    make NETCDF=3 REPRO=1 FC=mpiifort CC=mpiicc LD=mpiifort libfms.a -j
    ```
    
  ## Ocean -SIS2 coupled:
    go to /scratch/ds2/MOM6-examples folder:
    ```
    mkdir -p build/intel/ice_ocean_SIS2/repro/
    cd build/gnu/ice_ocean_SIS2/repro/
    git submodule update --init --recursive
    ../../../../src/mkmf/bin/list_paths -l ./ ../../../../src/MOM6/config_src/{infra/FMS1,memory/dynamic_symmetric,drivers/FMS_cap,external} ../../../../src/MOM6/src/{*,*/*}/ ../../../../src/{atmos_null,coupler,land_null,ice_param,icebergs,SIS2,FMS/coupler,FMS/include}/
    ../../../../src/mkmf/bin/mkmf -t ../../../../src/mkmf/templates/theia-intel.mk -o '-I../../shared/repro' -p MOM6 -l '-L../../shared/repro -lfms' -c '-Duse_AM3_physics -D_USE_LEGACY_LAND_' path_names
    ```
    ```
    make NETCDF=3 REPRO=1 FC=mpiifort CC=mpiicc LD=mpiifort libfms.a -j
    ```
