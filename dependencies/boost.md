# Boost C++ Libraries Setup Guideline
This is a step-by-step guide to install Boost C++ Libraries.

## Build and Install Boost

```sh
cd /root
wget https://dl.bintray.com/boostorg/release/1.70.0/source/boost_1_70_0.tar.gz
tar xf boost_1_70_0.tar.gz
cd boost_1_70_0
./bootstrap.sh

## Input your custom number of jobs
./b2 toolset=clang -j$CPU_CORES install
```
