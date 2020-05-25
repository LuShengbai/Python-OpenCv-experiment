#                                       实验6、Python-OpenCV宽度测量

## 一、  题目描述

 测量所给图片的高度，即上下边缘间的距离。

![gongjian1](http://p.ananas.chaoxing.com/star3/origin/487b38904a9610338b4101f3c16e7cc8.png)

**思路：**

1. 将图片进行阈值操作得到二值化图片。
2. 截取只包含上下边框的部分，以便于后续的轮廓提取
3. 轮廓检测
4. 得到结果

--------------------------



##  二、 实现过程

#### 1.用于给图片添加中文字符

```python
#用于给图片添加中文字符
def ImgText_CN(img, text, left, top, textColor=(0, 255, 0), textSize=20):
    if (isinstance(img, np.ndarray)):  #判断是否为OpenCV图片类型
        img = Image.fromarray(cv2.cvtColor(img, cv2.COLOR_BGR2RGB))
    draw = ImageDraw.Draw(img)
    fontText = ImageFont.truetype(r'C:\Windows\Fonts\simsun.ttc', textSize, encoding="utf-8")          ##中文字体
    draw.text((left, top), text, textColor, font=fontText)     #写文字
    return cv2.cvtColor(np.asarray(img), cv2.COLOR_RGB2BGR)
```



#### 2.实现图片反色功能

```python
#实现图片反色功能
def PointInvert(img):
    height, width = img.shape        #获取图片尺寸
    for i in range(height):
        for j in range(width):
            pi = img[i, j]
            img[i, j] = 255 - pi
    return img
```

#### 4.边缘检测

```python
# canny边缘检测
edges = cv2.Canny(th, 30, 70) 
res=PointInvert(edges)                           #颜色反转
#显示图片
cv2.imshow('original', th)                       #显示二值化后的图，主题为白色，背景为黑色 更加容易找出轮廓
key = cv2.waitKey(0)
if key==27: #按esc键时，关闭所有窗口
    print(key)
    cv2.destroyAllWindows()
```

#### 5.轮廓操作

```python
contours, hierarchy = cv2.findContours(th, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)       #得到轮廓

cnt = contours[0]               #取出轮廓

x, y, w, h = cv2.boundingRect(cnt)         #用一个矩形将轮廓包围

img_gray = cv2.cvtColor(res, cv2.COLOR_GRAY2BGR)               #将灰度转化为彩色图片方便画图

cv2.line(img_gray, (x, y), (x + w, y), (0,0,255), 2, 5)         #上边缘
cv2.line(img_gray, (x, y+h), (x + w, y+h), (0, 0, 255), 2, 5)    #下边缘

img1[80:230, 90:230] = img_gray          #用带有上下轮廓的图替换掉原图的对应部分
```

#### 6.显示图片

```python
res1=ImgText_CN(img1, '宽度%d'%h, 25, 25, textColor=(0, 255, 0), textSize=30)    #绘制文字
#显示图片 
cv2.imshow('original', res1)
key = cv2.waitKey(0)
if key==27: #按esc键时，关闭所有窗口
    print(key)
    cv2.destroyAllWindows()
```



## 三、  运行结果（效果）

![image-20200524113856389](C:\Users\Shinebay\AppData\Roaming\Typora\typora-user-images\image-20200524113856389.png)

![image-20200524113909356](C:\Users\Shinebay\AppData\Roaming\Typora\typora-user-images\image-20200524113909356.png)

## 四、 问题及解决方法

红色轮廓没有显示，解决方案：将灰度图转化为彩色图


##  五、 实验总结

学习了OpenCV的宽度测量，遇到了作业问题自己解决了，锻炼了自己的能力。

