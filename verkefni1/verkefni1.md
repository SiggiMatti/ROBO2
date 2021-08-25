```
#pragma config(Motor,  port1,           rightMotor,    tmotorNormal, openLoop, reversed)
#pragma config(Motor,  port10,           leftMotor,     tmotorNormal, openLoop)   

void drive(int time, int f_b) {
	motor[rightMotor] = 127 * f_b;	// Keyri mótorana á fullum hraða afturábak eða áfram
	motor[leftMotor]  = 127 * f_b;
	wait1Msec(time);			        
}
void stopMotor(int stopTime) {
	motor[rightMotor] = 0;  // Stöðva mótorana
	motor[leftMotor]  = 0;
	wait1Msec(stopTime);
}
//+++++++++++++++++++++++++++++++++++++++++++++| MAIN |+++++++++++++++++++++++++++++++++++++++++++++++
task main()
{
	wait1Msec(2000);
	for( int i = 1;1<5;i++) {  // Keyri loopuna fimm sinnum
		drive(BASETIME * i, 1);  // Reikna út hversu langt róbotinn á að keyra
		stopMotor(500);
		drive(BASETIME * i, -1);
		stopMotor(500);
	}
	stopAllTasks();  // Þegar for loopan er búin hættir forritið
}												        // Program ends, and the robot stops
//++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
```
