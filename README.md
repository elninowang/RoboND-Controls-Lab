[![Udacity - Robotics NanoDegree Program](https://s3-us-west-1.amazonaws.com/udacity-robotics/Extra+Images/RoboND_flag.png)](https://www.udacity.com/robotics)

# Table of Contents 目录 #

- [Hover Controller](#hover-controller) 悬停控制器
- [Attitude Controller](#attitude-controller) 姿态控制器
- [Position Controller](#position-controller) 位置控制器
- [Additional Helpful Tools](#additional-helpful-tools) 其他有用的工具
- [Using the Simulator](#using-the-simulator) 使用模拟器

# Hover Controller 悬停控制器 #
### Step 1: Writing the PID Class ###

步骤1，编写PID类

Add the PID class you wrote to `quad_controller/src/quad_controller/pid_controller.py`

将您写入的PID类添加到 `quad_controller/src/quad_controller/pid_controller.py`

### Step 2: Running the Hover Controller ###

步骤2：运行悬停控制器

**NOTE: Launch `roscore` first, then Unity**

**注意：先启动`roscore`，然后启动Unity **

After you have added the PID class, you can launch the hover controller.
To do so, run the following commands in your terminal:

添加PID类后，可以启动悬停控制器。
为此，请在终端中运行以下命令：
```
$ cd ~/catkin_ws/
$ catkin_make
$ source devel/setup.bash
$ roslaunch quad_controller hover_controller.launch
```
Now, with the hover controller launched, you can launch the quad simulator on your host platform.
The details surrounding this process will be be different for each host platform (Win/Mac/Linux).
please see "Using the Simulator" below.

现在，当启动了悬停控制器时，您可以在主机平台上启动四模拟器。
每个主机平台（Win/Mac/Linux）的细节将不同。
请参阅下面的“使用模拟器”。

### Step 3: Tuning Parameters ###

步骤3：调整参数

Now that Unity has been launched, verify that you can see poses being published by the simulator
on the `/quad_rotor/pose` topic:

现在Unity已经启动，验证您可以看到模拟器发布的姿势
在 `/quad_rotor/pose` 主题：
```
$ rostopic echo /quad_rotor/pose
```
If you see messages being published, you're golden! If you don't see messages being published,
then you might want to check your firewall settings on your host VM, or try restarting
the simulator (at the moment there's an infrequent bug where comunications with the simulator does not work).

如果你看到消息被发布，你是金色的！ 如果您看不到发布的消息，
那么您可能需要检查主机VM上的防火墙设置，或尝试重新启动
模拟器（目前有一个不常见的错误，模拟器的通讯不起作用）。

Now that you've verified poses are being published, you can use `rqt_reconfigure` to adjust the setpoints
and PID parameters for the hover controller!

现在您已经验证了配置文件，您可以使用`rqt_reconfigure`来调整设定值
和PID参数为悬停控制器！

You should tune the parameters using rqt_reconfigure until you are happy with the result.
For more information about how to tune PID parameters, head back into the classroom.

您应该使用rqt_reconfigure调整参数，直到您对结果满意为止。
有关如何调整PID参数的更多信息，请返回教室。

If manually adjusting the PID parameters is not your thing, feel free to try out the
Ziegler–Nichols tuning method using either `hover_zn_tuner_node`, or `hover_twiddle_tuner_node`.
These nodes both implement tuning methods that were discussed in the classroom.

如果手动调整PID参数不是您的事情，请随时尝试
使用 `hover_zn_tuner_node` 或  `hover_twiddle_tuner_node` 的Ziegler-Nichols调整方法。
这些节点都实现了在课堂上讨论的调优方法。

You might find that these tuner helpers only get you part way to a good tune.
There's a reason for this! Do you know?

你可能会发现这些调音助手只能让你分享一下好听。
这有一个理由！ 你知道吗？

To run `hover_zn_tuner_node`:
```
$ rosrun quad_controller hover_zn_tuner_node
```

To run `twiddle_tuner_node`:
```
$ rosrun quad_controller hover_twiddle_tuner_node
```

Write down the parameters you came up with, as you will need them later, for the full positional controller!

记下你提出的参数，以后你将需要它们，为完整的位置控制器！

**Bonus Quesion:** 奖金问题：

Why does the quad overshoot more frequently when heading to a set point at a lower altitude?
How can you modify the code to overcome this issue?

为什么当在较低的高度驶向一个设定点时，四边形过冲更频繁？
如何修改代码来克服这个问题？

# Attitude Controller 姿态控制器 #
The attitude controller is responsible for controlling the roll, pitch, and yaw angle of the quad rotor.
You can tune it very similarly to how you tuned the hover controller!
Note: ZN/Twiddle Tuner nodes only work with the Hover controller.

姿态控制器负责控制四轮转子的滚动，俯仰和偏航角。
您可以非常类似于调整悬停控制器！
注意：ZN/Twiddle 调谐器节点仅适用于悬停控制器。

### Step 1: Launch the Attitude Controller ###

步骤1：启动姿态控制器
```
$ roslaunch quad_controller attitude_controller.launch
```

### Step 2: Launch the Attitude Controller ###

步骤2：启动姿态控制器

Tune roll and pitch PID parmaeters until things look good.
You'll also need to write down these PID parameters.
As mentioned previously, you'll be using them in the positional controller!

调整滚动和俯仰PID参数，直到看起来很好。
您还需要记下这些PID参数。
如前所述，您将在位置控制器中使用它们！

# Position Controller 位置控制器 #

Finally, now that you have tuned the attitude controller and positional controllers
you can begin to work on the positional controller. The positional controller is
responsible for commanding the attitude and thrust vectors in order to acheive a
goal orientation in three dimensional space.

最后，现在你调整了姿态控制器和位置控制器
您可以开始在位置控制器上工作。 位置控制器是负责指挥态度和推力向量，以达到目的
三维空间中的目标取向。

Luckily, you've likely found some pretty decent PID parameters in your previous exploration.
If you're fortunate, these parameters will work for you... However, even if they don't
we've got you covered. There's all sorts of additional tooling that you can use to
troubleshoot and tune positional control of your quad!

幸运的是，在以前的探索中，您可能会发现一些非常不错的PID参数。
如果你幸运的话，这些参数可以为你而工作...但是，即使他们不是我们已经覆盖了。有各种额外的工具，您可以使用它来排除故障并调整四边形的位置控制！

### Step 1: Launch the Positional Controller ###

步骤1：启动位置控制器
```
$roslaunch quad_controller position_controller.launch
```

### Step 2: Tuning Parameters ### 

步骤2：调整参数

Tune parameters until the controller is well-behaved.
This should not be a very familiar, albeit potentially more difficult problem.

调整参数，直到控制器运行正常。
这不应该是一个非常熟悉的，尽管可能更困难的问题。

# Additional Helpful Tools 其他有用的工具 #
With so many degrees of freedom, debugging and troubleshooting can be a painful process.
In order to make things a little bit simpler, we've provided some tools that might make
life a little bit easier.

具有如此多的自由度，调试和故障排除可能是一个痛苦的过程。
为了使事情变得更简单，我们提供了一些可能使生活变得更容易的工具。

### Constraining Forces and Torques 约束力和扭矩 ###

It is possible to constrain forces and torques on the quad rotor's body frame.
This can be useful if you're trying to debug only a single degree of freedom.

可以约束四转子体框架上的力和转矩。
如果您只尝试调试一个单一的自由度，这将非常有用。

Example: Disallow movement along the quad's X axis

示例：不允许沿着四边形的X轴移动
```
$ rosservice call /quad_rotor/x_force_constrained "data: true"
```
Example: Disallow rotation about the quad's X axis

示例：不允许围绕四边形的X轴旋转
```
$ rosservice call /quad_rotor/x_torque_constrained "data: true"
```

### Setting the Camera Pose 设置相机姿势 ###

To set the camera pose you can either, right click in the simulator, and drag
or you can use the following service call, where the data parameter may take on the following
values:
0: Orthogonal to the inertial frame's YZ plane, facing the positive X direction.
1: Orthogonal to the inertial frame's XZ plane, facing the positive Y direction.
2: Orthogonal to the intertial frame's XY plane, facing the negative Z direction.
3: Perspective view, facing the quad's body frame origin from the -X,-Y, +Z quadrant.

要设置摄像机姿态，您可以右键单击模拟器，然后拖动或使用以下服务调用，其中数据参数可能采用以下值：
0：正交于惯性框架的YZ平面，面向正X方向。
1：正交于惯性框架的XZ平面，面向正Y方向。
2：正交到中间框架的XY平面，面向负Z方向。
3：透视图，面向四面体的身体框架起源于-X，-Y，+ Z象限。

```
$ rosservice call /quad_rotor/camera_pose_type "data: 0"
```
To reset the camera pose, to the default pose, you can use the service call, or right click.

要将相机姿态重置为默认姿势，可以使用服务调用，或右键单击。

### Setting the Camera Distance 设置相机距离 ###

To set the distance between the camera and the quad's body frame, you can use the
`/quad_rotor/camera_distance` service. For example, to set the camera distance to be
20 meters, you would call the service as follows:

要设置相机和四面体的身体框架之间的距离，可以使用 `/quad_rotor/camera_distance` 服务。 例如，将相机距离设置为20米，你会调用如下服务：
```
$ rosservice call /quad_rotor/camera_distance "data: 20.0"
```

To reset the camera distance to the default, simply right click in the simulator.

要将相机距离重置为默认值，只需在模拟器中右键单击即可。

### Disabling Gravity 禁用重力 ###

Gravity can be a harsh reality. Particularly when you're dealing with attitude tuning.
Fortunately, we can disable gravity in the simulator for the purposes of debugging.
To do so, call the `/quad_rotor/gravity` service as follows:

重力可能是一个严酷的现实。 特别是在处理态度调整时。
幸运的是，我们可以在模拟器中禁用重力进行调试。
要这样做，请按如下方式调用 `/quad_rotor/gravity` 服务：
```
$ rosservice call /quad_rotor/gravity "data: false"
```

### Setting Pose 设置姿势 ###

To set the quad pose, use the `/quad_rotor/set_pose` service. The following service call
will place the quad at the origin:

要设置四边形姿势，请使用`/ quad_rotor / set_pose`服务。 以下服务调用将将quad放在原点：
```
$ rosservice call /quad_rotor/set_pose "pose:
  position:
    x: 0.0
    y: 0.0
    z: 0.0
  orientation:
    x: 0.0
    y: 0.0
    z: 0.0
    w: 0.0" 
```

### Plotting using `quad_plotter_node` 绘制使用 `quad_plotter_node`###

The `quad_plotter_node` is a handy tool which allows you to capture and plot quad movements.
This might be useful to you while you are tuning the quad rotor in simulation.

`quad_plotter_node` 是一个方便的工具，可以捕获和绘制四边形的动作。
当您在模拟中调整四角转子时，这可能对您有用。

**Services:**
 - `/quad_plotter/start_recording` - Begins recording plot data (poses)
 - `/quad_plotter/stop_recording` - Stops recording plot data (poses)
 - `/quad_plotter/clear_path_history` - Clear plot history (poses)
 - `/quad_plotter/clear_waypoints` - Clear waypoints
 - `/quad_plotter/load_waypoints_from_sim` - Gets waypoints from the drone simulator
 - `/quad_plotter/get_path_history` - Returns path history as pose array
 - `/quad_plotter/plot_one` - Plots a 2D path on a given plane (*plane selection coming soon!*)
 - `/quad_plotter/plot_3d` - Create a 3D plot depicting path in perspective
 - `/quad_plotter/plot_grid` - Create a grid plot, showing 4 different views
 
**服务：**
 - `/quad_plotter/start_recording` - 开始记录绘图数据（姿势）
 - `/quad_plotter/stop_recording` - 停止录制图数据（姿势）
 - `/quad_plotter/clear_path_history` - 清除绘图历史（姿势）
 - `/quad_plotter/clear_waypoints` - 清除路点
 - `/quad_plotter/load_waypoints_from_sim` - 从无人机模拟器获取路点
 - `/quad_plotter/get_path_history` - 返回作为姿势数组的路径历史
 - `/quad_plotter/plot_one` - 在给定的平面上绘制2D路径（*平面选择即将推出！*）
 - `/quad_plotter/plot_3d` - 创建以透视图描绘路径的3D图
 - `/quad_plotter/plot_grid` - 创建一个网格图，显示4个不同的视图
 
**Example: Capturing a Grid Plot**
1. Load waypoints from the simulator (optional) `$ rosservice call /quad_plotter/load_waypoints_from_sim`
2. Begin recording poses `$ rosservice call /quad_plotter/start_recording "{}"`
3. Perform the behavior that you wish to capture in the simulator.
4. Stop recording poses `$ rosservice call /quad_plotter/stop_recording "{}"`
5. Generate the plot `$ rosservice call /quad_plotter/plot_gird "{}"`

**示例：捕获网格情景**
1. 从模拟器加载航点（可选）`$ rosservice call /quad_plotter/load_waypoints_from_sim`
2. 开始录制姿势 `$ rosservice call /quad_plotter/start_recording "{}"`
3. 执行您想要在模拟器中捕获的行为。
4. 停止录音姿势 `$ rosservice call /quad_plotter/stop_recording "{}"`
5. 生成图 `$ rosservice call /quad_plotter/plot_gird "{}"`
   
After performing the above steps, a new timestamped PNG image should be generated and placed in the `/quad_controller/output_data` directory.

执行上述步骤后，应生成新的时间戳PNG图像，并将其放置在 `/quad_controller/output_data` 目录中。

# Using the Simulator 使用模拟器 #

First be sure to grab the newest version of the simulator for your host computer OS [here](https://github.com/udacity/RoboND-Controls-Lab/releases). **Please note that you cannot use the simulator inside of the Udacity supplied course VM (Virtual Machine). You have to use the simulator for your host operating system and connect it to your VM.**

首先，请务必抓住最新版本的主机操作系统[这里](https://github.com/udacity/RoboND-Controls-Lab/releases) 的模拟器。 **请注意，您不能在Udacity提供的课程VM（虚拟机）中使用模拟器。 您必须使用主机操作系统的模拟器并将其连接到您的VM。**

If using the VM, inside the simulator's `_data` or `/Contents` folder, edit `ros_settings.txt` and set `vm-ip` to the VM’s IP address and set `vm-override` to `true`. If not using a VM, no edit is necessary.

如果使用虚拟机，在模拟器的`_data`或`/ Contents`文件夹中，编辑`ros_settings.txt`并将`vm-ip`设置为VM的IP地址，并将`vm-override`设置为`true`。 如果不使用虚拟机，则不需要编辑。

To find the ip of your VM, type `echo $(hostname -I)` into a terminal of your choice. **Be aware that the ip address of your VM can change. If you are experiencing problems, be sure to check that the VM's ip matches that of which you have in ros_settings.txt**

要找到您的VM的ip，请在您选择的终端中键入 `echo $(hostname -I)`。 请注意，您的虚拟机的IP地址可以更改。 如果您遇到问题，请确保检查VM的ip与您在ros_settings.txt中的ip匹配

**Note: Be sure to use at least V2.1.0 of the Udacity provided VM for this lab**

**注意：请务必至少使用本实验室的Udacity提供的VM的V2.1.0 **
