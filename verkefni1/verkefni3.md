```c
#pragma config(Sensor, dgtl4,  rightEncoder,        sensorQuadEncoder)
#pragma config(Sensor, dgtl6,  leftEncoder,         sensorQuadEncoder)
#pragma config(Motor,  port1,           rightMotor,    tmotorNormal, openLoop, reversed)
#pragma config(Motor,  port10,           leftMotor,     tmotorNormal, openLoop)
#define BASEDEG 2.71
const int BASEDIST = 562;
//+++++++++++++++++++++++++++++++++++++++++++++| MAIN |+++++++++++++++++++++++++++++++++++++++++++++++
int turnArray[14]={1,-1,-1,1,1,-1,1,1,-1,1,1,-1,-1,1};
void turn(int deg, int leftOrRight) {
	while(abs(SensorValue[rightEncoder])<deg*BASEDEG) {
		motor[rightMotor] = 80* leftOrRight;
		motor[leftMotor] = -80* leftOrRight; 
	}
}

void stopMotor(int stopTime) {
	motor[rightMotor] = 0;
	motor[leftMotor]  = 0;
	wait1Msec(stopTime);
}

void drive(int dist) {
	while(abs(SensorValue[rightEncoder])<dist)
	{
		if(abs(SensorValue[rightEncoder]) == abs(SensorValue[leftEncoder]))
		{
			motor[rightMotor] = 80;
			motor[leftMotor]  = 80;
		}
		else if(abs(SensorValue[rightEncoder]) > abs(SensorValue[leftEncoder]))
		{
			// Beygja smá til vinstri
			motor[rightMotor] = 60;
			motor[leftMotor]  = 80;
		}
		else
		{
			// Beygja smá til hægri
			motor[rightMotor] = 80;
			motor[leftMotor]  = 60;
		}
	}
}

void resetEncoder() {
	SensorValue[rightEncoder] = 0;
	SensorValue[leftEncoder] = 0;
}
task main()
{
	resetEncoder();
	for(int i = 0; i<14; i++) {
		drive(BASEDIST);
		resetEncoder();
		stopMotor(500);
		turn(90, turnArray[i]);
		resetEncoder();
		stopMotor(500);
	}
}
//++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
```
