# ROS notes

## How to change the ID number

This is the code to change the ID number of your ros domain. It **can't** be bigger than 232. Replace 10 with your chosen ID.

```bash
echo "export ROS_DOMAIN_ID=10" >> ~/.bashrc
source ~/.bashrc
```

You can check your ID with this command

```bash
echo $ROS_DOMAIN_ID
```

**You need to reopen your terminal to see the ID change**

## Running turtel sim

Run both of these things

```bash
source /opt/ros/jazzy/setup.bash
ros2 run turtlesim turtlesim_node
```

# Making the turtel move

## Moving the turtel in a straight line

```bash
ros2 topic pub --once /turtle1/cmd_vel geometry_msgs/msg/Twist "{linear: {x: 2.0}, angular: {z: 0.0}}"
```
- `ros2 topic pub` = Tells ROS2, publish (send) a message.

- `--once` = Send it just one time, then stop.

- `/turtle1/cmd_vel` = The "channel" this message is sent to. Think of it like a radio frequency — the turtle is tuned in and listening here for movement instructions. 

- `cmd_vel` = command velocity

- `geometry_msgs/msg/Twist` =  The type of message being sent — one that describes motion using speed and turning.

- `{linear: {x: 2.0}, angular: {z: 0.0}}` = The actual instruction:
  - linear x: 2.0 → move forward at speed 2
  - angular z: 0.0 → no turning (go straight)

## Making the turtel go in a circle

```bash
ros2 topic pub --rate 5 /turtle1/cmd_vel geometry_msgs/msg/Twist "{linear: {x: 1.0}, angular: {z: 1.0}}"
```

- `ros2 topic pub` = Tells ROS2, publish (send) a message.

- `--rate 5` = Sends the command 5 times per second

- `/turtle1/cmd_vel` = The "channel" this message is sent to. Think of it like a radio frequency — the turtle is tuned in and listening here for movement instructions.

- `cmd_vel` = command velocity

- `geometry_msgs/msg/Twist` =  The type of message being sent — one that describes motion using speed and turning.

- `{linear: {x: 1.0}, angular: {z: 1.0}}` = The actual instruction:
  - linear x: 1.0 → move forward at speed 1
  - angular z: 1.0 → turn at the same time

## Turtle Keyboard Controls
 
Launch command:
```
ros2 run teleop_twist_keyboard teleop_twist_keyboard --ros-args -r cmd_vel:=/turtle1/cmd_vel
```

 
## Movement Keys
 
| Key | Action |
|-----|--------|
| `i` | Forward |
| `,` | Backward |
| `j` | Turn left |
| `l` | Turn right |
| `u` `o` `m` `.` | Diagonal moves |
 
---
 
## Stop
 
| Key | Action |
|-----|--------|
| `k` | Stop completely |
 
---
 
## Speed Adjustments
 
| Key | Action |
|-----|--------|
| `q` / `z` | Increase / decrease both speeds |
| `w` / `x` | Increase / decrease forward speed only |
| `e` / `c` | Increase / decrease turn speed only |
 
---
 
## Quit
 
Press `Ctrl + C` to exit.