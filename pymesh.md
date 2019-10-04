## How to install Pymesh 

1. Go to their site [`Pymesh Install`](https://pymesh.readthedocs.io/en/latest/installation.html#building-pymesh)
2. Follow from the build from the source
```
git clone https://github.com/PyMesh/PyMesh.git
cd PyMesh
git submodule update --init
export PYMESH_PATH=`pwd`
```
Install dependencies 
```
sudo apt-get install \
    libeigen3-dev \
    libgmp-dev \
    libgmpxx4ldbl \
    libmpfr-dev \
    libboost-dev \
    libboost-thread-dev \
    libtbb-dev \
    python3-dev
```
Install pip
```
pip install -r $PYMESH_PATH/python/requirements.txt
```

Continue with Build with setup.py
**CMake build gave me segmentation fault!!!**
```
cd $PYMESH_PATH/third_party
mkdir build
cd build
cmake ..
make
make install
```
To test, cd build nd make tests here
```
make
make tests
```
For installation
```
./setup.py install
```
## if update CMake
Download the latest one from [here](https://cmake.org/download/)
Export the file, then in the file run the bootstrap
```
./bootstrap && make && sudo make install
```
check the version updated
```
cmake --version
``` 
This may only work in the new terminal 
