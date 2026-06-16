# Create software image
```
podman pull docker.io/osrf/ros:jazzy-desktop-full
podman run -it --rm --name super_lio docker.io/osrf/ros:jazzy-desktop-full
```
```
   apt update && apt -y upgrade
   apt install libgoogle-glog-dev libtbb-dev
   git clone https://github.com/qza36/Livox-SDK2.git
   cd Livox-SDK2/
   mkdir build && cd build
   cmake .. && make -j && make install

   mkdir -p /ros2_ws/src
   cd /ros2_ws/src/
   git clone https://github.com/Ericsii/livox_ros_driver2.git ws_livox/src/livox_ros_driver2
   cd .. && colcon build --symlink-install

   cd /ros2_ws/src/
   git clone https://github.com/ecervera/Super-LIO.git
   cd ..
   colcon build --symlink-install
```
```
podman commit super_lio ghcr.io/ecervera/super_lio:latest
podman push ghcr.io/ecervera/super_lio:latest
```

# Run container
```
xhost +local:$USER
podman run -it --rm --name super_lio \
    --env="DISPLAY=$DISPLAY" \
    --env="QT_X11_NO_MITSHM=1" \
    --volume="/tmp/.X11-unix:/tmp/.X11-unix:rw" \
    --device /dev/dri \
    ghcr.io/ecervera/super_lio:latest
```
