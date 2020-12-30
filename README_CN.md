# Zack.OpenCVSharp.Ext
����OpenCvSharp��һ����չ�⡣�������ResourceTracker��������Mat����Դ�Ĺ�����Ҳ�ṩ�����ֽ���np���࣬����Numpy��.NETԭ���йܰ汾��

NuGet ��

```
Install-Package Zack.OpenCVSharp.Ext
```
## ResourceTracker

**ResourceTracker�Ѿ����׵�OpenCvSharp4�����°��У����Ҹ���ΪResourceTrackers������鿴[OpenCvSharp4�ĵ�](https://github.com/shimat/opencvsharp) ��[PullRequest˵��](https://github.com/shimat/opencvsharp/pull/1110)�������ResourceTracker��������ά����**


��OpenCVSharp�У�Mat �� MatExpr�����з��й���Դ������������Ҫͨ�������ֶ��ͷš����ǣ�����ǳ����¡��������ǣ�+��-��*�������ÿ�ζ��ᴴ��һ���µĶ�����Щ������Ҫ�ͷš����µĴ�����������ġ�

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

ResourceTracker����������OpenCV����Դ������ Mat�� MatExpr�ȡ�
* T(). ResourceTracker���T()�������ڰ�OpenCV���������ټ�¼��Ȼ���ٰѶ��󷵻أ�����T()�������൱��һ�����ͷ���Դ�İ���������ResourceTracker��� Dispose()���������ú�ResourceTracker���ٵ�������Դ���ᱻ�ͷš�T()�������Ը���һ���������һ���������顣
* NewMat(). ���������T(new Mat(...)) ��һ���򻯡�

���Ӵ��룺

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

��Ϊ+��-��*�������ÿ�ζ��ᴴ��һ���µĶ�����Щ������Ҫ�ͷţ����ǿ���ʹ��T()���а��������磺t.T(255 - t.T(picMat * 0.8))

## np
np�����Python���npһ������ΪPython���﷨��C#�Ĳ�һ��������ΪNumpy��.NET�󶨰治����õķ��������ң�Numpy.NET֮�����ֲ���޷��������C#��lambda֮����﷨�ǡ�
��ˣ��Ҵ�����һ��np���йܰ汾�����ṩ��zeros_like, array, where�ȷ���������Numpy��������ˣ�����Numpy����Ҳ��û�����о��������û��ʵ��Numpy�еĴ󲿷ַ����������ԭ���׸�����뵽np�࣬�һ�ǳ����ġ�

## GreenScreenRemovalDemo
GreenScreenRemovalDemo��Ŀ��һ��ʵ�ʵİ������������Ƴ���Ƶ��������ͷ�е���Ļ��

���������Ŀ������: [���ȥ��������Ļ��ͼ������.NET+OpenCVSharp](https://www.bilibili.com/read/cv8850462)

GreenScreenRemovalDemo�Ķ����ƿ�ִ�г���:
* Windows(X86 and X64): [�� �ٶ���������](https://pan.baidu.com/s/1mRs4etacjO-jb1b7iH1R3Q)  ����ȡ�룺6cu7��||  [�� OneDrive����](https://1drv.ms/u/s!ArtUX5uRoj_cmWWM1xf0CfVMx4FI?e=YupHDl)
* MacOS (osx.10.15-x64):[�� �ٶ���������](https://pan.baidu.com/s/1bSxtpFxnfXwl1VfhpGO-rQ) ����ȡ�룺w7ki��||  [�� OneDrive����](https://1drv.ms/u/s!ArtUX5uRoj_cmWIlLKUw77KVx0r7?e=zfdafg)
* centos7-x64: [�� OneDrive����](https://1drv.ms/u/s!ArtUX5uRoj_cmWMwztai5lT-ag8n?e=rTiejq)
* ubuntu.16.04-x64: [�� OneDrive����](https://1drv.ms/u/s!ArtUX5uRoj_cmWbBCY5TRpcwjb_y?e=xWBBkx)
* debian.10-amd64:[�� OneDrive����](https://1drv.ms/u/s!ArtUX5uRoj_cmWQcrpCEMT3cNjBz?e=0fg5XV)