# Benchmarking cluster computing facilities
Test case files and supplementary material for benchmarking FASTER and Grace cluster computing facilities for a short paper for PEARC-23 conference.

## Description

Two types benchmarking tests were run on FASTER cluster computing facility: 
1. Benchmarking with a Taylor-Green Vortex breakdown case on PyFR
2. P2P communication bandwidth/latency test

This repository attempts to provide all details required to replicate the test case on FASTER and Grace facilities. 

The following steps were followed to set up PyFR on FASTER and Grace cluster computing facilities. 

### PyFR dependancies setup on FASTER

#### Modules required for Python 3.11.1 for PYFR
    module load foss/2022b
    module load libffi/3.4.4
    module load OpenSSL/1.1.1k
    module load METIS/5.1.0
    module load HDF5/1.13.1

#### decide on python install location. Decide on a `.local` directory to install Python 3.11.1 from source. 

INSTALL=
LOCAL=

#### Download, extract and compile `Python 3.11.1`

    cd $INSTALL/Python-3.11.1/
    ./configure --prefix=$LOCAL --enable-shared --with-system-ffi --with-openssl=/sw/eb/sw/OpenSSL/1.1.1k-GCCcore-11.2.0/ PKG_CONFIG_PATH=$LOCAL/pkgconfig LDFLAGS=/usr/lib64/libffi.so.6.0.2

    make clean; make -j20; make install;

#### Create virtual environment for PyFR in the correct directory

  pip3.11 install virtualenv
  python3.11 -m venv pyfr-venv
  . pyfr-venv/bin/activate

#### Get all PyFR connected packages

    pip3 install --upgrade pip
    pip3 install --no-cache-dir wheel
    pip3 install --no-cache-dir botorch pandas matplotlib pyfr
    pip3 uninstall -y pyfr

#### Finally, setup PyFR from source

    cd /scratch/user/sambit98/github/PyFR/
    python3 setup.py develop


#### Generating hexahedral elements meshes for the TGV case

All meshes were created using a python script from Will Trojak's Github repository:
  https://github.com/WillTrojak/basic_gmsh

The python script `cube_hex_mesh.py` was run with the following arguements:
  python3 cube_hex_mesh.py -n 64 -l 3.14159265 
With the above command, a \pi^3^ length units domain with 64^3^ elements is created.



## Other benchmarking tests
### P2P communication between multiple NVIDIA GPUs

The following code was run on one node on the FASTER cluster that was dynamically connected to 16 NVIDIA A100 GPUs. Results of the P2P communicaion/latency benchmarking code is given in file P2P-comm.txt

  https://gist.github.com/joshlk/bbb1aca6e70b11d251886baee6423dcb
