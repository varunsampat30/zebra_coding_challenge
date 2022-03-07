# Zebra Take Home Coding Challenge
This is the take-home assignment for the Simulation Software Engineering Position (Robotics) at Zebra Technologies

# Software Dependencies
- ROS Kinetic
- Ubuntu 16.04
- Gazebo 7
- RVIZ

# Task 1
Build a simulated warehouse using gazebo (version 7 or higher) or using ROSDS. Add 9 blue shelves that are 10 meter x 2 meter x 2 meter. Be sure to include a floor and some lights. Warehouse dimensions 40 meter x 20 meter.

1. Provide (2) pictures of the map (note this was modified because one of the shelves overlaps on the origin, which is undesirable):
	1. Top View: Can be found in "./worlds/warehouse_top_view.png"
![Top View Image](https://github.com/varunsampat30/zebra_coding_challenge/blob/main/worlds/warehouse_top_view.png?raw=true)
	2. Isometric View: Can be found in "./worlds/warehouse_isometric_view.png" (added 4 racks for shelf)
![Isometric View Image](https://github.com/varunsampat30/zebra_coding_challenge/blob/main/worlds/warehouse_isometric_view.png?raw=true)
2. Provide the world file for the map: The .world file can be found in ./worlds/warehouse.world


# Task 2
The part will use the repository https://github.com/fetchrobotics/fetch_gazebo. Follow the tutorial and answer the questions below. There will be some tasks beyond the tutorial. 
1. When you are running the freight robot simulation:
For this, I ran the following commands in seperate terminals:
  `roslaunch fetch_gazebo playground.launch robot:=freight`  
  `roslaunch fetch_gazebo_demo demo.launch`  
  `rosrun rviz rviz`
- Provide an image of rviz with the freight robot: 
	- Image of RVIZ![RVIZ Image](https://github.com/varunsampat30/zebra_coding_challenge/blob/main/figs/freight_robot_rviz.png?raw=true)
	- Additionally, this is what the playground looks like on Gazebo:
![Playground Gazebo Image](https://github.com/varunsampat30/zebra_coding_challenge/blob/main/figs/playground_launch.png?raw=true)
- Modify the freight robot to include a 3D depth camera at the front of the robot, provide the modified 
    - Adding a 3D depth camera: Code for the Kinect can be found in `./kinect_ros` (copied to `/opt/ros/kinetic/share/fetch_description/sensors`)
    - Modified URDF File can be found in `./frieghtRobot/freight_robot_w_camera.urdf` (copied to `/opt/ros/kinetic/share/fetch_description/robots`)
    - Modified XACRO File can be found in `./freightRobot/freight_robot_w_camera.gazebo.xacro` (copied to `/opt/ros/kinetic/share/fetch_gazebo/robots/`)
    - Launch file for the modified frieght robot (with the 3D Camera) can be found in `.freightRobot/freightDepthCamera.launch.xml` (copied to `/opt/ros/kinetic/share/fetch_gazebo/launch/include`)
- Run it again providing an image from RVIZ. Make sure the sensor data from the 3D depth camera is being displayed. 

2. While demo.launch is running, use `rosbag record` to record a bag file of topics that you think are important for pathplaning, mapping and testing. (Hint: `rostopic list` to see all the topics). Provide the bag file you recorded:

	- **Path Planning**: Path planning deals with generating a path that is needed to be taken in order to reach a destination point on a given map. For path planning, an occupancy grid (or any alternative way to represent the map) would be needed, along with the current coordinates (or estimate) of the robot in the map frame. `/map` (& `/map_metadata`) along with the current coordinates of the robot in the map frame would suffice for a path-planning algorithm like A\*. By default, `tf/` does not provide the direct transformation between map and base_link, but that can be acquired either by setting up a tf_listener and looking up the transform between `map` and `base_link` OR by direct calculation through tf: `odom` -> `base_link` and the odom coordinates. Hence, the required topics are `/map`, `/map_metadata`, `tf/` and `odom/`. The output bagfile with this can be found in **./bagfiles/path_planning.bag**
	
	- **Mapping**: 
	    -  Assuming 'mapping' refers to the task of mapping an unknown environment. SLAM Algorithms such as Pose-Map optimization require Laser/Camera readings and odometry values. If SLAM Algorithms such as Pose-Map optimization are to be implemented, then `/base_scan`, `/odom`, and `/tf` would be topics of interest, where `/base_scan` represents the laser data, `/odom` represents data from the odometry, and `tf/` provides transformations between the odom frame and base link frame. The output bagfile for this can be found in **./bagfiles/mapping_raw.bag**. 
	    -  However, if already implemented SLAM algorithms are to be leveraged, the occupancy grid (or any map) can be directly tracked. In that case, the topics of interest become: `/map`, `/map_metadata`, and `/odom`. Additionally, `tf/` would be needed for the same reason mentioned before. The output bagfile for this can be found in **./bagfiles/mapping.bag**. 
	    -  If mapping refers to just localization in a known map (Active Monte-Carlo Localization), then topics such as `/particlecloud` or `/amcl_pose` could be tracked using `rosbag`. 
	
	- **Testing**: Testing a robot in a simulation environment would also depend on the task being performed. Assuming the robot would be moved manually, the commands from the teleop operation (`/cmd_vel`) would be necessary to trace/debug the steps taken by the robot and the consequent actions. If the robot is being driven by another high-level controller (probably defined by us, as programmers) then the published velocity (odom).  Overall, the occupancy grid (`/map` and `map_metadata`) and velocity of the robot (`/odom`) would be useful.  The output bagfile with this can be found in **./bagfiles/testing.bag**. Other additions could include important sensor data such as bumpers that may help debug the testing aspect further. 

3. Run the modified freight robot in Part 2.1.2 in the world file you created in Part 1.
	- Provide (2) pictures of the map: one from above and one isometric view:
		- Top View:  
![Playground Gazebo Image](https://github.com/varunsampat30/zebra_coding_challenge/blob/main/figs/manual_drive_start.png?raw=true)
		- Isometric View: ![Playground Gazebo Image](https://github.com/varunsampat30/zebra_coding_challenge/blob/main/figs/manual_drive_isometric_view.png?raw=true)
	    - Note: the `warehouse.world` was copied to the `/opt/ros/kinetic/share/fetch_gazebo/worlds/` directory (`sudo cp /PATH/TO/THIS/FILE /opt/ros/kinetic/share/fetch_gazebo/worlds/`)
	    - Note: the `warehouse.launch` was copied to the `/opt/ros/kinetic/share/fetch_gazebo/launch/` directory
	    - With that, I could directly run `roslaunch fetch_gazebo warehouse.launch robot:=freightDepthCamera`
	- Use teleop_twist to move the robot and provide a recorded rosbag of the manual drive:
	    - For this, I first ran `rosrun teleop_twist_keyboard teleop_twist_keyboard.py`. 
	    - Once I had the teleop setup, I set up a rosbag command: `rosbag record -O manual_drive.bag /cmd_vel`
	    - The output bagfile for this can be found in **./bagfiles/manual_drive.bag**. Finally, here is the top view of the moved freight robot. 
	   ![Playground Gazebo Image](https://github.com/varunsampat30/zebra_coding_challenge/blob/main/figs/manual_drive_end.png?raw=true)
