# Robotic Navigation and Planning with ROSPlan

## Description
This project leverages a ROSPlan-based system to implement robotic planning in a Gazebo simulation environment. A mobile robot equipped with a camera and laser scanner navigates a set of predefined waypoints, detects ArUco markers at each location, and ultimately moves to the waypoint associated with the marker with the lowest ID. The system integrates planning, localization, and obstacle avoidance using PDDL (Planning Domain Definition Language) and ROS packages.

## Key Functionalities
- **Autonomous Navigation:** The robot travels between predefined waypoints:
  - **WP0:** `x = -7.0, y = 1.5`
  - **WP1:** `x = -3.0, y = -8.0`
  - **WP2:** `x = 6.0, y = 2.0`
  - **WP3:** `x = 7.0, y = -5.0`
  - **WP4 (initial position):** `x = 0, y = 2.75`
  
- **Marker Detection:** Identifies ArUco markers at waypoints, determines the marker with the lowest ID, and adjusts the robot's goal accordingly.

- **Planning Integration:** Utilizes PDDL-based definitions for planning and automated task execution.

- **Obstacle Avoidance:** Employs SLAM for mapping and localization to navigate safely around obstacles.

---

## System Setup and Execution

1. **Clone the Repository:**
   ```bash
   git clone https://github.com/YourRepoLink/RoboticsPlanningProject.git
   ```

2. **Install Required Packages:**
   - Clone and set up dependencies:
     ```bash
     git clone https://github.com/CarmineD8/aruco_ros.git
     git clone https://github.com/CarmineD8/SLAM_packages.git
     git clone https://github.com/KCL-Planning/ROSPlan.git
     ```
   - Replace the `marker_publish.cpp` file in the `aruco_ros` package with the customized version from this repository.

3. **Launch the Simulation Environment:**
   - Run the main launch file to initialize the Gazebo simulation, SLAM, and MoveBase:
     ```bash
     roslaunch robotics_project main.launch
     ```

4. **Start the Planning Process:**
   - Launch the planning simulation:
     ```bash
     roslaunch robotics_project plan_sim.launch
     ```

5. **Generate and Execute the Plan:**
   - Run the following commands sequentially in separate terminals:
     ```bash
     rosservice call /rosplan_problem_interface/problem_generation_server
     rosservice call /rosplan_planner_interface/planning_server
     rosservice call /rosplan_parsing_interface/parse_plan
     rosservice call /rosplan_plan_dispatcher/dispatch_plan
     ```

6. **Monitor the Plan:**
   - View the planner's output:
     ```bash
     rostopic echo /rosplan_planner_interface/planner_output -p
     ```

---

## Defined Actions
The planning system operates with three primary actions defined in the PDDL domain file:

1. **navigate_to:** Moves the robot between waypoints.
2. **detect_marker:** Rotates the robot to locate and identify ArUco markers, updating the smallest ID encountered.
3. **finalize_target:** Ensures all waypoints have been visited and directs the robot to the waypoint with the lowest marker ID.

---

## Package Overview

- **`robotics_project`:** Contains the simulation setup, including:
  - PDDL files for planning (`domain.pddl` and `problem.pddl`).
  - Configuration files for SLAM and MoveBase (`param/` directory).
  - Launch files (`main.launch` and `plan_sim.launch`).

- **`custom_action_interface`:** Manages action dispatching:
  - **navigate_to:** Sends coordinates to MoveBase for waypoint navigation.
  - **detect_marker:** Aligns detected markers and updates the lowest ID.
  - **finalize_target:** Guides the robot to the waypoint with the lowest marker ID.

- **`aruco_ros`:** Includes the modified `marker_publish.cpp` file for enhanced marker detection capabilities.

---

This README provides a comprehensive guide to the project's structure, functionality, and setup, ensuring smooth integration and execution of the robotic planning system.
