"""
#pragma config(Motor,  port1,           rightMotor,    tmotorNormal, openLoop, reversed)
#pragma config(Motor,  port10,           leftMotor,     tmotorNormal, openLoop)   

void drive(int time, int f_b) {
	motor[rightMotor] = 127 * f_b;		  // Motor on port2 is run at full (127) power forward
	motor[leftMotor]  = 127 * f_b;		  // Motor on port3 is run at full (127) power forward
	wait1Msec(time);			        // Robot runs previous code for 3000
}
void stopMotor(int stopTime) {
	motor[rightMotor] = 0;
	motor[leftMotor]  = 0;
	wait1Msec(stopTime);
}
//+++++++++++++++++++++++++++++++++++++++++++++| MAIN |+++++++++++++++++++++++++++++++++++++++++++++++
task main()
{
	wait1Msec(2000);						// Robot waits for 2000 milliseconds before executing program
	for( int i = 1;1<5;i++) {
		drive(BASETIME * i, 1);
		stopMotor(500);
		drive(BASETIME * i, -1);
		stopMotor(500);
	}
	stopAllTasks();
}												        // Program ends, and the robot stops
//++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
"""
