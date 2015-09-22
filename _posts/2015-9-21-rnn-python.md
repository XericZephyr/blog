---
layout: post
title:  "Recurrent Neural Network Basics"
date:   2015-9-21 22:11:00
categories: python rnn
---


Last week, I put most of my effort into studying the Recurrent Neural Network, mostly regarding to some basic concepts and softwares. Another attempt is to do some caffe Python interface testing. 

## Recurrent Nerual Network 

### Software 

#### Caffe
 
Caffe is a deep learning framework made with expression, speed, and modularity in mind, which has several variants, e.g.

* [Official Caffe](https://github.com/BVLC/caffe)
* [NLPCaffe](https://github.com/Russell91/nlpcaffe)
* [FastFPBP](https://github.com/myfavouritekk/caffe/tree/FastFPBP)
 
#### Torch 

[Torch](http://torch.ch/) is a scientific computing framework with wide support for machine learning algorithms.

**I planned to start learning Torch very soon since it has very good support for RNN. Probably much better for RNN researchers.**

### Several Useful Links

- [*Hacker's Note to Deep Learning*](http://karpathy.github.io/neuralnets/)
	
	Code-based knowledge in Deep Learning.
- [*Unreasonably Effectiveness of RNN*](http://karpathy.github.io/2015/05/21/rnn-effectiveness/)
	
	Several fancy generation-related applications using RNN 
- [*Awesome RNN*](http://jiwonkim.org/awesome-rnn/)

	A relatively complete survey in recent progress in applying RNN to different applications. 
	
## Contributions 

In the last week, my contributions are generally with Python coding. I become aware of a very powerful tool called with statement in Python. Moreover, I refrained several of my old Python code using with-statment. The result is very pleasing. Below is an incomplete list in recent important code updates: 

*	PyCaffe: Training Interface 
*	Pycaffe: Network Manipulation
* 	ProtoText: A very powerful wrapper class for protobuf
	
## Miscelleneous

Last but not least, I learned several other important things last week.

* **Place set_mode_gpu before constructing network in caffe. [Ref](https://github.com/BVLC/caffe/issues/3053)** 
	
	
	
	

