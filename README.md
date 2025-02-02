# sdk_manager_docker

## Introduction

This is a Dockerfile to use [NVIDIA SDK Manager](https://docs.nvidia.com/sdk-manager/) on Docker container.

## Important Information

NVIDIA released official Docker image(<https://docs.nvidia.com/sdk-manager/docker-containers/index.html>).

## Requirements

* Docker

## Preparation

### Download NVIDIA SDK Manager

Please download the package of NVIDIA SDK Manager from <https://developer.nvidia.com/nvidia-sdk-manager>.  
And, please put the package of NVIDIA SDK Manager in the same directory as the Dockerfile.  
This time, I used `sdkmanager_1.8.1-10392_amd64.deb`.

### Build Docker image

```
docker build --build-arg GID=$(id -g) --build-arg UID=$(id -u) -t jetpack .
```

To build a Docker image with a specific SDK Manager version override the ``SDK_MANAGER_VERSION`` variable in the Docker command line

```
docker build --build-arg SDK_MANAGER_VERSION=1.8.0-10363 --build-arg GID=$(id -g) --build-arg UID=$(id -u) -t jetpack .
```

### Create Docker container

```
./launch_container.sh
```

## Launch NVIDIA SDK Manager

Please launch NVIDIA SDK Manager by the following command.

```
sdkmanager --cli install --logintype devzone --product Jetson --version 4.5.1 --targetos Linux --host --target JETSON_TX2_NX --flash all --additionalsdk 'DeepStream 5.1'


sdkmanager --cli install --logintype devzone --product Jetson --target JETSON_TX2_NX --targetos Linux --version 4.5.1 --select 'Jetson OS' --deselect 'Jetson SDK Components' --flash all --license accept --staylogin true --datacollection enable --exitonfinish

```

Please refer to <https://docs.nvidia.com/sdk-manager/install-with-sdkm-jetson/index.html>.
And, I tested in the following setting.

* Manual Setup
* OEM Configuration: Runtime

![](image/jetson-os-flash-setting.png)

## Notes

If you get errors like this, it means that QEMU is not installed in host/container

![](image/filesystem-error.png)

To configure QEMU, run below, either on host, or the running container.

```shell
./configure_qemu.sh
```
