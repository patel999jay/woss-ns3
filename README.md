# woss-ns3
WOSS ns3 Integration Framework

This repository aims to introduce WOSS support for ns3 users.

WOSS is a multi-threaded C++ framework that permits the integration of any existing underwater channel simulator that expects environmental data as input and provides as output a channel realization. 
Currently, WOSS integrates the Bellhop ray-tracing program. 
Thanks to its automation the user only has to specify the location in the world and the time where the simulation should take place. This is done by setting the simulated date and the wanted latitude and longitude of every node involved. The simulator automatically handles the rest (see technical description). 
WOSS can be integrated in any network simulator or simulation tool that supports C++.

'woss-ns3' relies on the following libraries:
- WOSS
- NetCDF
- Acoustic Toolbox

Latest WOSS source code, installation instructions and related libraries can be found at http://telecom.dei.unipd.it/ns/woss/

'woss-ns3' module will be automatically installed by the ns3 app installer, but this feature is not yet complete.

It can also be manually installed by:
- downloading and installing the latest Acoustic Toolbox library
- downloading and installing the recommended NetCDF library
- downloading and installing the latest WOSS library
- cloning this repository in the '<ns3-dir>/src' path and then running ./waf configure 
--with-woss-source=<woss_source_path> --with-woss-library=<woss_lib_path> --with-netcdf-lib=<netcdf_installed_lib_path> --with-netcdf-include=<netcdf_installed_include_path>
- for info on how to install all the required libraries with the suggested paths, please check  http://telecom.dei.unipd.it/ns/woss/doxygen/installation.html

For any info and question please use the NS3 users mailing list.

Any issue should be reported via github Issue tracker.


Added By Jay : 

You need to perform installation of netCDF and HDF5 support in order to make everything working, I would recommand to use the exact steps/command shown at http://telecom.dei.unipd.it/ns/woss/doxygen/installation.html

if you come across error this would be helpful.
https://www.youtube.com/watch?v=psK7wdBo_SU

If you don't know how to do this, do exact same thing shown in video description or go to link (2).
1. https://www.youtube.com/watch?v=psK7wdBo_SU
2. https://tuxcoder.wordpress.com/2015/02/02/install-netcdf4-with-hdf5-in-ubuntu-linux/

Please remember the path where you install this as that is required for woss-ns3 installation. Please export the same path to your bashrc so it won't throw you error.



Steps to install WOSS-NS3 :
Please perform following commands :

mkdir workspace
cd workspace
wget https://www.nsnam.org/release/ns-allinone-3.30.tar.bz2
tar xjf ns-allinone-3.30.tar.bz2
cd ns-allinone-3.30/
./build.py --enable-examples --enable-tests
cd ns-3.30/
./waf --run scratch/scratch-simulator #To check everything working fine 
which bellhop.exe # Please make sure bellhop.exe is in your bashrc or PATH, This should return you bellhop.exe path in your computer

-download woss-ns3 module from github from - https://github.com/MetalKnight/woss-ns3.git (Please use this as this is stable version without error).

-untar that and copy all the model files to ns3.30/src folder and make sure the folder name should be woss-ns3

-in order to compile custom woss-ns3 code launched from the ns-3 "scratch" directory, due to a unresolved warning in the NetCDF-C++4 v4.3.1 library, it is recommended to manually remove the line 18 from the <netcdf4_and_hdf5_installed_path>/include/ncGroup.h file

Please configure your ns again, please go to your ns-3.30 folder and do this :

./waf -d debug --enable-tests --enable-examples --enable-sudo --with-woss-source=/home/jay/Downloads/netcdf-c-4.7.3/woss-1.9.0 --with-woss-library=/home/jay/netcdf/lib --with-netcdf4-install=/home/jay/netcdf configure

./waf build

This will take a while. I belive you follow all the installation instruction provided in http://telecom.dei.unipd.it/ns/woss/doxygen/installation.html

If you get this in the end, everything is fine:

Waf: Leaving directory `/home/jay/Documents/dev/workspace/ns-allinone-3.30/ns-3.30/build'
Build commands will be stored in build/compile_commands.json
'build' finished successfully (5m31.704s)

Modules built:
antenna                   aodv                      applications              
bridge                    buildings                 config-store              
core                      csma                      csma-layout               
dsdv                      dsr                       energy                    
fd-net-device             flow-monitor              internet                  
internet-apps             lr-wpan                   lte                       
mesh                      mobility                  mpi                       
netanim (no Python)       network                   nix-vector-routing        
olsr                      point-to-point            point-to-point-layout     
propagation               sixlowpan                 spectrum                  
stats                     tap-bridge                test (no Python)          
topology-read             traffic-control           uan                       
virtual-net-device        visualizer                wave                      
wifi                      wimax                     woss-ns3 (no Python)      

Modules not built (see ns-3 tutorial for explanation):
brite                     click                     openflow    

Please run the example file from the folder itself as it is still under development.

Please use this command to run the example.

./waf --run src/woss-ns3/examples/woss-aloha-example
Waf: Entering directory `/home/jay/Documents/dev/workspace/ns-allinone-3.30/ns-3.30/build'
[2572/2848] Linking build/src/woss-ns3/examples/ns3.30-woss-aloha-example-debug
Waf: Leaving directory `/home/jay/Documents/dev/workspace/ns-allinone-3.30/ns-3.30/build'
Build commands will be stored in build/compile_commands.json
'build' finished successfully (1.990s)
WossManagerResDbMT::checkConcurrentThreads() 24
WossManagerResDbMT::checkConcurrentThreads() 4
Received a packet of size 1000 bytes
Received a total of 1000 bytes at sink


APPENDIX A : VIDEO DESCRIPTION

== Building NetCDF with HDF5, use GNU Compiler ==
1. download source files

ftp://ftp.unidata.ucar.edu/pub/netcdf/

2. configure to install zlib, hdf5, netcdf
##### export environment
export F77=gfortran
export FC=gfortran
export CC=gcc
export CXX=g++
export CFLAGS=-fPIC

export fld_install=/home/nam/apps/netcdf #change this to your folder

##### install zlib
./configure --prefix=$fld_install; make clean; make all install
##### install hdf5
./configure --prefix=$fld_install --with-zlib=$fld_install; make clean; make all install
##### install netcdf
LDFLAGS=-L$fld_install/lib CPPFLAGS=-I$fld_install/include \
./configure --prefix=$fld_install; make clean; make all install

3. export variables to .bashrc
#NETCDF
export NETCDF=/home/nam/apps/netcdf
export PATH=$NETCDF/bin:$PATH
export NETCDF_INCDIR=$NETCDF/include 
export NETCDF_LIBDIR=$NETCDF/lib
export LD_LIBRARY_PATH=$NETCDF/lib:$LD_LIBRARY_PATH
export PATH NETCDF

Installation of netCDF cxx module:

./configure --prefix=/usr/local/netcdf4 --enable-shared CPPFLAGS="$CPPFLAGS -I/usr/local/netcdf4/include" LDFLAGS="$LDFLAGS -L/usr/local/netcdf4/lib"

./configure; make; sudo make install

Make sure you also install WOSS module as well. 

Please follow installation instructions from http://telecom.dei.unipd.it/ns/woss/doxygen/installation.html


    extract the compressed file in a directory of your choice and cd into that directory;
    run ./autogen.sh
    run ./configure with the following options:
        –with-ns-allinone=<ns2-allinone_path> optional, needed if you want to compile the library for NS2 and NSMIRACLE.
        –with-nsmiracle=<ns-miracle_path> optional, needed if you want to compile the library for NS2 and NSMIRACLE.
        –with-netcdf4=<NetCDF4_install_path> optional, needed if you want to use NetCDF4 + HDF5 databases. (The path is the same mentioned in the Requirements section, if NetCDF4 was installed with no –prefix, the default path SHOULD be /usr/local)
        –with-pthread optional but recommended
        –prefix=<path_where_libraries_will_be_installed>
        this path is optional and should be the same one used with NS-Miracle installation. If used, this path should be added to the environmental variable LD_LIBRARY_PATH. Please refer to NS-Miracle documentation for more info
    run make
    run make install
