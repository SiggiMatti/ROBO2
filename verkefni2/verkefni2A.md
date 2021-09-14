```c
#pragma config(Motor,  port1,           leftMotor,     tmotorVex393, openLoop, reversed)
#pragma config(Motor,  port2,           clawMotor,     tmotorVex269, openLoop, reversed)
#pragma config(Motor,  port5,           armMotor,      tmotorVex393, openLoop, reversed)
#pragma config(Motor,  port10,          rightMotor,    tmotorVex393, openLoop)
#pragma config(Sensor, dgtl10,  			bumpSwitch, sensorTouch)
//+++++++++++++++++++++++++++++++++++++++++++++| MAIN |+++++++++++++++++++++++++++++++++++++++++++++++
task main ()
{
	while(vexRT[Btn8D] != 1 || SensorValue[bumpSwitch]!=1)
	{
		motor[leftMotor]  = (vexRT[Ch2] + vexRT[Ch1])/2;  // (y + x)/2
		motor[rightMotor] = (vexRT[Ch2] - vexRT[Ch1])/2;  // (y - x)/2

// Raise, lower or do not move arm
		if(vexRT[Btn5U] == 1)       	//If button 5U is pressed...
		{
			motor[armMotor] = 127;    	//...raise the arm.
		}
		else if(vexRT[Btn5D] == 1)  	//Else, if button 5D is pressed...
		{
			motor[armMotor] = -127;   	//...lower the arm.
		}
		else                      		//Else (neither button is pressed)...
		{
			motor[armMotor] = 0;      	//...stop the arm.
		}

// Open, close or do not more claw
		if(vexRT[Btn6U] == 1)       	//If Button 6U is pressed...
		{
			motor[clawMotor]  = 127;  		//...close the gripper.
		}
		else if(vexRT[Btn6D] == 1)  	//Else, if button 6D is pressed...
		{
			motor[clawMotor] = -127; 		//...open the gripper.
		}
		else                      		//Else (neither button is pressed)...
		{
			motor[clawMotor] = 0;    		//...stop the gripper.
		}
	}
	stopAllTasks();
}
//++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
```
