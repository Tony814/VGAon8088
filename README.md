## VGA-on-8088

A copy of the files I posted to VCFed a number of years ago.  Below is my original writeup.

---------------------------

This is a patched drop-in version of the color VGA driver that comes with Windows 3.0.  I did this mainly as a programming/RE challenge, but also to gain insight into writing future drivers for this OS.  This driver should run unencumbered on any 8088+ CPU in real mode.  


### INSTALLATION
To use this driver, use the SETUP.EXE in the windows directory to install the regular VGA driver.  Once that's completed, rename that to VGA.OLD and rename my driver to VGA.DRV.  To check the version that you have, just rename it to VGA.EXE and run.


#### Version 1.0
- Patched SHR DX, 3 at CodeSegment1:09C2 with a call to end of segment and subsequent expansion.
- Patched SHR BX, 2 at CSEG1:16AF with a call to end of segment.
- Modified segment table to include extra bytes inserted before relocation table. 
- \*Luckily the LINKer MS used to create this left a bunch of unused bytes between the 2 code segments.


#### Version 1.1
Fixed an issue where the code put on the stack by BitBLT seemed to be corrupted by the REP bug.

- Patched CALL 14BE at CSEG1:0A64 with a call to end of segment.
- Added CLI and STI around the CALL to 14BE, to keep the stack compiler from being interrupted.
- \*This may result in poor KEYB/MOUSE performance while this routine is running
