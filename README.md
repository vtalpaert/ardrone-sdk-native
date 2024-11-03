
# ARDrone SDK

Build a tarball of ARDroneSDK3 using https://github.com/Parrot-Developers/arsdk_manifests/

## Build SDK locally

### Solving dependencies

```bash
sudo apt install -y repo  autoconf  libavahi-client-dev  libavcodec-dev  libavformat-dev  libswscale-dev
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

### Build the SDK

[This is the exact SDK version used](https://github.com/Parrot-Developers/arsdk_manifests/tree/4b50e865427e6f2f2f19be1d4c55ab87a03b804a), it corresponds to the tag `ARSDK3_version_3_14_0` or ARSDK 3.14.0

```bash
mkdir arsdk && cd arsdk
repo init -u https://github.com/Parrot-Developers/arsdk_manifests.git
repo sync
./build.sh -p native -t build-sdk -j 3
```

### Build samples

```bash
./build.sh -p native -tt # list all tasks for native platform
./build.sh -p native -t build-sample-JumpingSumoSample -j 3
./build.sh -p native -t build-sample-BebopSample -j 3
```

For the JumpingSumo, the interesting files are in `arsdk/packages/Samples/Unix/JumpingSumoSample` and are installed to `out/arsdk-native/staging/usr/bin/JumpingSumoSample`

### Bundle and cleanup

```bash
deactivate # if your virtualenv was still active
cd .. # return above arsdk folder
rm -r venv # if using venv
TAR_DIR=arsdk/out/arsdk-native/staging/
TAR_NAME=arsdk-native-samples-master.tar.gz
find $TAR_DIR -printf "%P\n" | tar -cvzf $TAR_NAME --no-recursion -C $TAR_DIR -T -
# Equivalent to tar cvzf arsdk-native-samples-master.tar.gz -C arsdk/out/arsdk-native/staging but without a ./ parent directory
```
