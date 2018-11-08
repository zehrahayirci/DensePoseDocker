# Installing NVIDIA

This is the pipeline to set up NVIDIA and use it with CUDA

## Install Graphics Driver 
Make sure you have the driver 

```
sudo add-apt-repository ppa:graphics-drivers/ppa

```
It finds available drivers (i.e 390,384)

```
aptitude search nvidia

```
Also, System Settings > Software & Updates > Additional Drivers should list the available driver.
Set the one with **highest number**. (i.e if it shows 384,387,**390**)
**Reboot** and 

```
nvidia-smi 

```

## Install Cuda 9.2
Because previous versions are not compatible with gcc version of 17.10, the best solution is to set up 9.2
Changing gcc version or setting soft links for another gcc versions may also solve this problem

Download ['Cuda 9.2'](https://developer.nvidia.com/cuda-92-download-archive?target_os=Linux&target_arch=x86_64&target_distro=Ubuntu)
Check the PATHS. Change if necessary. 

```
echo $PATH
echo $LD_LIBRARY_PATH

```


```
 export PATH=/usr/local/cuda-9.2/bin:$PATH
 export LD_LIBRARY_PATH=/usr/local/cuda-9.2/lib64:$LD_LIBRARY_PATH

```
Now, nvcc-V should work and it shows your CUDA version

```
nvcc -V
nvcc: NVIDIA (R) Cuda compiler driver
Copyright (c) 2005-2018 NVIDIA Corporation
Built on Tue_Jun_12_23:07:04_CDT_2018
Cuda compilation tools, release 9.2, V9.2.148

```
## CUDNN 
Download cuDNN v7.3.1 Library for Linux from [`official website`](https://developer.nvidia.com/rdp/cudnn-download) 
```
tar -xvzf cudnn-9.1-linux-x64-v7.1.tgz 
cd cuda
sudo cp */*.h /usr/local/cuda/include/
sudo cp */libcudnn* /usr/local/cuda/lib64/
sudo chmod a+r /usr/local/cuda/lib64/libcudnn*
```
Check with `nvidia-smi`

## NCCL 
Download NCCL 2.3.4 O/S agnostic and CUDA 9.2 for agnostic from [`official website`](https://developer.nvidia.com/nccl/nccl-legacy-downloads) 
```
tar -xvf nccl_2.3.4-1+cuda9.2_x86_64.txz 
sudo cp -vRf nccl_2.3.4-1+cuda9.2_x86_64/* /usr/local/nccl-2.3
```
Then I followed the steps in [this blog](https://tech.amikelive.com/node-735/how-to-install-nvidia-collective-communications-library-nccl-2-for-tensorflow-on-ubuntu-16-04/)
Step 6 gives a strange error while creating symbolic link
```
ln: failed to create symbolic link '/usr/include/nccl.h': File exists
```
But still nvdia-smi works and it can find NCLL

