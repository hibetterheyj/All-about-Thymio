# SimplestExamples

## Official

### Thymio stops at the border of the table

> https://www.thymio.org/program/aseba/

```
# reset outputs
call sound.system(-1)
call leds.top(0,0,0)
call leds.bottom.left(0,0,0)
call leds.bottom.right(0,0,0)
call leds.circle(1,0,1,0,1,0,1,0)
#call leds.circle(0,0,0,0,0,0,0,0)

# Thymio moves forward when you press the forward button
onevent buttons
  when button.forward == 1 do
    motor.left.target = 200
    motor.right.target = 200
  end
  when button.left == 1 do
    motor.left.target = 0
    motor.right.target = 200
  end
  when button.right == 1 do
    motor.left.target = 200
    motor.right.target = 0
  end
  

# if the ground sensors detects nothing Thymio becomes red otherwise he becomes green
onevent prox
  if prox.ground.delta[0] <= 400 or prox.ground.delta[1] <= 400 then
  motor.left.target = 0
  motor.right.target = 0
    call leds.top(32,0,0) # red
  else
    #call leds.top(0,32,0) # green
    #call leds.top(0,0,32) # blue
    call leds.top(0,32,32) # light blue
  end
```
