I have yet to investigate what those are, but it's very interesting, and I the more I learn about onnxruntime, the more I see that I still don't knwo.

##Findings and general direction so far:

-Onnxruntime actually creates a CoreML .mlmodelc model and basically uses coremltools to run it. Apparently, Pytorch and Tensorflow have a relatively similar behaviour, but I ididn't test it.  

-By default, ONNX Runtime always places input(s) and output(s) on CPU: it needs to be specified literaly, ideallh input for input, output for output.  
-CoreMLExecutionProvider is incorporated in onnxruntime as CPU only with fails-safe.  
-The state of the affair in onnxruntime allow MPS device to be fully accelerated using coremltoolss code, and when there is a fallback to CPU it's without acceleration.  
-ANE apple neural engine seems, wtihtout proper benchmarking, nor proper instructions, to be more efficient than A GPU that don't know what it's doing.  
-It seems to be as headache giving to code instructions for Metal than configuring some Oracle point of sale, but the benefits will probably motivate someon: CoreImage has to be integreted bringing all needed to manipulate images.    
-Apple being Apple as usual, you can only do what's permitted, Windows you can do anything that's not forbidden, and sometimes you'll have to do something forbidden, and Linux allow to _try_ anyhing.


Confusion between MetalPerformanceShaders and CoreML.
No mention nor uses of proper device instructions, only uses of model creation instruction: MLcomputeunits, by default "all", is a coreml model parameter, not a mps device parameter
I start to think 

and have been tested on M1max and M2air, running full gpu, little gpu, no gpu.
Metal libraries do not stay open when the GPU is at 0%.

It is true there is since inception a converter to coreml  model within Onnxruntimetools https://github.com/microsoft/onnxruntime/blob/0bc71b0c9b817b0e54d9b6f1a7f92eaa1b77ea66/onnxruntime/core/providers/coreml/model/model.mm#L312

I thought we actually had to convert metal language instructions, but could it be a temporary model convertion and an under-the-hood import coremltools?
Very interesting and learning a lot. I always wondered why we passed from metal performance shaders to coreml execution provider.
In rough terms, mps is like a driver, isn't it?
![ss 2023-12-27 at 6 14 56 AM](https://github.com/Oil3/onnxruntime-mac/assets/22565084/29c26aa7-3f75-4d89-aac6-237acbd85a6c)


# onnxruntime-mac :
Unlocking onnxruntime to maximize computing abilities of devices on MacOS, silicon and intel.  


## Silicon
All computing units up and running!

![ss 2023-12-20 at 6 25 26 AM](https://github.com/Oil3/onnxruntime-mac/assets/22565084/6f046a0a-aa08-4c0b-8756-0632dd734704)

![gpu](https://github.com/Oil3/onnxruntime-mac/assets/22565084/10d043dc-c9f5-47aa-81dd-55ad95ca5831)


## Intel x86
Available: onnxruntime with CoreML support for intel macs.  

Done: CoreML provider used  

In progress: eGPU used



![Screenshot 2023-12-14 at 4 48 25 AM](https://github.com/Oil3/onnxruntime-mac/assets/22565084/e9aa631c-712c-40be-9d4b-811485155b60)


  

