<p align = 'center'>
    <font size="6" face="arial" color="">
        <b>
            The first glance of ZENO
        </b>
    </font>
</p>

### What is ZENO?

ZENO is an OpenSource, Node based 3D system able to produce cinematic physics effects at High Efficiency, it was designed for large scale simulations and has been tested on complex setups. Aside of its simulation Tools, ZENO provides necessary visualization nodes for users to import and run simulations if you feel that the current software you are using is too slow.

![hello_zeno](img/hello_zeno.png)

### How to start on Ubuntu 20.04?

> If you want to simulate with GPU, make sure your CUDA Toolkit is above 11.3

#### Install CUDA Toolkit 11.4

> If you have a previous version, please remove it before you start.

```bash
cd  /usr/local/cuda/bin
sudo ./cuda-uninstaller
# Successfully uninstalled and clean some extra files
cd ../..
sudo rm -rf cuda-11.1/
```

**1. Check Updates**

```bash
sudo apt-get update
sudo apt-get upgrade
sudo apt-get autoremove
```

**2. Switch Navida Metapackage Version**

> Open Software & Updates and switch Navida metapackage to 470.
> Apply Changes.
>
> ```bash
> reboot
> ```

**3. Download Installer for Linux Ubuntu 20.04 x86_64** (https://developer.nvidia.com/cuda-downloads)

```bash
# If you not install wget, run $ sudo apt install wget
wget https://developer.download.nvidia.com/compute/cuda/11.4.1/local_installers/cuda_11.4.1_470.57.02_linux.run
sudo sh cuda_11.4.1_470.57.02_linux.run
```

> On the first prompt choose Continue.
> On the second prompt press Enter add the first metapackage to blacklist.
> Choose Install.

**4. Config Environment Variables**

```bash
gedit ~/.bashrc
# Add these lines to bashrc file.
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda/lib64
export PATH=$PATH:/usr/local/cuda/bin
export CUDA_HOME=$CUDA_HOME:/usr/local/cuda

# Exit current terminal and open a new terminal, or Config bashrc.
source ~/.bashrc
# Do the same with zshrc. if you are using zsh

# See CUDA version
nvcc -V
# nvcc: NVIDIA (R) Cuda compiler driver
# Copyright (c) 2005-2021 NVIDIA Corporation
# Built on Wed_Jul_14_19:41:19_PDT_2021
# Cuda compilation tools, release 11.4, V11.4.100
# Build cuda_11.4.r11.4/compiler.30188945_0
```

**5. Test CUDA**

```bash
cd /usr/local/cuda/samples/1_Utilities/deviceQuery
make # If permission denied type $ sudo make
./deviceQuery #If permission denied type $ sudo ./deviceQuery

# Result = PASS indicate CUDA has installed successfully.
```

> Tips:
>
> If you think your CUDA Toolkit is too large, you can move it to other disk and may symbolic link 
>
> In /usr/local/ dir and do:
>
> $ sudo ln -s ~/Disk/Linux/cuda-11.4 cuda-11.4 && sudo ln -s cuda-11.4 cuda



#### Install some dependencies

**1. Install basic dependencies**

```bash
sudo apt-get install gcc make python-is-python3 python-dev-is-python3 python3-pip qt5dxcb-plugin

python --version  # make sure Python version >= 3.6
sudo python -m pip install -U pip
sudo python -m pip install pybind11 numpy PySide2
```

**2. Install cmake**

> You can using apt-get install cmake, but the version maybe not the latest.

```bash
# cmake-curses-gui is optional for easily altering cmake configurations from terminal
sudo apt-get install cmake cmake-curses-gui
```

> Recommend build cmake from source.

```bash
# remove cmake you may installed before
sudo apt-get remove cmake

# Build cmake
wget https://cmake.org/files/v3.21/cmake-3.21.1.tar.gz
tar –xvzf cmake-3.21.1.tar.gz
cd cmake-3.21.1
./bootstrap && make && sudo make install

# See cmake version
cmake --version
```

**3. Install some optional tool**

```bash
# (Optional) Installing IlmBase and other dependencies:
sudo apt-get install -y libilmbase-dev libopenexr-dev
sudo apt-get install -y zlib1g-dev libeigen3-dev libopenblas-dev

# (Optional) Installing OpenVDB dependencies (Boost, TBB, Blosc):
sudo apt-get install -y libboost-iostreams-dev libboost-system-dev libtbb-dev
git clone https://github.com/Blosc/c-blosc.git --branch=v1.5.0
cd c-blosc && mkdir build && cd build
cmake .. && make -j4 && sudo make install && cd ../..

# (Optional) Install OpenVDB:
git clone https://github.com/AcademySoftwareFoundation/openvdb.git --branch=v7.2.1
cd openvdb && mkdir build && cd build 
cmake .. && make -j4 && sudo make install && cd ../..
```



#### Install latest ZENO

```bash
# Get ZENO source code
git clone https://github.com/zenustech/zeno.git
cd zeno
# (Optional) if you want to build GPUMPM
git submodule update --init --recursive
# Build ZENO
cmake -B build
# change some cmake configurations via ccmake:
ccmake -B build  # will shows up a curses screen, c to save, q to exit
cmake --build build --parallel
```

> NOTE:
>
> I set EasyGL to OFF, other details see https://github.com/zenustech/zeno, my configuration file is in the end of this file.



### The first glance of ZENO

```bash
# You can add an alias in your terminal rc file
# alias zeno='cd /home/veen/Disk/Linux/zeno && ./run.sh'
vi ~/.bashrc # add alias to the end
# Exit current terminal and open a new terminal, or Config bashrc.
source ~/.bashrc
# Do the same with zshrc. if you are using zsh
# Before you done this, you can run $ ./run.sh in your terminal in zeno dir
# After you done this, you can run $ zeno in your terminal at anywhere
```

**Hello ZENO!**

> Open ```zeno/arts/OfficialSubnets/zelloWorld.zsg``` and run it you will see **THIS IS ZENO**

![hello_zeno](img/hello_zeno.png)



> My config file

```
 BUILD_BULLET2_DEMOS              OFF
 BUILD_BULLET3                    ON
 BUILD_CLSOCKET                   ON
 BUILD_CPU_DEMOS                  OFF
 BUILD_EGL                        ON
 BUILD_ENET                       ON
 BUILD_EXTRAS                     OFF
 BUILD_OPENGL3_DEMOS              OFF
 BUILD_PYBULLET                   OFF
 BUILD_UNIT_TESTS                 OFF
 BULLET2_MULTITHREADING           ON
 BULLET2_USE_OPEN_MP_MULTITHREA   ON
 BULLET2_USE_TBB_MULTITHREADING   OFF
 Boost_INCLUDE_DIR                /usr/include
 Boost_IOSTREAMS_LIBRARY_RELEAS   /usr/lib/x86_64-linux-gnu/libboost_iostreams.
 Boost_SYSTEM_LIBRARY_RELEASE     /usr/lib/x86_64-linux-gnu/libboost_system.so.
 CACHE_BINARY                     CACHE_BINARY-NOTFOUND
 CACHE_OPTION                     ccache
 CLAMP_VELOCITIES                 0
 CMAKE_BACKWARDS_COMPATIBILITY    2.4
 CMAKE_BUILD_TYPE                 Release
 CMAKE_CUDA_ARCHITECTURES         52
 CMAKE_INSTALL_PREFIX             /usr/local
 EASYGL_ENABLE_GLUT               ON
 ENABLE_CACHE                     ON
 ENABLE_CLANG_TIDY                OFF
 ENABLE_COVERAGE                  OFF
 ENABLE_CPPCHECK                  OFF
 ENABLE_DOXYGEN                   OFF
 ENABLE_INCLUDE_WHAT_YOU_USE      OFF
 ENABLE_SANITIZER_ADDRESS         OFF
 ENABLE_SANITIZER_MEMORY          OFF
 ENABLE_SANITIZER_THREAD          OFF
 ENABLE_SANITIZER_UNDEFINED_BEH   OFF
 ENABLE_VHACD                     ON
 EXECUTABLE_OUTPUT_PATH
 EXTENSION_EasyGL                 OFF
 EXTENSION_FastFLIP               ON
 EXTENSION_OldZFX                 OFF
 EXTENSION_Rigid                  ON
 EXTENSION_ZMS                    ON
 EXTENSION_ZenoFX                 ON
 EXTENSION_gmpm                   ON
 EXTENSION_mesher                 ON
 EXTENSION_oldzenbase             ON
 EXTENSION_zenvdb                 ON
 Eigen3_DIR                       /usr/lib/cmake/eigen3
 INCLUDE_INSTALL_DIR              include/bullet
 INSTALL_CMAKE_FILES              ON
 INSTALL_LIBS                     ON
 INTERNAL_UPDATE_SERIALIZATION_   OFF
 IlmBase_Half_LIBRARY             /usr/lib/x86_64-linux-gnu/libHalf.so         
 LIBRARY_OUTPUT_PATH   
 LIB_DESTINATION                  lib
 LIB_SUFFIX        
 OpenVDB_openvdb_INCLUDE_DIR      /usr/local/include
 OpenVDB_openvdb_LIBRARY          /usr/local/lib/libopenvdb.so
 PARTIO_BUILD_SHARED_LIBS         ON
 PKGCONFIG_INSTALL_PREFIX         lib/pkgconfig/
 PYBIND11_FINDPYTHON              OFF
 PYBIND11_INSTALL                 OFF
 PYBIND11_NOPYTHON                OFF
 PYBIND11_PYTHON_VERSION
 PYBIND11_TEST                    OFF
 TBB_DIR                          /usr/lib/x86_64-linux-gnu/cmake/TBB
 Tbb_tbb_LIBRARY                  /usr/lib/x86_64-linux-gnu/libtbb.so          
 USE_DOUBLE_PRECISION             OFF
 USE_GLUT                         ON
 USE_GRAPHICAL_BENCHMARK          OFF
 USE_MSVC_INCREMENTAL_LINKING     OFF
 USE_MSVC_RELEASE_RUNTIME_ALWAY   OFF
 USE_MSVC_RUNTIME_LIBRARY_DLL     OFF
 USE_OPENVR                       OFF
 USE_SOFT_BODY_MULTI_BODY_DYNAM   ON
 ZENOFX_ENABLE_OPENVDB            ON
 ZENO_BUILD_EXTENSIONS            ON
 ZENO_BUILD_TESTS                 OFF
 ZENO_BUILD_ZFX                   ON
 ZENO_ENABLE_OPENMP               ON
 ZENO_ENABLE_PYTHON               ON
 ZENO_FAULTHANDLER                ON
 ZENO_GLOBALSTATE                 ON
 ZENO_VISUALIZATION               ON
 ZFX_ENABLE_CUDA                  ON
 ZFX_PRINT_IR                     OFF
 ZS_BUILD_SHARED_LIBS             OFF
 ZS_ENABLE_CUDA                   ON
 ZS_ENABLE_INSTALL                OFF
 ZS_ENABLE_OPENGL                 OFF
 ZS_ENABLE_OPENMP                 ON
 ZS_ENABLE_OPENVDB                ON
 ZS_ENABLE_PACKAGE                OFF
 ZS_ENABLE_PARTIO                 ON
 ZS_ENABLE_PCH                    OFF
 ZS_ENABLE_PTHREADS               ON
 gp_cmd                           /usr/bin/ldd

```

