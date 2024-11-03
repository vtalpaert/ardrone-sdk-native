
# ARDrone SDK

## Install

### Solving dependencies

```bash
sudo apt install repo  autoconf  libavahi-client-dev  libavcodec-dev  libavformat-dev  libswscale-dev
```

#### Python

Note that the build fails for python3.10 and requires to downgrade to python3.7. To avoid messing with our system, we use a virtual environment.
Several solutions are possible: in a docker image, using a third party apt repository for python3.7 or rebuilding using pyenv.

Here is my solution using the third party ppa, which you should only use at your own risk.
```bash
sudo add-apt-repository ppa:deadsnakes/ppa
sudo apt update
sudo apt install python3-virtualenv python3.7
virtualenv venv -p python3.7
source venv/bin/activate
```

## Build the sdk

```bash
mkdir arsdk && cd arsdk
repo init -u https://github.com/Parrot-Developers/arsdk_manifests.git
repo sync
./build.sh -p native -t build-sdk -j 3
```

## Build samples

```bash
./build.sh -p native -tt # list all tasks for native platform
./build.sh -p native -t build-sample-JumpingSumoSample -j 3
```

For the JumpingSumo, the interesting files are in `arsdk/packages/Samples/Unix/JumpingSumoSample` and are installed to `out/arsdk-native/staging/usr/bin/JumpingSumoSample`
