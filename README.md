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
    cd px4_sitl_gazebo_with_mavros
    mv my_model ~/PX4-Autopilot/Tools/sitl_gazebo/model/my_model
    
 PX4_Autopilot/ROMFS/px4fmu_commom/init.d-posix/airframes  
 안에 1011_iris_irlock 파일을 복사한후 <겹치지않는 4자리수>_my_model 로 변경   
 
 PX4_Autopilot/build/px4_sitl_default/etc/init.d-posix/airframes   
 안에 1011_iris_irlock 파일을 복사한후 <겹치지않는 4자리수>_my_model 로 변경 
 
 PX4_Autopilot/platforms/posix/cmake 안의 sitl_target.cmake 파일 수정    
 set(models 마지막에 my_model 삽입
 
    cd ~/PX4_Autopilot
    make px4_sitl gazebo_my_model
    
# import custom world in px4 firmware

    cd ~/px4_sitl_gazebo_with_mavros
    mv armarker ~/PX4-Autopilot/Tools/sitl_gazebo/model/armarker
    mv landing_point ~/PX4-Autopilot/Tools/sitl_gazebo/model/landing_point
    mv 3rd_building ~/PX4-Autopilot/Tools/sitl_gazebo/model/3rd_building
    mv truck_1 ~/PX4-Autopilot/Tools/sitl_gazebo/model/truck_1
    mv my_world.world ~/PX4-Autopilot/Tools/sitl_gazebo/worlds/my_world.world
    
test
    
    cd ~/PX4-Autopilot
    make px4_sitl gazebo_my_model PX4_SITL_WORLD=my_world


# mavros를 이용한 시뮬

ros 와 mavros 패키지 필요
    sudo apt-get install ros-<rosversion>-mavros
    
launchfile

    cd ~/px4_sitl_gazebo_with_mavros
    mv mavros_posix_sitl_my_model.launch ~/PX4-Autopilot/launch/mavros_posix_sitl_my_model.launch


    cd ~/PX4-Autopilot  (px4 펌웨어 디렉토리)
    DONT_RUN=1 make px4_sitl_default gazebo 
    source Tools/setup_gazebo.bash $(pwd) $(pwd)/build/px4_sitl_default 
    export ROS_PACKAGE_PATH=$ROS_PACKAGE_PATH:$(pwd) 
    export ROS_PACKAGE_PATH=$ROS_PACKAGE_PATH:$(pwd)/Tools/sitl_gazebo 
    
매번 배쉬에서 실행해줘야함 혹은 ~/.bashrc 마지막줄에 추가
    
    cd ~/PX4-Autopilot
    DONT_RUN=1 make px4_sitl_default gazebo > /dev/null
    source Tools/setup_gazebo.bash $(pwd) $(pwd)/build/px4_sitl_default > /dev/null
    export ROS_PACKAGE_PATH=$ROS_PACKAGE_PATH:$(pwd) > /dev/null
    export ROS_PACKAGE_PATH=$ROS_PACKAGE_PATH:$(pwd)/Tools/sitl_gazebo > /dev/null
    cd
    
       
실행

   roslaunch px4 mavros_posix_sitl_my_model.launch
   

rqt 로 카메라 topic 확인

    rqt_image_view
    
cam topic => /camera/rgb/image_raw
depth topic => /camera/depth/image_raw


 

