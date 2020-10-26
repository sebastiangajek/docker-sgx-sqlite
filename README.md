## Quick Start

###  Prerequisites
1. Install [Docker and Compose](https://docs.docker.com/) and configure them properly following their respective installation guide.
2. Install SGX driver. **Tested with:** https://download.01.org/intel-sgx/sgx-linux/2.8/distro/ubuntu18.04-server/


### Run with Docker Compose
This will start AESM and the SGX_SQLite command line interface.
```
$ sudo docker-compose run sqlite
```

## Legacy Launch Control driver and kernel for SGX

All SGX applications need access to the SGX device nodes exposed by the kernel space driver. Depending on the driver or kernel you are using, the SGX device nodes may have different names and locations. Therefore, you need to ensure those nodes are mapped and mounted inside the containers properly.


[SGX kernel patches](https://github.com/jsakkine-intel/linux-sgx/commits/master) are still in process of upstreaming.
The [Flexible Launch Control driver](https://github.com/intel/SGXDataCenterAttestationPrimitives/tree/master/driver) is developed to imitate the kernel patches as closely as possible.

The sample scripts and Compose files are compatible with the Flexible Launch Control driver or a custom built kernel with SGX support. If you need to use the Legacy Launch Control driver then you need to make following modifications:
1. Replace "/dev/sgx/enclave" device with "/dev/isgx" and **remove** "/dev/sgx/provision" device for AESM in docker-compose.yml and build_and_run_aesm_docker.sh
2. Replace "/dev/sgx/enclave" with "/dev/isgx" for the sample container in docker-compose.yml and build_and_run_sample_docker.sh

**Note**: When you switch between drivers, make sure you uninstall the previous driver and reboot the system before installing the other one.

**Note**: Earlier versions of the Flexible Launch Control driver and kernel patches may expose the SGX device as a single node at "/dev/sgx".
