# Zebra Take Home Coding Challenge
This is the take-home assignment for the Simulation Software Engineering Position (Robotics) at Zebra Technologies

# Software Dependencies
- ROS Kinetic
- Ubuntu 16.04
- Gazebo 7
- RVIZ

# Task 1
Build a simulated warehouse using gazebo (version 7 or higher) or using ROSDS. Add 9 blue shelves that are 10 meter x 2 meter x 2 meter. Be sure to include a floor and some lights. Warehouse dimensions 40 meter x 20 meter.

1. Provide (2) pictures of the map:
	1. Top View: Can be found in "./worlds/warehouse_top_view.png"
![Top View Image](https://github.com/varunsampat30/zebra_coding_challenge/blob/main/worlds/warehouse_top_view.png?raw=true)
	2. Isometric View: Can be found in "./worlds/warehouse_isometric_view.png" 
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
- Modify the freight robot to include a 3D depth camera at the front of the robot, provide the modified file:
	- (MS Kinect, code for which can be found in the kinect_ros folder) 
- Run it again providing an image from RVIZ. Make sure the sensor data from the 3D depth camera is being displayed. 

2. While demo.launch is running, use `rosbag record` to record a bag file of topics that you think are important for pathplaning, mapping and testing. (Hint: `rostopic list` to see all the topics). Provide the bag file you recorded:

	- **Path Planning**: Path planning is the problem that deals with the generation of a path that would needed to be taken to reach a destination point on the map, given some conditions (robot pose, etc). For path planning, an occupancy grid would be needed, along with the current coordinates (or estimate) of the robot in the map frame. `/map` (& `/map_metadata`) along with the current coordinates of the robot in the map frame would suffice for an path-planning algorithm like A*. By default, `tf/` does not provide the direct transformation between map and base_link, but that can be acquired either by setting up a tf_listener and looking up the transform between `map` and `base_link` OR by direct calculation through tf: `odom` -> `base_link` and the odom coordinates. Hence, the required topics are `/map`, `/map_metadata`, `tf/` and `odom/`. The output bagfile with this can be found in **./bagfiles/path_planning.bag**
	
	- **Mapping**: Assuming 'mapping' refers to the task of mapping an unknown environment. SLAM Algorithms such as Pose-Map optimization require Laser/Camera readings and odometry values. Hence the topics of interest become: `/map`, `/map_metadata`, and `/odom`. Additionally, transformations between the map frame, odom frame and base_link would be needed, for which `tf/` would be needed.  The output bagfile with this can be found in **./bagfiles/mapping.bag**. If mapping refers to just localization in a known map (Active Monte-Carlo Localization), then topics such as `/particlecloud` or `/amcl_pose` could be added to the bag as well. 
	
	- **Testing**: Testing a robot in a simulation environment would also depend on the task being performed. Assuming the robot would be moved manually, the commands from the teleop operation would be necessary to trace/debug the steps taken by the robot and the consequent actions. If the robot is being driven by another high-level controller (probably defined by us, as programmers) then the published velocity (odom).  Overall, the occupancy grid (`/map` and `map_metadata`), velocity of the robot (`/odom`) would be useful. The output bagfile with this can be found in **./bagfiles/testing.bag**
	- 
3. Run the modified freight robot in Part 2.1.2 in the world file you created in Part 1.
	- Provide (2) pictures of the map: one from above and one isometric view.
	- Use teleop_twist to move the robot and provide a recorded rosbag of the manual drive
  `rosrun teleop_twist_keyboard teleop_twist_keyboard.py`
