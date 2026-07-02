# ros2-autonomous-target-tracking

A ROS2 (C++) project built on `turtlesim` where a controller node autonomously drives the main turtle to catch a continuous stream of randomly spawned "target" turtles.

## Demo

<video src="https://raw.githubusercontent.com/frankNumfor/ros2-autonomous-target-tracking/main/media/demo.webm" controls width="700">
  Your browser does not support inline video. <a href="media/demo.webm">Download the demo instead</a>.
</video>

## How it works

- **`turtle_spawner`** periodically spawns a new turtle at a random position via the `turtlesim` `/spawn` service, and publishes the list of currently alive turtles on the `alive_turtles` topic. It also exposes a `catch_turtle` service that kills a turtle (via `/kill`) once it's been caught.
- **`turtle_controller`** subscribes to `alive_turtles` and to the main turtle's `/turtle1/pose`, picks a target (closest turtle first, configurable via the `catch_closest_turtle_first` parameter), and publishes `geometry_msgs/Twist` commands on `/turtle1/cmd_vel` to steer toward and catch it. Once within range, it calls `catch_turtle` to remove the target.

## Packages

| Package | Language | Purpose |
|---|---|---|
| [`turtlesim_project_cpp`](turtlesim_project_cpp) | C++ | `turtle_spawner` and `turtle_controller` nodes |
| [`my_robot_interfaces`](my_robot_interfaces) | interfaces | Custom messages/service used by the nodes above: `Turtle.msg`, `TurtleArray.msg`, `CatchTurtle.srv` |

## Build

Requires ROS2 (tested on Jazzy) and the `turtlesim` package.

```bash
mkdir -p ~/ros2_ws/src
cd ~/ros2_ws/src
git clone https://github.com/frankNumfor/ros2-autonomous-target-tracking.git
cd ~/ros2_ws
colcon build
source install/setup.bash
```

## Run

In separate terminals (after sourcing `install/setup.bash` in each):

```bash
ros2 run turtlesim turtlesim_node
ros2 run turtlesim_project_cpp spawner
ros2 run turtlesim_project_cpp controller
```
