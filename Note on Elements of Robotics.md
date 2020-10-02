# Notes on *Elements of Robotics*

## Chap3 Reactive Behavior

- [Braitenberg vehicle](https://en.wikipedia.org/wiki/Braitenberg_vehicle)：只能小车雏形

#### :star:Algorithm 3.1: Timid

```
#call sound.system(-1)
call leds.top(32,0,32) # 顶部大灯，位于圆孔左右两侧(RGB)
call leds.bottom.left(0,0,32) # 左下灯(RGB)
call leds.bottom.right(0,32,0)# 右下灯(RGB)
call leds.circle(32,0,0,0,32,0,0,0)# 围绕方向键盘四周的八盏灯，从正前顺时针

# onevent定义事件
onevent proxIf
	# prox.horizontal共有序号[0]到[6]，越接近0表示前面没有物体遮挡
	if prox.horizontal[2] < 1000 then
		# motor.left.target轮速设定
		motor.left.target = 200
		motor.right.target = 200
	end
	if prox.horizontal[2] > 2000 or prox.horizontal[1] > 2000 then
		motor.left.target = 0
		motor.right.target = 0
	end
```

#### :star:Algorithm 3.2: Timid with while

```
#call sound.system(-1)
call leds.top(32,0,32) # 顶部大灯，位于圆孔左右两侧(RGB)
call leds.bottom.left(0,0,32) # 左下灯(RGB)
call leds.bottom.right(0,32,0)# 右下灯(RGB)
call leds.circle(32,0,0,0,32,0,0,0)# 围绕方向键盘四周的八盏灯，从正前顺时针

onevent prox
	# prox.horizontal共有序号[0]到[6]，越接近0表示前面没有物体遮挡
	while prox.horizontal[2] < 1000 do
		# motor.left.target轮速设定
		motor.left.target = 200
		motor.right.target = 200
	end
	while prox.horizontal[2] > 2000 do
		motor.left.target = 0
		motor.right.target = 0
	end
```

#### Activity 3.3: Dogged

> Specification (Dogged): When the robot detects an object in front, it moves backwards.
> When the robot detects an object in back, it moves forwards.

```
call sound.system(-1)
call leds.top(0,0,0)
call leds.bottom.left(0,0,0)
call leds.bottom.right(0,0,0)
call leds.circle(0,0,0,0,0,0,0,0)

onevent prox
	if prox.horizontal[2] > 2000 then
		motor.left.target = -200
		motor.right.target = -200
	end
	if prox.horizontal[5] > 2000 and prox.horizontal[6] > 2000 then
		motor.left.target = 200
		motor.right.target = 200
	end
```

#### :star:Activity 3.4: Dogged (stop)

> Specification (Dogged (stop)): As in Activity 3.3, but when an object
> is not detected, the robot stops.

```
call sound.system(-1)
call leds.top(32,0,32) # 顶部大灯，位于圆孔左右两侧(RGB)
call leds.bottom.left(0,0,32) # 左下灯(RGB)
call leds.bottom.right(0,32,0)# 右下灯(RGB)
call leds.circle(0,0,0,0,0,0,0,0)# 围绕方向键盘四周的八盏灯，从正前顺时针

onevent prox
	# prox.horizontal共有序号[0]到[6]，越接近0表示前面没有物体遮挡
	if prox.horizontal[2] > 2000 then
		motor.left.target = -100
		motor.right.target = -100
	end
	if prox.horizontal[5] > 2000 or prox.horizontal[6] > 2000 then
		motor.left.target = 100
		motor.right.target = 100
	end
	if prox.horizontal[2] < 1000 and prox.horizontal[5] < 1000 and prox.horizontal[6] < 1000 then
		motor.left.target = 0
		motor.right.target = 0
	end
```

#### Activity3.5 Attractive and repulsive

> Specification (Attractive and repulsive):When an object approaches the robot from behind, the robot runs away until it is out of range.

```
call sound.system(-1)
call leds.top(0,0,0)
call leds.bottom.left(0,0,0)
call leds.bottom.right(0,0,0)
call leds.circle(0,0,0,0,0,0,0,0)

onevent prox
	if prox.horizontal[5] > 2000 and prox.horizontal[6] > 2000 then
		motor.left.target = 300
		motor.right.target = 300
	end
	# 当两个事件同时发生才会运动
	if prox.horizontal[5] < 1000 and prox.horizontal[6] < 1000 then
		motor.left.target = 0
		motor.right.target = 0
	end
```

#### Algorithm 3.3 Paranoid

> In the algorithm, the left and right motors are set to equal but opposite powers. Experiment with these power levels to see their influence on the turning radius of the robot.

```
onevent prox
	if prox.horizontal[2] > 2000 then
		motor.left.target = 200
		motor.right.target = 200
	end
	if prox.horizontal[2] < 1000 then
		motor.left.target = -200
		motor.right.target = 200
	end
```

#### :star:Activity 3.6 Paranoid (right-left)

> Specification (Paranoid (right-left)): When an object is detected in front of the robot, the robot moves forwards. When an object is detected to the right of the robot, the robot turns right. When an object is detected to the left of the
> robot, the robot turns left. When no object is detected the robot does not move.

```
var speed = 150 # 定义速度变量，只能在最开始进行定义

call sound.system(-1)
call leds.top(32,0,32) # 顶部大灯，位于圆孔左右两侧(RGB)
call leds.bottom.left(0,0,32) # 左下灯(RGB)
call leds.bottom.right(0,32,0)# 右下灯(RGB)
call leds.circle(0,0,0,0,0,0,0,0)# 围绕方向键盘四周的八盏灯，从正前顺时针

onevent prox
	# prox.horizontal共有序号[0]到[6]，越接近0表示前面没有物体遮挡
	if prox.horizontal[2] > 2000 then
		motor.left.target = speed
		motor.right.target = speed
	end
	if prox.horizontal[2] < 1000 and prox.horizontal[0] > 2000 then
		motor.left.target = -speed
		motor.right.target = speed
	end
	if prox.horizontal[2] < 1000 and prox.horizontal[4] > 2000 then
		motor.left.target = speed
		motor.right.target = -speed
	end
	if prox.horizontal[2] < 1000 and prox.horizontal[4] < 1000 and prox.horizontal[0] < 1000 then
		motor.left.target = 0
		motor.right.target = 0
	end
	
```

#### Activity 3.8: Insecure

> Specification (Insecure): If **an object is not detected to the left of the robot**, set the right motor to rotate forwards and set the left motor off. If an object is **detected** to the left of the robot, set the right motor off and set the left motor to rotate forwards.

```
var speed = 200 # 定义速度变量，只能在最开始进行定义

call sound.system(-1)
call leds.top(32,0,32) # 顶部大灯，位于圆孔左右两侧(RGB)
call leds.bottom.left(0,0,32) # 左下灯(RGB)
call leds.bottom.right(0,32,0)# 右下灯(RGB)
call leds.circle(0,0,0,0,0,0,0,0)# 围绕方向键盘四周的八盏灯，从正前顺时针

onevent prox
	# prox.horizontal共有序号[0]到[6]，越接近0表示前面没有物体遮挡
	if prox.horizontal[0] < 1000 or prox.horizontal[1] < 1000 then
		motor.left.target = 0
		motor.right.target = speed
	end
	if prox.horizontal[0] > 2000 or prox.horizontal[1] > 2000 then
		motor.left.target = speed
		motor.right.target = 0
	end
```

会观察发现这个机器人可以跟着墙右边行走

#### Activity 3.9: Driven

> Specification (Driven): If an object is detected to the left of the robot, set the right motor to rotate forwards and set the left motor off. If an object is detected to the right of the robot, set the right motor off and set the left motor to rotate forwards.

```
var speed = 200 # 定义速度变量，只能在最开始进行定义

call sound.system(-1)
call leds.top(32,0,32) # 顶部大灯，位于圆孔左右两侧(RGB)
call leds.bottom.left(0,0,32) # 左下灯(RGB)
call leds.bottom.right(0,32,0)# 右下灯(RGB)
call leds.circle(0,0,0,0,0,0,0,0)# 围绕方向键盘四周的八盏灯，从正前顺时针

onevent prox
	# prox.horizontal共有序号[0]到[6]，越接近0表示前面没有物体遮挡
	if prox.horizontal[3] > 2000 or prox.horizontal[4] > 2000 then
		motor.left.target = 0
		motor.right.target = speed
	end
	if prox.horizontal[0] > 2000 or prox.horizontal[1] > 2000 then
		motor.left.target = speed
		motor.right.target = 0
	end
```

#### Algorithm 3.4: Line following with two sensors

> If the robot moves off the line to the left, the left sensor will not detect the line while the right sensor is still detecting it; the robot must turn to the right.
> If the robot moves off the line to the right, the right sensor will not detect the line while the left sensor is still detecting it; the robot must turn to the left.
>
> the robot must move forwardwhenever both sensors detect a dark surface,

- 主要传感器

  prox.ground.delta/relected两个值遇到黑色反光就会降到200以下

- 伪代码

  ```
  1: when both sensors detect black
  2: left-motor-power ← 100
  3: right-motor-power ← 100
  4:
  5: when neither sensor detects black
  6: left-motor-power ← 0
  7: right-motor-power ← 0
  8:
  9: when only the left sensor detects black
  10: left-motor-power ← 0
  11: right-motor-power ← 50
  12:
  13: when only the right sensor detects black
  14: left-motor-power ← 50
  15: right-motor-power ← 0
  ```

- 我的初始程序

  ```
  var speed = 100 # 定义速度变量，只能在最开始进行定义
  
  call sound.system(-1)
  call leds.top(32,0,32) # 顶部大灯，位于圆孔左右两侧(RGB)
  call leds.bottom.left(0,0,32) # 左下灯(RGB)
  call leds.bottom.right(0,32,0)# 右下灯(RGB)
  call leds.circle(0,0,0,0,0,0,0,0)# 围绕方向键盘四周的八盏灯，从正前顺时针
  
  onevent prox
  	# prox.ground.delta 共有序号[0]和[1]，越接近0表示前面两个都在线上
  	if prox.ground.delta[0] < 150 and prox.ground.delta[1] < 150 then
  		motor.left.target = 2 * speed
  		motor.right.target = 2 * speed
  	end
  	if prox.ground.delta[0] > 200 and prox.ground.delta[1] > 200 then
  		motor.left.target = 0
  		motor.right.target = 0
  	end
  	if prox.horizontal[0] < 150 and prox.ground.delta[1] > 200 then
  		motor.left.target = 0
  		motor.right.target = speed
  	end
  	if prox.horizontal[1] < 150 and prox.ground.delta[0] > 200 then
  		motor.left.target = speed
  		motor.right.target = 0
  	end
  ```

- 官方版本代码（这样的算法无法跑出全程）

  ```
  # Constants 常量
  var CHANGE = 25
  var INCREMENT = 100
  var THRESHOLD = 200
  
  # Variables 变量
  var motor          # The base value of the motor power
  var motor_change   # The amount to change when approaching and turning
  
  var state # 0 = off, 1 = on 运行状态
  
  # Initialization 变量以及LED灯状态初始化
  state = 0
  motor = 0
  motor_change = (motor * CHANGE)/100
  call leds.circle(0,0,0,0,0,0,0,0)
  call leds.top(0,0,0)
  
  # Set the circle leds to indicate the motor power
  # 设计subroutine，实现在后续motor功率调整后，车上LED灯显现出对应的颜色
  sub set_circle_leds
    if motor/100==0 then call leds.circle(0,0,0,0,0,0,0,0) end
    if motor/100==1 then call leds.circle(32,0,0,0,0,0,0,0) end
    if motor/100==2 then call leds.circle(32,32,0,0,0,0,0,0) end
    if motor/100==3 then call leds.circle(32,32,32,0,0,0,0,0) end
    if motor/100==4 then call leds.circle(32,32,32,32,0,0,0,0) end
    if motor/100==5 then call leds.circle(32,32,32,32,32,0,0,0) end
  
  # Left and right button event handlers 使用左右按钮调整功率
  #   Increase or decrease motor power within 0-500
  onevent button.left
    if  button.left == 0 then
      motor = motor - INCREMENT
      if  motor < 0 then motor = 0 end
      motor_change = (motor * CHANGE)/100
      callsub set_circle_leds
    end
  
  onevent button.right
    if  button.right == 0 then
      motor = motor + INCREMENT
      if  motor > 500 then motor = 500 end
      motor_change = (motor * CHANGE)/100
      callsub set_circle_leds
    end
  
  # When center button released
  onevent button.center
    if  button.center == 0 then
      # If off, set state to 1 (on) and drive forwards
      if  state == 0 then
        state = 1
        motor.left.target  = motor
        motor.right.target = motor
      #   else if state is on, call stop the motors
      else
        state = 0
        motor.left.target  = 0
        motor.right.target = 0
      end
      motor_change = (motor * CHANGE)/100
      callsub set_circle_leds
    end
  
  # Proximity event occurs, turn if necessary 
  # 判定是否接触地面上特定的条纹
  onevent prox
    if  state == 0 then return end
    # If both sensors detect the tape, go straight
    if  prox.ground.delta[0] < THRESHOLD and
        prox.ground.delta[1] < THRESHOLD then
      motor.left.target  = motor
      motor.right.target = motor
      call leds.top(32,0,0)
    # If left detects and right doesn't, turn left
     elseif prox.ground.delta[0] < THRESHOLD and
           prox.ground.delta[1] >= THRESHOLD then
    	 motor.left.target  = motor - motor_change
    	 motor.right.target = motor + motor_change
       call leds.top(0, 0, 32)
    # If right detects and left doesn't, turn right
     elseif prox.ground.delta[0] >= THRESHOLD and
           prox.ground.delta[1] < THRESHOLD then
      motor.left.target  = motor + motor_change
      motor.right.target = motor - motor_change
      call leds.top(0, 32, 0)
    end
  ```

- 我的最终程序

  ```
  # Constants 常量
  var CHANGE = 25
  var INCREMENT = 50
  var THRESHOLD = 200
  
  # Variables 变量
  var motor          # The base value of the motor power
  var motor_change   # The amount to change when approaching and turning
  
  var state # 0 = off, 1 = on 运行状态
  
  # Initialization 变量以及LED灯状态初始化
  state = 0
  motor = 0
  motor_change = (motor * CHANGE)/100
  call leds.circle(0,0,0,0,0,0,0,0)
  call leds.top(0,0,0)
  
  # Set the circle leds to indicate the motor power
  # 设计subroutine，实现在后续motor功率调整后，车上LED灯显现出对应的颜色
  sub set_circle_leds
    if motor/50==0 then call leds.circle(0,0,0,0,0,0,0,0) end
    if motor/50==1 then call leds.circle(32,0,0,0,0,0,0,0) end
    if motor/50==2 then call leds.circle(32,32,0,0,0,0,0,0) end
    if motor/50==3 then call leds.circle(32,32,32,0,0,0,0,0) end
    if motor/50==4 then call leds.circle(32,32,32,32,0,0,0,0) end
    if motor/50==5 then call leds.circle(32,32,32,32,32,0,0,0) end
    if motor/50==6 then call leds.circle(32,32,32,32,32,32,0,0) end
    if motor/50==7 then call leds.circle(32,32,32,32,32,32,32,0) end
    if motor/50>=8 then call leds.circle(32,32,32,32,32,32,32,32) end
  
  # Left and right button event handlers 使用左右按钮调整功率
  #   Increase or decrease motor power within 0-500
  onevent button.left
    if  button.left == 0 then
      motor = motor - INCREMENT
      if  motor < 0 then motor = 0 end
      motor_change = (motor * CHANGE)/100
      callsub set_circle_leds
    end
  
  onevent button.right
    if  button.right == 0 then
      motor = motor + INCREMENT
      if  motor > 500 then motor = 500 end
      motor_change = (motor * CHANGE)/100
      callsub set_circle_leds
    end
  
  # When center button released
  onevent button.center
    if  button.center == 0 then
      # If off, set state to 1 (on) and drive forwards
      if  state == 0 then
        state = 1
        motor.left.target  = motor
        motor.right.target = motor
      #   else if state is on, call stop the motors
      else
        state = 0
        motor.left.target  = 0
        motor.right.target = 0
      end
      motor_change = (motor * CHANGE)/100
      callsub set_circle_leds
    end
  
  # Proximity event occurs, turn if necessary 
  # 判定是否接触地面上特定的条纹
  onevent prox
    if  state == 0 then return end
    # If both sensors detect the tape, go straight
    if  prox.ground.delta[0] < THRESHOLD and
        prox.ground.delta[1] < THRESHOLD then
      motor.left.target  = motor
      motor.right.target = motor
      call leds.top(32,0,0)
    # If left detects and right doesn't, turn left
     elseif prox.ground.delta[0] < THRESHOLD and
           prox.ground.delta[1] >= THRESHOLD then
    	 #motor.left.target  = motor - motor_change
    	 #motor.right.target = motor + motor_change
    	 #motor.left.target  = motor - 2 * motor_change
    	 #motor.right.target = motor
    	 motor.left.target = 0
    	 motor.right.target = motor
       call leds.top(0, 0, 32)
    # If right detects and left doesn't, turn right
     elseif prox.ground.delta[0] >= THRESHOLD and
           prox.ground.delta[1] < THRESHOLD then
      #motor.left.target  = motor + motor_change
      #motor.right.target = motor - motor_change    
      #motor.left.target  = motor
      #motor.right.target = motor - 2 * motor_change
    	 motor.left.target = motor
    	 motor.right.target = 0
      call leds.top(0, 32, 0)
    end
  ```

#### :star:Activity 3.12: Regaining the line after losing it

> • Modify the algorithm so that the robot returns to the line after it loses it, that is, when both sensors no longer detect black.
> 
> • Algorithm 1: Before both sensors no longer detect the line, one of them will be the first that no longer detects the line. Use a variable to remember the sensor that first lost the line; when the line is lost, turn in the opposite direction of the value of this variable.
> 
> • Algorithm 2: The previous algorithm might not work if the robot is moving too fast and runs off the line before it detects that only one sensor lost the line. Instead, if both sensors no longer detect black, cause the robot the search for the line

- Algorithm1

  - 特性

    - 结合`button.backward`可以实现救援模式的打开与关闭
    - 结合`button.center`代码进行小车车速控制
    - 实现`sub set_bottom_leds`实现救援模式下不同LED效果的控制

  - 代码

    ```
    # 2020.10.02
    # Constants 常量
    var CHANGE = 25
    var INCREMENT = 50
    var THRESHOLD = 200
    
    # Variables 变量
    var motor          # The base value of the motor power
    var motor_change   # The amount to change when approaching and turning
    # Activity 3.12 Algorithm 1 P46
    var lost_first = -1 # 0 for left doesn't, 1 for right doesn't
    var recovery_mode = 0 # 0 = off, 1 = on
    
    var state # 0 = off, 1 = on 运行状态
    
    # Initialization 变量以及LED灯状态初始化
    state = 0
    motor = 0
    motor_change = (motor * CHANGE)/100
    call leds.circle(0,0,0,0,0,0,0,0)
    call leds.top(0,0,0)
    
    call leds.bottom.left(0,0,0)
    call leds.bottom.right(0,0,0)
    
    # 对于救援模式的灯
    sub set_bottom_leds
      if recovery_mode==1 then
      	  # 如果救援模式打开，若未离开线，就显示左右红灯
    	  if lost_first==-1 then
    		call leds.bottom.left(32,0,0)
    	  	call leds.bottom.right(32,0,0)
    	  # 如果救援模式打开，若离开线，就显示左右蓝灯
    	  else
    	  	call leds.bottom.left(0,0,32)
    	  	call leds.bottom.right(0,0,32)
    	  end
      # 如果救援模式不打开，就不开左右灯
      elseif recovery_mode==0 then
    	  	call leds.bottom.left(0,0,0)
    	  	call leds.bottom.right(0,0,0)
      end
    
    # Set the circle leds to indicate the motor power
    # 设计subroutine，实现在后续motor功率调整后，车上LED灯显现出对应的颜色
    sub set_circle_leds
      if motor/50==0 then call leds.circle(0,0,0,0,0,0,0,0) end
      if motor/50==1 then call leds.circle(32,0,0,0,0,0,0,0) end
      if motor/50==2 then call leds.circle(32,32,0,0,0,0,0,0) end
      if motor/50==3 then call leds.circle(32,32,32,0,0,0,0,0) end
      if motor/50==4 then call leds.circle(32,32,32,32,0,0,0,0) end
      if motor/50==5 then call leds.circle(32,32,32,32,32,0,0,0) end
      if motor/50==6 then call leds.circle(32,32,32,32,32,32,0,0) end
      if motor/50==7 then call leds.circle(32,32,32,32,32,32,32,0) end
      if motor/50>=8 then call leds.circle(32,32,32,32,32,32,32,32) end
    
    # Left and right button event handlers 使用左右按钮调整功率
    #   Increase or decrease motor power within 0-500
    onevent button.left
      if  button.left == 0 then
        motor = motor - INCREMENT
        if  motor < 0 then motor = 0 end
        motor_change = (motor * CHANGE)/100
        callsub set_circle_leds
      end
    
    onevent button.right
      if  button.right == 0 then
        motor = motor + INCREMENT
        if  motor > 500 then motor = 500 end
        motor_change = (motor * CHANGE)/100
        callsub set_circle_leds
      end
    
    # When center button released
    onevent button.center
      if  button.center == 0 then
        # If off, set state to 1 (on) and drive forwards
        if  state == 0 then
          state = 1
          motor.left.target  = motor
          motor.right.target = motor
        #   else if state is on, call stop the motors
        else
          state = 0
          motor.left.target  = 0
          motor.right.target = 0
        end
        motor_change = (motor * CHANGE)/100
        callsub set_circle_leds
      end
    
    # 打开救援模式按钮
    # When backford button released
    onevent button.backward
      if  button.backward == 0 then
        if  recovery_mode == 0 then
          recovery_mode = 1
        else
          recovery_mode = 0
          lost_first = -1
        end
      	callsub set_bottom_leds
      end
    
    # Proximity event occurs, turn if necessary 
    # 判定是否接触地面上特定的条纹
    onevent prox
      if  state == 0 then return end
      # If both sensors detect the tape, go straight
      if  prox.ground.delta[0] < THRESHOLD and
          prox.ground.delta[1] < THRESHOLD then
        motor.left.target  = motor
        motor.right.target = motor
        call leds.top(32,0,0)
        lost_first = -1
        callsub set_bottom_leds
      # If left detects and right doesn't, turn left
       elseif prox.ground.delta[0] < THRESHOLD and
             prox.ground.delta[1] >= THRESHOLD then
      	motor.left.target  = motor - motor_change
      	motor.right.target = motor + motor_change
        call leds.top(0, 0, 32)
        # Activity 3.12 Algorithm 1 P46
        lost_first = 1 # 0 for left doesn't, 1 for right doesn't
      # If right detects and left doesn't, turn right
       elseif prox.ground.delta[0] >= THRESHOLD and
             prox.ground.delta[1] < THRESHOLD then
        motor.left.target  = motor + motor_change
        motor.right.target = motor - motor_change    
        call leds.top(0, 32, 0)
        # Activity 3.12 Algorithm 1 P46
        lost_first = 0 # 0 for left doesn't, 1 for right doesn't
      # If both don't detect
       elseif prox.ground.delta[0] >= THRESHOLD and
             prox.ground.delta[1] >= THRESHOLD then
        # 若打开救援模式
        if recovery_mode == 1 then
    	    if  lost_first == 1 then
    		  	motor.left.target = -motor_change
    		  	motor.right.target = motor
    	    	callsub set_bottom_leds
    	    elseif lost_first == 0 then
    	    	motor.left.target = motor
    		  	motor.right.target = -motor_change
    	    	callsub set_bottom_leds
    	    end
    	# 若不开救援模式，则两个传感器均没有检测到就停止
    	elseif recovery_mode == 0 then
    		motor.left.target = 0
    		motor.right.target = 0
        end
      end
    ```

    

  