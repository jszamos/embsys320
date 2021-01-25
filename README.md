# Assignment 1 - uDebugger
EMBSYS 320 Winter 2021
Due: 01/24/2021
The goal of this assignment is to write a diagnostic hard fault exception handler that may help you to debug later assignments.
Example of the information you should log:
Hard fault at PC=0x1234ABCD LR=0xABCD1234
1. Download and unzip the uDebugger project contained in the zip file: uDebugger.zip
2. Open the uDebugger.eww workspace in the EWARM IDE.
3. Make sure the uDebugger project builds and runs. It should print a “Fail” message to the UART (BAUD rate should be 38400).
4. Implement the code specified by the TODO comments - do a global search in the project (Ctrl_Shift-F) for the “TODO” string. There are TODO comments in these 3 files:
a. main.c
b. debugger.c
c. startup.s
5. Clean your project and zip it into a file named uDebugger_<YourName>.zip
a. Clean means delete the “Debug” folder before zipping so you don’t bloat your submission.
b. Example submission filename: uDebugger_johndoe.zip
6. Submit your zip file by the due date.
Note: Your program should fault exactly 10 times.
Additional Challenge
If you would like an additional challenge, instead of printing just the PC and LR, modify your program to print the entire stack frame consisting of R0-R3, R12, LR, PC, PSR.
