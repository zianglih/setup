# IPM

## Download IPM
```bash
git clone https://github.com/nerscadmin/IPM.git
```

## Define Paths and Variables
Change this path to your desired install directory and run.
```bash
IPM_INSTALL_DIR=</home/ziangli/IPM_install_dir>
IPM_LOG_DIR=</home/ziangli/IPM_log_dir>
```

## Install IPM
First, load all related modules, this is to let the configure script set options automatically.
```bash
module load gcc openmpi cuda
```
Configure and install:
```bash
./bootstrap.sh
./configure --prefix $IPM_INSTALL_DIR
make && make install
```
## Configure IPM
```bash
export IPM_REPORT=full IPM_LOG=full IPM_LOGDIR=$IPM_LOG_DIR
export IPM_HPM=PAPI_L1_TCM,PAPI_L2_TCM
```
More information on PAPI counter on https://amrex-codes.github.io/amrex/docs_html/External_Profiling_Tools.html#papi-performance-counters

## Run Profiling
cd into your desired exec folder.  
Run make as usual.  
Rerun make, but add ``` -L$IPM_install_dir/lib -lipm``` to the end of the link command.  
For CNS, the link command is exactly the last line of make output so we can use the following script:
```bash
module purge
module load cuda gcc openmpi
make -j 32 > tmp_make.log
link_cmd=$(tail -n 1 tmp_make.log)+" -L$IPM_INSTALL_DIR/lib -lipm"
rm tmp_make.log
$link_cmd
```
For CNS, run:
```bash
mpirun -np 2 -x LD_PRELOAD=$IPM_INSTALL_DIR/lib/libipm.so <CNS.ex> inputs
```
A xml report will be generated under IPM_LOG_DIR.
## Parse the Report
Install ploticus from https://amrex-codes.github.io/amrex/docs_html/External_Profiling_Tools.html#papi-performance-counters  
mv the ```pl``` executable under ```bin``` to a path where it can be detected.  
Or simply add its path to ```PATH``` will do.  
Run to generate html report:
```bash
$IPM_INSTALL_DIR/bin/ipm_parse -html $IPM_LOG_DIR/<xmlfile>
```
ipm_parse will generate a folder under IPM_LOG_DIR.  
Navigate to the generated folder and open ```index.html``` with a browser and you can see the report.  

## Potential Issues
- Don't know whether it works if submitted as a job

## Useful Links
- https://hpcadvisorycouncil.atlassian.net/wiki/spaces/HPCWORKS/pages/2910060545/Profiling+using+IPM+and+HPC-X
- https://ipm-hpc.sourceforge.net/
- https://ploticus.sourceforge.net/doc/download.html
- https://github.com/nerscadmin/IPM
