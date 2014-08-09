Test Report - FaceTracker Random Crash Bug Report and Fix
=========

Author: Zheng Xu

# Bug Restatements

The program will crash in below circumstances,

1. When it cannot recognize at least one face from the first frame captured, (hereafter no-face-crash) 
2. When the blink module cannot correctly compute the eye-box size; (hereafter no-eye-crash)

# Problem Source 

## No Face Crash

When face could not be recognized from the frame captured, the device object could not be properly released. 

## No Eye Crash

When the BlinkDll module could not locate eyes on the face, a 0x3 matrix would be allocated and then cause crash.  

# Solutions

## No Face Crash 

Fix the release crash problems. 

In `/ASMTrackerDLLWithGUI/ASMTrackerDLL/Controller.cpp`,

`@@ -26,16 +26,20 @@ Controller::Controller(CASMParam & ASMParam)` 
```C
 	// BLINK
 	useBlinkDLL = true;
+	this->myBlinkDLL = NULL;
 	bShowBlinkWin = false;
 	blink_img = cvCreateImage(cvSize(640,480),8,3);
 
 	// EXPRESSION
 	useExpressionDLL = true;
+	this->myExpDLL= NULL;
 	bShowExpWin = false;
 	exp_img = cvCreateImage(cvSize(400, 350), IPL_DEPTH_8U, 3);
 
 	// HEADGESTURE
 	useGestureDLL = true;
+	this->myGestureDLL = NULL;
+	
 
 	nLostTrack = 0;
 }
```

In `@@ -54,21 +58,24 @@ Controller::~Controller()`,

```C
 	// BLINK
 	if (useBlinkDLL)
 	{
-		delete myBlinkDLL;
+		if (myBlinkDLL)
+			delete myBlinkDLL;
 		cvReleaseImage(&blink_img);
 	}
 
 	// EXPRESSION
 	if (useExpressionDLL)
 	{
-		delete myExpDLL;
+		if (myExpDLL)
+			delete myExpDLL;
 		cvReleaseImage(&exp_img);
 	}
 
 	// HEADGESTURE
 	if (useGestureDLL)
 	{
-		delete myGestureDLL;
+		if (myGestureDLL)
+			delete myGestureDLL;
 	}	
 }
```


In `/ASMTrackerDLLWithGUI/ASMTrackerDLL/TrackerDriverDLL.cpp`,
`@@ -42,6 +42,7 @@ CASMTrackerDriver::CASMTrackerDriver(CASMParam *params, int devID)`

```C
 	rawFrame = NULL;
 	trackedFrame = NULL;
 	graphFrame = NULL;
+	dev = NULL;
 	
 	frameNumb = 0;
 }
```



## No Eye Crash

In `/ASMTrackerDLLWithGUI/BlinkDLL/Gaussian.cpp`, 
`@@ -176,10 +176,15 @@ void Gaussian::ModelTrainHSV1_CPP(const IplImage* Img, const CvRect left_rect, c`

```C
 	//////////////////////////////////////////////////////////////////////////////////////////
 	// Get all pixels
+	int nPix     = left_rect.width * left_rect.height + right_rect.width * right_rect.height;
+	if (!nPix) {
+		cout<<"Warning: Cannot find eyebox"<<endl;
+		return;
+	}
 	IplImage* HSVImg = cvCreateImage(cvSize(Img->width, Img->height), Img->depth, Img->nChannels);
 	cvCvtColor(Img, HSVImg, CV_BGR2HSV); 
 
-	int nPix     = left_rect.width * left_rect.height + right_rect.width * right_rect.height;
+	
 	CvMat* Mat   = cvCreateMat(nPix,3, CV_32FC1);
 	CvMat* Sample = cvCreateMat(1, 3, CV_32FC1);
```
