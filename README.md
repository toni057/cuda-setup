# Installation guide to cuda and nvidia video driver gt650m on Y500

### init
sudo apt-get install linux-headers-$(uname -r)

### install cuda
sudo dpkg -i cuda-repo-ubuntu1604-8-0-local_8.0.44-1_amd64.deb
sudo apt-get update
sudo apt-get install cuda

### add environment variables to .profile
gedit ~/.profile
export PATH=/usr/local/cuda-8.0/bin${PATH:+:${PATH}}
export LD_LIBRARY_PATH=/usr/local/cuda-8.0/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}

### check env. variables
printenv

## now install proprietary drivers from software&updates

### remove previous driver version
sudo apt-get remove --purge nvidia-*

### check cuda version
nvcc -V

### not sure if needed
sudo update-initramfs -u

### show video
lshw -c video

### ckeck version
lspci | grep -i nvidia

### stop graphics device
sudo service lightdm stop

## Install driver
sudo chmod +x NVIDIA-Linux-x86_64-367.57.run
sudo ./NVIDIA-Linux-x86_64-367.57.run
sudo ln -sf /usr/lib/x86_64-linux-gnu/libGL.so.1 /usr/lib/libGL.so.1

## Install cuDNN
tar xvzf cudnn-8.0-linux-x64-v5.0-ga.tgz
sudo cp -P cuda/include/cudnn.h /usr/local/cuda/include
sudo cp -P cuda/lib64/libcudnn* /usr/local/cuda/lib64
sudo chmod a+r /usr/local/cuda/include/cudnn.h /usr/local/cuda/lib64/libcudnn*

*to fix "cannot find -lboost_python3",  still doesn't compile*
sudo ln -s libboost_python-py35.so libboost_python3.so
