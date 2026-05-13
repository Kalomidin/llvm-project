docker pull 
# Instructions
1. To launch as docker image, run following command:
 ```bash docker run --privileged -it --rm --platform linux/arm64 --name cuda-13.2-arm -v /Users/kalomidin/Documents/compiler:/home/compiler nvidia/cuda:13.2.0-cudnn-devel-ubuntu24.04
 ``` Or in case you create docker compose with necessary packages, u can do this: ```bash docker run -it \ --platform linux/amd64 \ --name cuda-13.2 \ -v /Users/kalomidinklichev/Documents/HipIFY:/home/HipIFY \ -v /Users/kalomidinklichev/Documents/llvm-project:/home/llvm-project \ -v /Users/kalomidinklichev/Documents/hipify_build/build:/home/llvm-build \ a2a66cc2aed3 ```
2. To get version of the linux:
 ``` ```
3. Update apt packages:
 ```bash apt-get update ```
4. Install necessary packages:
 ```bash apt-get install -y cmake ninja-build git python3 clang lld ```
5. Install llvm-project:
 ```bash cmake -G Ninja \ -DCMAKE_INSTALL_PREFIX=../build_llvm_install \ -DLLVM_TARGETS_TO_BUILD="AArch64;ARM" \ -DCMAKE_OSX_ARCHITECTURES="arm64" \ -DLLVM_ENABLE_PROJECTS="clang" \ -DLLVM_INCLUDE_TESTS=OFF \ -DCMAKE_BUILD_TYPE=Release \ -DCMAKE_C_COMPILER=/usr/bin/clang \ -DCMAKE_CXX_COMPILER=/usr/bin/clang++ \ ../../llvm ```
6. Install hipify:
 ```bash cmake -G Ninja -DCMAKE_INSTALL_PREFIX=$(pwd)/../ascify_install -DCMAKE_BUILD_TYPE=Release \ -DCMAKE_PREFIX_PATH=$(pwd)/../../../llvm-project/build_docker/build \ -DCMAKE_PREFIX_PATH=$(pwd)/../../../llvm-project/clang ../../ ```
7. Test:
 ```bash ./build-hipify/hipify-clang a.cu --clang-resource-directory=/home/build-llvm/lib/clang/23 ```
## Helpful tips
- To go inside a docker image:
 ```bash docker exec -it cuda-13.2 bash ```

- First install llvm-project for macos.- Install hipify for macos- Problem: We can not install nvidia toolkit for macos. But we can use linux-arm64 docker image. Download and run the docker with mounting: - llvm project build(do we need to mount it? maybe to lookup for clang) -*need to rebuild for docker image llvm project and also hipify* - hipify-build(expected to work as it is built for arm64)- Work on hipify-build -> Test on the docker image