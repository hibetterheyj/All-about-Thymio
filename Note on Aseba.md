# Note on `Aseba`

> 

- Both `if` and `when` execute a different block of code according to whether a condition is true or false; but `when` executes the block corresponding to true only if the previous evaluation of the condition was false and the current one is true. This allows the execution of code only when something changes.

  [Conditionals](http://wiki.thymio.org/en:asebalanguage#toc11)

- `sub` | Subroutines

  When you want to perform the same sequence of operations at two or more different places in the code, you can write common code just once in a subroutine and then call this subroutine from different places. You define a subroutine using the `sub` keyword followed by the name of the subroutine. You call the subroutine using the `callsub` keyword, followed by the name of the subroutine. Subroutines cannot have arguments, nor be [recursive](http://en.wikipedia.org/wiki/Recursion_(computer_science)), either directly or indirectly. Subroutines can access any variable.

  - Example:

    ```
    var v = 0
    
    sub toto
    v = 1
    
    onevent test
    callsub toto
    ```

  - More complex example

    ```
    sub set_circle_leds
      if motor/100==0 then call leds.circle(0,0,0,0,0,0,0,0) end
      if motor/100==1 then call leds.circle(32,0,0,0,0,0,0,0) end
      if motor/100==2 then call leds.circle(32,32,0,0,0,0,0,0) end
      if motor/100==3 then call leds.circle(32,32,32,0,0,0,0,0) end
      if motor/100==4 then call leds.circle(32,32,32,32,0,0,0,0) end
      if motor/100==5 then call leds.circle(32,32,32,32,32,0,0,0) end
    
    # once the right button got activated, the motor power adds up INCREMENT power and corresponding led get changed!
    onevent button.right
      if  button.right == 0 then
        motor = motor + INCREMENT
        if  motor > 500 then motor = 500 end
        motor_change = (motor * CHANGE)/100
        callsub set_circle_leds
      end
    ```

    