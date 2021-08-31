```c
#pragma config(Sensor, dgtl4,  rightEncoder,        sensorQuadEncoder)
#pragma config(Sensor, dgtl6,  leftEncoder,         sensorQuadEncoder)
#pragma config(Motor,  port1,           rightMotor,    tmotorNormal, openLoop, reversed)
#pragma config(Motor,  port10,           leftMotor,     tmotorNormal, openLoop)
// Þvermál = 101
// U = 101 * PI = 317,3
// fjöldi sn = 500/317,3 = 1,56 sn
// BASELENGTH = 1,56 * 360 = 562
const int BASEDIST = 562;
void drive(int dist, int f_b) {
	while(abs(SensorValue[rightEncoder])<dist)
	{
		if(abs(SensorValue[rightEncoder]) == abs(SensorValue[leftEncoder]))
		{
			motor[rightMotor] = 80 * f_b;
			motor[leftMotor]  = 80 * f_b;
		}
		else if(abs(SensorValue[rightEncoder]) > abs(SensorValue[leftEncoder]))
		{
			// Beygja smá til vinstri
			motor[rightMotor] = 60 * f_b;
			motor[leftMotor]  = 80 * f_b;
		}
		else
		{
			// Beygja smá til hægri
			motor[rightMotor] = 80 * f_b;
			motor[leftMotor]  = 60 * f_b;
		}
	}
}
void resetEncoder() {
	SensorValue[rightEncoder] = 0;
	SensorValue[leftEncoder] = 0;
}

//+++++++++++++++++++++++++++++++++++++++++++++| MAIN |+++++++++++++++++++++++++++++++++++++++++++++++
task main()
{
	wait1Msec(2000);
	for(int i = 1;i<6;i++) {
		drive(BASEDIST * i, 1);
		resetEncoder();
		drive(BASEDIST * i, -1);
		resetEncoder();
	}
}
//++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
```
