---
layout: post
category : iOS
title: iOS开发(一)：真机调试
date: 2015-06-13 7:00:00
tags: [iOS]
---

苹果真机调试是比较麻烦的，需要代码签名，主要的作用就是确保程序是苹果认证的开发者开发，下面列出主要的步骤。

## 购买开发者帐号

<style>
img {
  max-width: 700px;
  border: solid 2px #ccc;
  padding: 5px;
  border-radius:5px;
}
</style>

之前iOS开发者帐号和Mac开发者帐号需要分开购买，现在都合并为Apple Developer Program了，所以只需要出一份钱了。打开 [https://developer.apple.com/programs/](https://developer.apple.com/programs/), 点击右上角Enroll

<img src="/assets/images/ios/debug-in-device/ios-developer-center.png"/>

点击start your enrollment

<img src="/assets/images/ios/debug-in-device/start-your-enroll.png"/>

选择你是个人开发者还是企业，选择个人开发者

<img src="/assets/images/ios/debug-in-device/select-develop-type.png"/>

然后就按照步骤完成购买，一般需要几天才能审核通过。


## 创建证书请求申请 (Certificate Signing Request)

选择 “Certificate Assistant”，然后点击 “Request Certificate from A Certificate Authority.”

<img src="/assets/images/ios/debug-in-device/request-certification.png" />

填入你的Email 名字，选择Save to Disk, 这是会生成一个CertificateSigningRequest.certSigningRequest 文件

<img src="/assets/images/ios/debug-in-device/certificaiton-info.png"/>

<img src="/assets/images/ios/debug-in-device/save-certification-info.png"/>

## 创建开发者证书

登录开发者中心，选择证书Development, 然后点击右边添加

<img src="/assets/images/ios/debug-in-device/member-center.png" />

选择Development--->iOS App Development

<img src="/assets/images/ios/debug-in-device/select-dev-cert-type.png" />

<img src="/assets/images/ios/debug-in-device/how-to-create-csr.png"/>

这一步上传你刚才的CertificateSigningRequest.certSigningRequest 文件，点击Generate

<img src="/assets/images/ios/debug-in-device/generate-your-certificate.png"/>

Download 你的证书，然后双击就会加入系统

<img src="/assets/images/ios/debug-in-device/generate-dev-certification.png" />

## 注册你的设备

<img src="/assets/images/ios/debug-in-device/register-device.png"/>

如果不知道UUID, 打开iTunes, 双击Serial Number

<img src="/assets/images/ios/debug-in-device/itunes.png" />
<img src="/assets/images/ios/debug-in-device/uuid.png"/>


## 创建App ID

看说明创建你需要的APP ID, 主要是Bundle ID, 一般我们类似这样确保唯一 com.jackwang.nbapp

<img src="/assets/images/ios/debug-in-device/appid.png"/>

<img src="/assets/images/ios/debug-in-device/buildid.png" />


## 创建Provisioning Profile

<img src="/assets/images/ios/debug-in-device/add-provision-profile.png" />

选择iOS development, 点击继续，然后选择你刚的App ID，继续，选择要包含的开发者证书，点击继续，选择要包含的设备, 随后就generate你的provisioning profile.

<img src="/assets/images/ios/debug-in-device/generate-profile.png"/> 

找到对应的Profile 下载后双击即可。


## 项目设置

设置项目的Bundle ID 为之前创建的APP ID

<img src="/assets/images/ios/debug-in-device/xcode-setting-bundle.png"/>

然后选择你对应的Code Sign

<img src="/assets/images/ios/debug-in-device/xcode-code-sign.png" />

## 真机调试

插入你的设备，选择你的设备，点击运行，就可以真机调试了。