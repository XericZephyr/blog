Test Report - ASM Tracker Time Limit Detouring Project
=========

# Project Requirements Restatement

## Problems

1. Properly compile the Visual Studio Projects in `trunk_new.rar` given.
2. Run project and see the tracking process of human face.
3. Remove the time limit of the program.
4. Summarize above progress into a report document

	Include: 
	- Problems encountered
	- Solving process
	- Final Solutions
	
5. Briefly introduce the architecture and running process of the FaceDemo program.


# Prerequisites 

## Software Preparation

### Visual Studio 2008 Professional Edition or higher subscription
You could get this Integrated Development Environment(hereafter IDE) **for free with a student status** from [DreamSpark](https://www.dreamspark.com/).

### Qt 4.x.x
Get Qt version 4.x.x from http://qt-project.org/downloads and install it.

### OpenCV 2.1 
Navigate to the link below and follow the instruction to install OpenCV 2.1 and integrate it with Visual Studio 2008.
http://civanim.blogspot.com/2010/04/install-opencv-21-for-microsoft-visual.html


# Encountered Problems and Solutions

## Compatibility Problems

I encountered some problems when compiling the projects. 

### Notions and Predefinitions

- Define trunk_new extract target directory as $(TrunkDIR)
- Define ASMTracker Project directory as $(ASMTrkDIR)

### Solution Steps

1. Add following include path to Visual Studio 2008 

```
$(TrunkDIR)\FlyCapture2\include
```

2. Add following path to system variable PATH then reboot to make sure that it takes effect

```
$(TrunkDIR)\OpenCV2.1\bin
$(TrunkDIR)\FlyCapture2\bin
```

3. Adjust Running Parameters

In `$(ASMTrkDIR)/Param/Param.txt`
modify `captureType=3` to `captureType=2`

4. In `$(ASMTrkDIR)/ASMTrackerDLL/TrackerDriverDll.h`

modify `line 24` to
```C 
#include "../FlyToOpencvDLL/FlyToOpencv.h"
```
5. In `$(ASMTrkDIR)/QtDriverGUI/main.cpp`
modify `line 45` to 
```C
	bool allowStart = 1;
```

## Time Limit Detouring Problem

There are many solutions to the Time Limit Detouring problems.

### Method without Code Patch
You could simply change `maxFrame` parameter to `(unsigned int)-1`(`0xFFFFFFFF`) in Params.txt, it will raise the time limit to `0xFFFFFFFF` frames. It is not forever but you could use it for approximately 4 years long.

### Code Patch Methods

There are also several code patch methods to detour time limit of the program. Below is only one of them.

The basic idea is to check the progressBar code and then find the source of the time limit.
In `$(ASMTrkDIR)\ASMTrackerDLL\TrackerDriverDLL.cpp`, find the code starting from `line 642`, comment `line 643,644,669,670,671`. Then the time limit will actually disappear. 

The full code block concerning time-limit is listed below.
```C
		// Check if maximum number of frames has been exceeded
		if (frameNumb < params->maxFrames)
		{				
			rawFrame = cvQueryFrame(capture);
			cvWaitKey(1);
			if (rawFrame)
			{
				frameNumb++;
				if (params->subsample)
					cvPyrUp(rawFrame, inputFrame);
				else
					cvCopy(rawFrame, inputFrame);						
				cout << "Processing frame #" << frameNumb << endl;

#ifdef FILTER_INPUT				
				// Smooth the input image		
				cvSmooth(inputFrame, inputFrame, FILTER_TYPE, FILTER_PARAM_1, FILTER_PARAM_1, FILTER_PARAM_2);
#endif

				tracker->trackFrame(inputFrame, frameNumb);				
				trackedFrame = tracker->getTrackedFrame();
			}
			else
			{
				cout << "Failed to grab a frame from the camera!" << endl;		
				initialized = false;
			}
		}		
		else
			initialized = false; // signals we need to stop tracking
```
	
# ASMTracker Architecture

The ASMTracker could be used into 
