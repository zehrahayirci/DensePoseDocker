I have spent at least 50 times doing the same thing last 6 months, so here goes the complete roadmap of my workspace
On Ubuntu 18.04, NVIDIA-SMI 410.104, Cuda 10.0, OpenCL 2.1 

# Which Nvidia
## Install Graphics Driver 
I install Nvidia driver 410 from before Cuda Toolkit. So I will say no to Driver installation NO
```
sudo apt install nvidia-driver-410
```
## Which cuda
There is table to show which Nvidia drivers supports which Cuda 
https://gist.github.com/ax3l/9489132

I download Cuda 9.1 from https://developer.nvidia.com/cuda-91-download-archive, for Ubuntu 18.04, Run file 
```
sudo sh cuda_9.1.85_387.26_linux.run
```
Then from the terminal, 
Graphics Driver: No, others Yes, Create a symbolic link yes 
Reboot and check the PATH

```
echo $PATH
echo $LD_LIBRARY_PATH

```


```
 export PATH=/usr/local/cuda/bin:$PATH
 export LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH

```
To check everything is working 
```
nvidia-smi
nvcc-V
```
Some cases, this does not work. Then install the drivers on tty.
After reboot on the login screen Ctrl+Alt+F2 to enter the tty,login and execute the run file from there 
to stop lightdm one should work
```
sudo lightdm stop
sudo service lightdm stop
sudo service gdm stop 
```
## Which OpenCL

This should create OpenCL file under etc/OpenCL. If not, install the drivers again
```
apt-get install ocl-icd-libopencl1 opencl-headers clinfo
```
And link 
```
sudo ln -s /usr/lib/x86_64-linux-gnu/libOpenCL.so.1 /usr/lib/libOpenCL.so
```
There should be one icd file in etc/OpenCL/vendors/cat nvidia.icd  
when we cat nvidia.icd , we should get .so file 
If there is no vendor insice the folder, but we have .so file somewhere 
probably under usr/lib/x86_64-linux
```
 sudo echo libnvidia-opencl.so.1 >> /etc/OpenCL/vendors/nvidia.icd
```
Afterall, this should also be in LD PATH 
```
export LD_LIBRARY_PATH=/usr/lib/x86_64-linux-gnu:$LD_LIBRARY_PATH
```
Then clinfo should work 

Pyopencl sometimes confuse the .so file from the cuda 
```
 sudo mv /usr/local/cuda/targets/x86_64-linux/lib/libOpenCL.so.1 /usr/local/cuda/targets/x86_64-linux/lib/libOpenCL.so_from_cuda_do_not_use
```




