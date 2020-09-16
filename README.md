rb1_base_sim
=============

Packages for the simulation of the RB-1 Base

<p align="center">
  <img src="https://robotnik.eu/wp-content/uploads/2020/05/Robotnik-RB-1_Base-01_.jpg" width="275" />
</p>


<h1> Packages </h1>

<h2>rb1_base_gazebo</h2>

This package contains the configuration files and worlds to launch the Gazebo environment along with the simulated robot.

<h2>rb1_base_sim_bringup</h2>

Launch files that launch the complete simulation of the robot/s.


<h1>Simulating RB-1 Base</h1>

1. Install the following dependencies:
  - [rb1_base_common](https://github.com/RobotnikAutomation/rb1_base_common/tree/melodic-devel) (melodic-devel)
  - [robotnik_msgs](https://github.com/RobotnikAutomation/robotnik_msgs) (master)
  - [robotnik_sensors](https://github.com/RobotnikAutomation/robotnik_sensors) (kinetic-devel)
  - [robotnik_base_hw_sim](https://github.com/RobotnikAutomation/robotnik_base_hw_sim) (master)
  - [robot_localization_utils](https://github.com/RobotnikAutomation/robot_localization_utils) (master)
  - [hector_gazebo](https://github.com/RobotnikAutomation/hector_gazebo/tree/melodic-devel) (melodic-devel)

    In the workspace install the packages dependencies:
    ```
    rosdep install --from-paths src --ignore-src -r -y
    ```

    ```bash
    sudo apt-get install ros-melodic-ros-control ros-melodic-joint-state-controller ros-melodic-effort-controllers ros-melodic-position-controllers ros-melodic-velocity-controllers ros-melodic-ros-controllers ros-melodic-gazebo-ros ros-melodic-gazebo-ros-control
    ```

2. Launch RB-1 Base simulation (1 robot by default, up to 3 robots): <br>
- RB-1 Base: <br>
  ```
  roslaunch rb1_base_sim_bringup rb1_base_complete.launch
  ```

  Optional general arguments:
  ```
  <arg name="launch_rviz" default="true"/>
  <arg name="gazebo_world" default="$(find rb1_base_gazebo)/worlds/rb1_base_office.world"/>
  ```
  Optional robot arguments:
  ```
  <!--arguments for each robot (example for robot A)-->
  <arg name="id_robot_a" default="robot"/>
  <arg name="launch_robot_a" default="true"/>
  <arg name="has_elevator_robot_a" default="true"/>
  <arg name="x_init_pose_robot_a" default="0.0" />
  <arg name="y_init_pose_robot_a" default="0.0" />
  <arg name="z_init_pose_robot_a" default="0.0" />
  <arg name="init_yaw_robot_a" default="0.0" />
  <arg name="gmapping_robot_a" default="false"/>
  <arg name="amcl_and_mapserver_robot_a" default="true"/>
  <arg name="map_frame_robot_a" default="$(arg id_robot_a)_map"/>
  <arg name="map_file_robot_a" default="$(find rb1_base_localization)/maps/willow_garage/willow_garage.yaml"/>
  <arg name="move_base_robot_a" default="true"/>
  <arg name="pad_robot_a" default="true"/>
  ```
- Example to launch simulation with 3 RB-1 Base robots:
  ```
  roslaunch rb1_base_sim_bringup rb1_base_complete.launch launch_robot_a:=true launch_robot_b:=true launch_robot_c:=true
  ```
- Example to launch simulation with 1 RB-1 Base robot with navigation and localization:
  ```
  roslaunch rb1_base_sim_bringup rb1_base_complete.launch launch_robot_a:=true move_base_robot_a:=true amcl_and_mapserver_robot_a:=true
  ```
- Example to launch simulation with 2 RB-1 Base robot with navigation and localization sharing the same global frame:
  ```
  roslaunch rb1_base_sim_bringup rb1_base_complete.launch launch_robot_a:=true amcl_and_mapserver_robot_a:=true move_base_robot_a:=true map_frame_a:=/map launch_robot_b:=true amcl_and_mapserver_robot_b:=true move_base_robot_b:=true map_frame_b:=/map
  ```
- Example to launch simulation with 3 RB-1 Base robot with navigation and localization sharing the same global frame:
```
roslaunch rb1_base_sim_bringup rb1_base_complete.launch launch_robot_a:=true amcl_and_mapserver_robot_a:=true move_base_robot_a:=true map_frame_a:=/map launch_robot_b:=true amcl_and_mapserver_robot_b:=true move_base_robot_b:=true map_frame_b:=/map launch_robot_c:=true amcl_and_mapserver_robot_c:=true move_base_robot_c:=true map_frame_c:=/map
```
3. Enjoy! You can use the topic `${id_robot}/robotnik_base_control/cmd_vel` to control the RB-1 Base robot:

```bash
rostopic pub /robot/robotnik_base_control/cmd_vel geometry_msgs/Twist "linear:
  x: 0.1
  y: 0.0
  z: 0.0
angular:
  x: 0.0
  y: 0.0
  z: 0.0" -r 10
```

or if you have launched move_base, you can send simple goals using `/${id_robot}/move_base_simple/goal`:
```bash
rostopic pub /robot/move_base_simple/goal geometry_msgs/PoseStamped "header:
  seq: 0
  stamp:
    secs: 0
    nsecs: 0
  frame_id: 'robot_map'
pose:
  position:
    x: 3.0
    y: 0.0
    z: 0.0
  orientation:
    x: 0.0
    y: 0.0
    z: 0.0
    w: 1.0" 
```


