[中文版文档(Chinese version)](https://github.com/yangzhongke/Zack.OpenCVSharp.Ext/blob/main/README_CN.md)

# Zack.OpenCVSharp.Ext
It is an extension library of OpenCvSharp. It provides ResourceTracker, which can facilitate the resources management of Mat and other unmanaged resources. It also provide a class, named np, which is a .NET native and  managed version of Numpy.

NuGet Package

```
Install-Package Zack.OpenCVSharp.Ext
```
## ResourceTracker

**ResourceTracker has been contributed to the latest version of OpenCvSharp4, and it has been renamed to ResourcesTracker, please see the [document of OpenCvSharp4](https://github.com/shimat/opencvsharp) ，[PullRequest](https://github.com/shimat/opencvsharp/pull/1110). The ResourceTracker here will not be maintained.**

In OpenCVSharp, Mat and MatExpr have unmanaged resources, so they should be disposed be code. However, the code is verbose. Worst of all, every operator, like +,-,* and others, will create new objects, and they should by disposed one by one. The verbose code is as follow.
```csharp
using (Mat mat1 = new Mat(new Size(100, 100), MatType.CV_8UC3))
using (Mat mat2 = mat1 * 0.8)
using (Mat mat3 = 255-mat2)
{
	Mat[] mats1 = mat3.Split();
	using (Mat mat4 = new Mat())
	{
		Cv2.Merge(new Mat[] { mats1[0], mats1[1], mats1[2] }, mat4);
	}
	foreach(var m in mats1)
	{
		m.Dispose();
	}
}
```

The class ResourceTracker is used for managing OpenCV resources, like Mat, MatExpr, etc.
* T(). The method T() of ResourceTracker is used for add OpenCV objects to the tracking records, then the object is  returned, so the T() method acts as a wrapper around the resource to be disposed. After Dispose() of ResourceTracker is called, all the resoruces kept by ResourceTracker will be disposed. The method T() can take one object and an array of objects.
* NewMat(). The method NewMat() is a combination of T(new Mat(...)) 

Sample code:

```csharp
using (ResourceTracker t = new ResourceTracker())
{
	Mat picMat = t.T(Cv2.ImRead("bg.png"));
	Mat mat1 = t.T(255 - picMat);
	Mat mat2 = t.T(np.zeros_like(mat1));
	Mat mat3 = t.NewMat();
	Mat mat4 = t.T(np.array(new byte[] { 33, 88, 99 }));
	Mat mat5 = t.T(np.array(33, 88, 99));
	Mat mat6 = t.T(255 - t.T(picMat * 0.8));
	Mat[] mats1 = t.T(picMat.Split());
	Mat[] mats2 = new Mat[] { mats1[0], mats1[1], mats1[2], t.T(np.zeros_like(picMat)) };
	Cv2.Merge(mats2, mat3);
}
```

Because every operator, like +,-,* and others, will create a new object, so they should be wrapped by T(). For example: t.T(255 - t.T(picMat * 0.8))

## np
The class np is like the np of NumPy in Python.
Because the syntax of Python is different from that of C#, I don't the .NET binding version of Numpy is a good idea. Furthermore, the ported packages, like Numpy.NET, cannot take advantages of C# syntax sugar, like lambda.

Therefore, I created the managed version of np, which provies zeros_like, array, where. I'm a newbie to the Numpy field, and I didn't do much research in Numpy, so I didn't implement most methods of Numpy. I will be appreciated if any one can contribute more methods to np.cs.

## GreenScreenRemovalDemo
The project GreenScreenRemovalDemo is a practical case. It can remove the green screen of a video or the images from the webcamera.

An article of this project: [Extract portrait and replace background(image matting) by code, using C# .NET and OpenCVSharp](https://www.reddit.com/r/csharp/comments/ker9fx/extract_portrait_and_replace_backgroundimage/)

Binaries of GreenScreenRemovalDemo:
* Windows(X86 and X64): [Download From OneDrive](https://1drv.ms/u/s!ArtUX5uRoj_cmWWM1xf0CfVMx4FI?e=YupHDl)
* MacOS (osx.10.15-x64):[Download From OneDrive](https://1drv.ms/u/s!ArtUX5uRoj_cmWIlLKUw77KVx0r7?e=zfdafg)
* centos7-x64: [Download From OneDrive](https://1drv.ms/u/s!ArtUX5uRoj_cmWMwztai5lT-ag8n?e=rTiejq)
* ubuntu.16.04-x64:[Download From OneDrive](https://1drv.ms/u/s!ArtUX5uRoj_cmWbBCY5TRpcwjb_y?e=xWBBkx)
* debian.10-amd64:[Download From OneDrive](https://1drv.ms/u/s!ArtUX5uRoj_cmWQcrpCEMT3cNjBz?e=0fg5XV)