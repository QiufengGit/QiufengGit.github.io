# 图床设置流程








记录一次图床配置流程

&lt;!--more--&gt;

## 前情提要

- 阿里云OSS（对象存储）（收费：9元/年）
- PicGo

## 阿里云OSS服务设置

### 基础OSS服务搭建

1. 登录阿里云

2. 对象存储OSS

![](https://qiu-media.oss-cn-wuhan-lr.aliyuncs.com/img/20240215140210.png)

3. 选择并购买OSS资源包（9元/年）

![image-20240215140408500](https://qiu-media.oss-cn-wuhan-lr.aliyuncs.com/img/image-20240215140408500.png)

4. 创建bucket

   注：

   - 读写权限：公共读

   ![image-20240215140556329](https://qiu-media.oss-cn-wuhan-lr.aliyuncs.com/img/image-20240215140556329.png)

   

### 用户设置

   1. 创建专门用于访问OSS的用户

      注：

      记录ACCESSID和ACCESSSECRET（仅创建时可以查看，后续无法再次看到）

   ![image-20240215140959648](https://qiu-media.oss-cn-wuhan-lr.aliyuncs.com/img/image-20240215140959648.png)

   2. 添加OSS权限

   ![image-20240215141049781](https://qiu-media.oss-cn-wuhan-lr.aliyuncs.com/img/image-20240215141049781.png)

## 配置PicGo

1、下载并安装PicGo

[Releases · Molunerfinn/PicGo (github.com)](https://github.com/Molunerfinn/PicGo/releases)

2、打开PicGo，打开图床设置，点击阿里云OSS，如下设置即可

![image-20240215141524146](https://qiu-media.oss-cn-wuhan-lr.aliyuncs.com/img/image-20240215141524146.png)





## MarkDown结合

1、偏好设置-&gt;图像    确定上传服务

![image-20240215141658998](https://qiu-media.oss-cn-wuhan-lr.aliyuncs.com/img/image-20240215141658998.png)



2、Markdown完成后点击上传所有图片


---

> 作者: [qiu](https://qiufenggit.github.io/)  
> URL: http://localhost:1313/posts/eb9b3b7/  

