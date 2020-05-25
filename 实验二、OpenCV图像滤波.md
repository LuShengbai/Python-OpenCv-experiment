#                       实验二、OpenCV图像滤波

 

## 一、  题目描述

对下面的图片进行滤波和边缘提取操作，请详细地记录每一步操作的步骤。![img](file:///C:\Users\Shinebay\AppData\Local\Temp\msohtmlclip1\01\clip_image002.jpg)

滤波操作可以用来过滤噪声，常见噪声有[椒盐噪声](https://baike.baidu.com/item/椒盐噪声/3455958?fr=aladdin)和[高斯噪声](https://baike.baidu.com/item/高斯噪声)，椒盐噪声可以理解为斑点，随机出现在图像中的黑点或白点；高斯噪声可以理解为拍摄图片时由于光照等原因造成的噪声。

##  二、  实现过程

###  1. 加载原图

```python
import cv2 

#加载图片

img=cv2.imread("test14.bmp",0) 

imgzi = cv2.putText(img, 'original', (40,25),cv2.FONT_HERSHEY_SIMPLEX, 1, (255,0, 0), 2) 

#显示图片

cv2.imshow('canny', img) 

key = cv2.waitKey(0) 

if key==27: #按esc键时，关闭所有窗口

print(key) 

cv2.destroyAllWindows()
```



 

### 2.均值滤波

 

​     均值滤波是一种最简单的滤波处理，它取的是卷积核区域内元素的均值，用`cv2.blur()`实现，如3×3的卷积核：

```python
mg=cv2.imread("test14.bmp",0) 

blur = cv2.blur(img, (3, 3)) # 均值模糊

imgzi = cv2.putText(img, 'averagefilter ', (40,25),cv2.FONT_HERSHEY_SIMPLEX, 1, (255,0, 0), 2) 
```



​    

### 3.中值滤波

中值又叫中位数，是所有数排序后取中间的值。中值滤波就是用区域内的中值来代替本像素值，所以那种孤立的斑点，如0或255很容易消除掉，适用于去除椒盐噪声和斑点噪声。中值是一种非线性操作，效率相比前面几种线性滤波要慢。

```python
img = cv2.imread('test14.bmp', 0) 

# 均值滤波vs中值滤波

median = cv2.medianBlur(img, 5) # 中值滤波

 imgzi = cv2.putText(img, 'medianfilter ', (40,25),cv2.FONT_HERSHEY_SIMPLEX, 1, (255,0, 0), 2) 
```



​    

### 4. 高斯滤波

​     ![http://blog.codec.wang/cv2_gaussian_kernel_function_theory.jpg](file:///C:\Users\Shinebay\AppData\Local\Temp\msohtmlclip1\01\clip_image003.jpg)

 

​    OpenCV中对应函数为`cv2.GaussianBlur(src,ksize,sigmaX)`：

​           

 ```python
img = cv2.imread('test14.bmp') 

#高斯滤波

gaussian = cv2.GaussianBlur(img, (5, 5), 1) #高斯滤波

imgzi = cv2.putText(img, 'gaussfilter ', (40,25),cv2.FONT_HERSHEY_SIMPLEX, 1, (255,0, 0), 2) 


 ```





### 5. 高斯边缘检测

原理：首先对图像做高斯滤波，然后再求其拉普拉斯（Laplacian）二阶导数。

即图像与 Laplacian of the Gaussian function 进行滤波运算。

最后，通过检测滤波结果的零交叉（Zero crossings）可以获得图像或物体的边缘。

因而，也被简称为Laplacian-of-Gaussian (LoG)算子。

在第一步的基础上，可以通过拉普拉斯边缘检测来实现这一功能。Laplacian函数简介：dst = cv.Laplacian(src, ddepth[, dst[, ksize[, scale[, delta[, borderType]]]]])。

 ```python
import cv2 

import numpy as np 

# Load the image in greyscale

img = cv2.imread('test14.bmp',0) 

# Apply Gaussian Blur

blur = cv2.GaussianBlur(img,(3,3),0) 

# Apply Laplacian operator in some higher datatype

laplacian = cv2.Laplacian(blur,cv2.CV_64F,5)

 
 ```





# 三、  运行结果（效果）

# ![img](file:///C:\Users\Shinebay\AppData\Local\Temp\msohtmlclip1\01\clip_image004.png)

# ![img](file:///C:\Users\Shinebay\AppData\Local\Temp\msohtmlclip1\01\clip_image005.png)

#  

# ![img](file:///C:\Users\Shinebay\AppData\Local\Temp\msohtmlclip1\01\clip_image006.png)

#  

# ![img](file:///C:\Users\Shinebay\AppData\Local\Temp\msohtmlclip1\01\clip_image007.png)

# ![img](file:///C:\Users\Shinebay\AppData\Local\Temp\msohtmlclip1\01\clip_image008.png)

# 四、  问题及解决方法

# ![img](file:///C:\Users\Shinebay\AppData\Local\Temp\msohtmlclip1\01\clip_image010.jpg)

老师给的第五步gauss边缘检测没有搜索到相关的资料，最后我查阅了官方文档，上面的GAUSS指高斯---拉普拉斯算法。我查阅了资料，完成了此次作业。解决方法：进行laplacian边缘检测得以实现。

Puttext函数不能输出中文,解决方法：使用英文输出

#  

# 五、  实验总结

我们要合理运用网络资源，特别时官方文档，学会查阅资料。

 