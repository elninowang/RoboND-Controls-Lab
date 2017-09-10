# Quad Controller Project 无人机项目 #

### Launching the Simulator 启动模拟器 ###
In order to launch the simulator, simply run the simulator binary.

为了启动模拟器，只需运行模拟器二进制。

**Note: 注意**
You must first run `roscore`. If you don't run `roscore`, the
simulator will still run, but you will not be able to communicate
with it via ROS.

你必须先运行`roscore`。 如果你不运行`roscore`，那么模拟器仍然可以运行，但是你将无法通过ROS进行通信。

### The hover controller node 悬停控制器节点 ###
Perhaps the simplest controller in the `quad_controller` package is the hover
controller node. It is nothing more than a simple PID controller that sets
applies a vertical thrust vector to the quad rotor body.

也许 `quad_controller`  包中最简单的控制器是悬停控制器节点。 它只不过是一个简单的PID控制器，它将垂直推力矢量应用于四转子体。

#### Running the hover controller node 运行悬停控制器节点 ####

```sh
$ rosrun quad_controller hover_controller_node

```
**Optional Step: 可选步骤：**
You can set up camera angles and motion constrants to make it
make visualization and debugging of the hover conteroller
simple. To do, simply run `setup_2d_hover_world.sh`.

您可以设置摄像机角度和运动稳定性，使其可以使悬停旋转器的可视化和调试变得简单。 要做，只需运行 `setup_2d_hover_world.sh`。

```sh
$ rosrun quad_controller setup_2d_hover_world.sh
```

**Hover controller parameter tuning: 悬停控制器参数调整：**
The easiest way to tune the hover controller's PID parmaeters
or set the target position is to use ROS' dynamic reconfigure.

调整悬浮控制器PID参数或设置目标位置的最简单方法是使用ROS的动态重新配置。

To run dynamic reconfigure: 运行动态重新配置：
```sh
$ rqt_reconfigure
```

If `hover_controller_node` is running you should now be able
to interactively explore the hover controller.

如果“hover_controller_node”正在运行，您现在应该可以交互地浏览悬停控制器。

**Plotting Altitude**
One simple way to plot the altitude is to use `rqt_plot`. 

**绘制高度**
绘制高度的一种简单方式是使用`rqt_plot`。

```sh
$ rqt_plot /quad_rotor/pose/pose/position/z
```
