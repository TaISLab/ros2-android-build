# ros2-android-build

Docker system to build [rcljava](https://github.com/ros2-java/ros2_java) for Android
and its use on ros2-android apps.  

This repo just explains a bit the install procedure.
For more info see:
- [rcljava](https://github.com/ros2-java/ros2_java)
- [original repo](https://github.com/YasuChiba/ros2-android-build)
- [nicholaslu version](https://github.com/nicholaslu/ros2-android-build)

## Version & Environment

Current environment:
- NDK:  android-ndk-r23b
- ABI: arm64-v8a  
- Android API Level(minSdkVersion): android-24

Modify [Dockerfile](./Dockerfile) file to change these.

## ROS2 java version  

ROS2 Humble is selectd for the docker image. Modify [repos](./ros2_java_android.repos) file for other options.


## Installation

### 1. Dependencies
See [this](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-22-04) tutorial on installing Docker on your system. Or just type this:
```
sudo apt update
sudo apt install git apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt update
sudo apt install docker-ce
sudo usermod -aG docker ${USER}
su - ${USER}
```

### 2. Clone repository

```
git clone https://github.com/TaISLab/ros2-android-build
cd ros2-android-build/
```

### 3. Build docker image

```
docker build -t ros2java-android-build ./
```

## Usage

There is a python script to init library compilation:

```
cd ros2-android-build/
python3 run.py ./out/soOut ./out/jarOut --srcDir ../src 
```

The script takes up to three arguments:

- `./out/soOut` : Android project folder to copy `.jar` files
- `./out/jarOut`: Android project folder to copy `.so` files  
- `--srcDir ../src` : (Optional) source folder for packages other than ros2 java to be included in your Android app, for example your own message packages. 


Usually, `.jar` files are in folder `app/libs/rcljava` folder and `.so` files in `app/src/main/jniLibs/arm64-v8a` folder from your Android app.

Remember to also add the line `implementation fileTree(include: ['*.jar'], dir: 'libs')` to `dependencies{}` of `app/build.gradle` file. 


## Example app   
YasuChiba has a repo with an example project [here](https://github.com/YasuChiba/ros2-android-test-app).
This project has the following structure:  
```
app
 -libs
    - .jar files go here!
 - src
    - main
        - java
        - jniLibs
            - arm64-v8a
                - .so files go here!
        - res
        - AndroidManifest.xml

```

