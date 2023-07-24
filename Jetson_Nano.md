# Jetson Nano
** For Jetson Nano Developer Kit, not 2GB **

## General Guide
*Get Started With Jetson Nano Developer Kit:*  
https://developer.nvidia.com/embedded/learn/get-started-jetson-nano-devkit

## SD Card Image
https://developer.nvidia.com/jetson-nano-sd-card-image

## Headless Mode
### Connect:
```bash
ssh ziangli@192.168.55.1
```
### Initial Setup From Host:
```bash
sudo screen $(ls /dev/cu.usbmodem*)
```

## Useful Things To Do
### Set the default python to python3:
```bash
echo "alias python=python3" >> ~/.bashrc && source ~/.bashrc
```
### Install jtop:
```bash
sudo pip3 install -U jetson-stats
jtop
```


## PyTorch
*Nvidia Developer Forum PyTorch for Jetson:*  
https://forums.developer.nvidia.com/t/pytorch-for-jetson/72048

For Jetson Nano, I use JetPack 4, PyTorch v1.10.0, torchvision v0.11.1.  
Keep in mind to install both PyTorch and torchvision.
### Install PyTorch:
```bash
wget https://nvidia.box.com/shared/static/fjtbno0vpo676a25cgvuqc1wty0fkkg6.whl -O torch-1.10.0-cp36-cp36m-linux_aarch64.whl
sudo apt-get install python3-pip libopenblas-base libopenmpi-dev libomp-dev
pip3 install Cython
pip3 install numpy torch-1.10.0-cp36-cp36m-linux_aarch64.whl
```
### Install pytorch-lightning
Using ```pip``` or ```pip3``` will not work.
```bash
python3 -m pip install pytorch-lightning
```
### Install torchvision:
```bash
sudo apt-get install libjpeg-dev zlib1g-dev libpython3-dev libopenblas-dev libavcodec-dev libavformat-dev libswscale-dev
git clone --branch v0.11.1 https://github.com/pytorch/vision torchvision
cd torchvision
export BUILD_VERSION=0.11.1
python3 setup.py install --user
cd ../
```

## CUDA
CUDA is installed but we need to add it to PATH.  
*Add PATH for CUDA:*  
https://forums.developer.nvidia.com/t/cuda-nvcc-not-found/118068

Append the following two lines to ~/.bashrc:
```bash
$ export PATH=/usr/local/cuda/bin${PATH:+:${PATH}}
$ export LD_LIBRARY_PATH=/usr/local/cuda/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
```

### Run this:
```bash
echo -e "export PATH=/usr/local/cuda/bin${PATH:+:${PATH}}\nexport LD_LIBRARY_PATH=/usr/local/cuda/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}" >> ~/.bashrc && source ~/.bashrc
```
### Check version using:
```bash
nvcc --version
```

## Fix "Illegal Instruction (core dumped)"
https://stackoverflow.com/questions/65631801/illegal-instructioncore-dumped-error-on-jetson-nano
```bash
echo "export OPENBLAS_CORETYPE=ARMV8" >> ~/.bashrc && source ~/.bashrc
```

## Change Swap Size
The following command can be used to check swap size:
```bash
free -m
```
To change swap size, run the following:
```bash
# Disable ZRAM:
sudo systemctl disable nvzramconfig

# Create 4GB swap file
sudo fallocate -l 4G /mnt/4GB.swap
sudo chmod 600 /mnt/4GB.swap
sudo mkswap /mnt/4GB.swap

# Append the following line to /etc/fstab
sudo su
echo "/mnt/4GB.swap swap swap defaults 0 0" >> /etc/fstab
exit

# REBOOT!
```

## Nsight Systems & Nsight Compute
Nsight Systems and Nsight Compute are not included in SD card image.  
To install, use Nvidia SDK Manager on a Ubuntu host computer.

### Some useful links:  
* https://forums.developer.nvidia.com/t/jetson-nano-nsight-systems-target-not-supported/256990
* https://forums.developer.nvidia.com/t/getting-target-is-not-supported-in-nvidia-nsight-systems-for-tx2/125704

### Alternatively,  use nvprof:
```bash
nvprof <EXE>
```

## SDK Manager
https://developer.nvidia.com/sdk-manager  
Installed on host computer, or a docker image.   
Note, on Apple Silicon Mac, this docker image is extremely slow.  
For Jetson Nano, should use Ubuntu 18.04.   
### Docker image install:
```bash
wget https://developer.nvidia.com/nvidia-sdk-manager-sdkmanager-docker-image-ubuntu1804
docker load -i ./sdkmanager-1.9.2.10899-Ubuntu_18.04_docker.tar.gz
docker tag sdkmanager:1.9.2.10899 sdkmanager:latest
```
### Docker image run:
```bash
docker run -it --rm sdkmanager --help
```