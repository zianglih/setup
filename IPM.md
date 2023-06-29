# IPM on HPC

## Useful Links
- https://hpcadvisorycouncil.atlassian.net/wiki/spaces/HPCWORKS/pages/2910060545/Profiling+using+IPM+and+HPC-X
- https://ipm-hpc.sourceforge.net/
- https://ploticus.sourceforge.net/doc/download.html
- https://github.com/nerscadmin/IPM

## Download IPM
```bash
git clone https://github.com/nerscadmin/IPM.git
```

## Define Paths and Variables
Change this path to your desired install directory and run.
```bash
IPM_INSTALL_DIR=/home/ziangli/IPM_install_dir
IPM_LOG_DIR=/home/ziangli/IPM_log_dir
```

## Install IPM
First, load all related modules, this is to let the configure script set options automatically.
```bash
module load gcc openmpi cuda
```
Configure and install:
```bash
./configure --prefix $IPM_INSTALL_DIR
make -j 32
make install
```

## Run Profiling
cd into your desired exec folder.  
Run make as usual.  
Rerun make, but add ``` -L$IPM_install_dir/lib -lipm``` to the end of the link command.  
For CNS, the link command is exactly the last line of make output so we can use the following script:
```bash
module purge
module load cuda gcc openmpi
make > make.log
link_cmd=$(tail -n 1 make.log)
link_cmd+=" -L$IPM_install_dir/lib -lipm"
rm make.log
$link_cmd
```
Configure IPM:
```bash
export IPM_REPORT=full IPM_LOG=full IPM_LOGDIR=$IPM_LOG_DIR
```
Run:
```bash
mpirun -np 2 -x LD_PRELOAD=$IPM_INSTALL_DIR/lib/libipm.so CNS.ex inputs
```
A xml report will be generated under IPM_LOG_DIR.
## Parse the Report