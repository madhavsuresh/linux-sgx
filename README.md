Intel(R) Software Guard Extensions for Linux\* OS
================================================

# linux-sgx

Introduction
------------
Intel(R) Software Guard Extensions (Intel(R) SGX) is an Intel technology for application developers seeking to protect select code and data from disclosure or modification.

The Linux SGX software stack is comprised of the SGX driver, the SGX SDK, and the SGX Platform Software. The SGX SDK and SGX PSW are hosted in the [linux-sgx](https://github.com/01org/linux-sgx) project.

The [linux-sgx-driver](https://github.com/01org/linux-sgx-driver) project hosts the out-of-tree driver for the Linux SGX software stack, which will be used until the driver upstreaming process is complete. 

License
-------
See License.txt for details.

Contributing
-------
See CONTRIBUTING.md for details.

Documentation
-------------
- [Intel(R) SGX for Linux\* OS][1] project home page on [01.org](http://01.org)
- [Intel(R) SGX Programming Reference][2]
[1]: https://01.org/intel-softwareguard-extensions
[2]: https://software.intel.com/sites/default/files/managed/48/88/329298-002.pdf

Build and Install the Intel(R) SGX Driver
-----------------------------------------
Follow the instructions in the [linux-sgx-driver](https://github.com/01org/linux-sgx-driver) project to build and install the SGX driver.

Build the Intel(R) SGX SDK and Intel(R) SGX PSW Package
-------------------------------------------------------
###Prerequisites:
- Ensure that you have the following required operating systems:  
  Ubuntu\* Desktop-14.04-LTS 64bits
- Use the following command to install the required tools to build Intel(R) SGX SDK:  
```
  $ sudo apt-get install build-essential ocaml automake autoconf libtool
```
- Use the following command to install additional required tools to build Intel(R) SGX PSW:  
```
  $ sudo apt-get install libcurl4-openssl-dev protobuf-compiler protobuf-c-compiler libprotobuf-dev libprotobuf-c0-dev
```
- Use the script `download_prebuilt.sh` inside source code package to download prebuilt binaries to prebuilt folder  
  You may need set http proxy for wget tool used by the script (such as `export http_proxy=http://test-proxy:test-port`)  
```
  $ ./download_prebuilt.sh
```

###Build the Intel(R) SGX SDK and Intel(R) SGX PSW
The following steps describe how to build the Intel SGX SDK and PSW. You can build the project according to your requirement.  
- To build both Intel SGX SDK and PSW with default configuration, enter the following command:  
  You can find the tools and libraries generated in the `build/linux` directory.  
  **Note**: You can also go to the sdk folder and use the `make` command to build the Intel SGX SDK component only. However, the building of PSW component is dependent on the building result of Intel SGX SDK.  
```
  $ make  
```  

- To build Intel SGX SDK and PSW with debug information, enter the following command:  
```
  $ make DEBUG=1
```
- To clean the files generated by previous `make` command, enter the following command:  
```
  $ make clean
```

###Build Intel(R) SGX SDK Installer
To build Intel(R) SGX SDK installer, enter the following command:
```
$ make sdk_install_pkg
```
You can find the generated Intel SGX SDK installer `sgx_linux_x64_sdk_${version}.bin` located under `linux/installer/bin/`, where `${version}` refers to the version number.

###Build Intel(R) SGX PSW Installer
To build Intel(R) SGX PSW installer, enter the following command:
```
$ make psw_install_pkg
```
You can find the generated Intel SGX PSW installer `sgx_linux_x64_psw_${version}.bin` located under `linux/installer/bin/`, where `${version}` refers to the version number.

Install Intel(R) SGX SDK
------------------------
###Prerequisites
- Ensure that you have the following required operating systems:  
  Ubuntu\*-14.04-LTS
- Use the following command to install the required tool to use Intel(R) SGX SDK:
```  
  $ sudo apt-get install build-essential
```

###Install Intel(R) SGX SDK
To install Intel(R) SGX SDK, execute the installer with root privilege:
```
$ cd linux/installer/bin
$ sudo ./sgx_linux_x64_sdk_${version}.bin 
```
###Test Intel(R) SGX SDK Package with the Sample Codes
- Copy the sample codes installed by Intel(R) SGX SDK package into your work folder, such as  
```
  $ cp -r /opt/intel/sgxsdk/SampleCode ~
```
- Compile and run each sample codes in the simulation mode to make sure the package works well.  
```
  $ cd SampleCode/LocalAttestation
  $ make
  $ ./app
```
   Use similar commands for other sample codes.

###Compile and Run the Sample Codes in the Hardware Mode
If you use an SGX hardware enabled machine, you need to run the sample codes in the hardware mode.
Ensure that you install SGX driver and Intel(R) SGX PSW installer on the machine.  
See the topic, Install Intel(R) SGX PSW, on how to install the PSW package.
- Copy the sample codes installed by the Intel(R) SGX SDK package into your work folder, such as  
```
  $ cp -r /opt/intel/sgxsdk/SampleCode ~
```
- Compile and run each sample codes in the debug mode.  
```
  $ cd SampleCode/LocalAttestation
  $ make SGX_MODE=HW SGX_DEBUG=1
  $ ./app
```
   Use similar commands for other sample codes.

Install Intel(R) SGX PSW
------------------------
###Prerequisites
- Ensure that you have the following required operating systems:  
  Ubuntu\*-14.04-LTS 64bits
- Ensure that you have the following required hardware:  
  6th Generation Intel(R) Core(TM) Processor (code named Skylake)
- Configure the system with the **SGX hardware enabled** option and install SGX driver in advance.  
  See the topic, Build and Install the Intel(R) SGX Driver, on how to install the SGX driver.
- Install the library using the following command:  
```
  $ sudo apt-get install libcurl4-openssl-dev libprotobuf-dev libprotobuf-c0-dev
```

###Install Intel(R) SGX PSW
To install Intel(R) SGX PSW, execute the installer with root privilege:  
```
$ cd linux/installer/bin
$ sudo ./sgx_linux_x64_psw_${version}.bin
```

###Start or Stop aesmd Service
The Intel(R) SGX PSW installer installs an aesmd service in your machine which is running in a special linux account aesmd.  
To stop the service: `$ sudo service aesmd stop`  
To start the service: `$ sudo service aesmd start`  
To restart the service: `$ sudo service aesmd restart`

###Configure the Proxy for aesmd Service
The aesmd service uses HTTP protocol to initialize some services.  
If proxy is required for HTTP protocol, you may need manually setup the proxy for aesmd service.  
You should manually edit file `/etc/aesmd.conf` (refer the comment in the file) to set the proxy for aesmd service.  
After you configure the proxy, you need to restart the service to enable the proxy.
