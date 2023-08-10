# CUDA

## AWS EC2
Login => EC2 => Instances => Launch Instances

On initial launch, you may need to submit a ticket to increase limit of CPUs.

For a quick configuration, choose Ubuntu, Instance Type (I use g4dn), Storage, and click Launch instance.

If you want to use SSH, a key pair is required.
```bash
sudo apt-get update
sudo apt-get install linux-headers-$(uname -r)
# sudo apt-get install gcc g++ build-essential openmpi-bin openmpi-common libopenmpi-dev
sudo apt-get install g++ freeglut3-dev build-essential libx11-dev libxmu-dev libxi-dev libglu1-mesa libglu1-mesa-dev
sudo apt-get install libfreeimage3 libfreeimage-dev
sudo apt-get install python3 python3-pip python3-venv
sudo apt-key del 7fa2af80
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2204/x86_64/cuda-keyring_1.1-1_all.deb
sudo dpkg -i cuda-keyring_1.1-1_all.deb
sudo apt-get update
sudo apt-get install cuda
sudo apt-get install libcudnn8=8.9.4.*-1+cuda12.2
sudo apt-get install libcudnn8-dev=8.9.4.*-1+cuda12.2
sudo apt-get install libcudnn8-samples-dev=8.9.4.*-1+cuda12.2
echo -e "export PATH=/usr/local/cuda-12.2/bin${PATH:+:${PATH}}\nexport LD_LIBRARY_PATH=/usr/local/cuda-12.2/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}" >> ~/.bashrc && source ~/.bashrc
# reboot
```
For setting up Python venv:
```bash
python3 -m venv env
source env/bin/activate
pip install --upgrade pip
pip install torch torchvision pytorch-lightning==1.5.10 opencv-python
```