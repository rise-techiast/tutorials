# CMake Setup Guideline

## Build and Install CMake

```sh
cd /root
wget https://github.com/Kitware/CMake/releases/download/v3.14.5/cmake-3.14.5.tar.gz
tar xf cmake-3.14.5.tar.gz
cd cmake-3.14.5

## Input your custom number of jobs
./bootstrap --parallel=$CPU_CORES
make -j$CPU_CORES
make -j$CPU_CORES install
```
