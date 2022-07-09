# px4_sitl_gazebo_with_mavros

create custom drone on px4_sitl_gazebo, use depth cam,fpv cam with ros.

# install px4 firmware
[px4 github](https://github.com/PX4/PX4-Autopilot)   
[px4 user guide](https://docs.px4.io/main/en/dev_setup/dev_env_linux_ubuntu.html)


    git clone https://github.com/PX4/PX4-Autopilot.git --recursive
    bash ./PX4-Autopilot/Tools/setup/ubuntu.sh

first build

    cd PX4-Autopilot
    make px4_sitl gazebo


# install QGC

[QGC Download Guide](https://docs.qgroundcontrol.com/master/en/getting_started/download_and_install.html)   

    sudo usermod -a -G dialout $USER
    sudo apt-get remove modemmanager -y
    sudo apt install gstreamer1.0-plugins-bad gstreamer1.0-libav gstreamer1.0-gl -y
    sudo apt install libqt5gui5 -y

[Download QGroundControl.AppImage.](https://d176tv9ibo4jno.cloudfront.net/latest/QGroundControl.AppImage)   

    chmod +x ./QGroundControl.AppImage
    
turn on QGC

    ./QGroundControl.AppImage
    
    
# import custom model in px4 firmware

    cd
    git clone https://github.com/cananella/px4_sitl_gazebo_with_mavros.git
    
