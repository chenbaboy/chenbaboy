# iOS H5马甲包打包与部署指南

本文档提供了如何在Mac上使用Xcode打开、编译和部署U查查iOS应用的详细指南。

## 目录

1. [项目概述](#项目概述)
2. [环境要求](#环境要求)
3. [打开项目](#打开项目)
4. [配置开发者账号](#配置开发者账号)
5. [编译与测试](#编译与测试)
6. [生成IPA文件](#生成ipa文件)
7. [安装到设备](#安装到设备)
8. [上传到App Store](#上传到app-store)
9. [常见问题](#常见问题)

## 项目概述

U查查是一个iOS H5马甲包应用，主要功能是：
- 加载指定网站URL: https://www.usdtchacha.com/browser
- 要求用户授权相册权限，拒绝授权则无法使用
- 使用自定义图标和启动页

## 环境要求

- Mac电脑（必须）
- Xcode 14.0或更高版本
- 有效的Apple开发者账号（个人或企业账号）
- iOS设备（用于测试，可选）

## 打开项目

1. 解压下载的`UChacha_iOS_Project.zip`文件
2. 打开Xcode
3. 在Xcode中选择"File" > "Open..."
4. 导航到解压后的文件夹，选择`UChacha.xcodeproj`文件
5. 点击"Open"按钮

## 配置开发者账号

1. 在Xcode中选择左侧导航栏中的项目文件（蓝色图标）
2. 在"Signing & Capabilities"选项卡中：
   - 确保"Automatically manage signing"选项已勾选
   - 在"Team"下拉菜单中选择您的开发者账号
   - 如果没有显示您的账号，点击"Add Account..."并登录您的Apple ID

3. 如果需要修改Bundle ID：
   - 默认Bundle ID是`com.company.ucaccacom`
   - 如果此ID已被使用，您可以修改为自己的Bundle ID
   - 确保格式为`com.yourcompany.appname`

## 编译与测试

### 模拟器测试

1. 在Xcode顶部工具栏中，选择一个iOS模拟器（如iPhone 14）
2. 点击运行按钮（三角形图标）或按下`Cmd+R`
3. 应用将在模拟器中启动
4. 测试相册权限和网页加载功能

### 真机测试

1. 使用Lightning或USB-C线缆将iOS设备连接到Mac
2. 在Xcode顶部工具栏中，选择您的设备
3. 首次在真机上运行时，可能需要在iOS设备上信任开发者证书：
   - 在iOS设备上，前往"设置" > "通用" > "设备管理"
   - 找到您的开发者证书并点击"信任"
4. 点击运行按钮或按下`Cmd+R`
5. 应用将在您的设备上安装并启动

## 生成IPA文件

### 开发版IPA（用于测试）

1. 在Xcode中，选择"Product" > "Archive"
2. 等待归档过程完成
3. 在出现的Organizer窗口中，选择刚创建的归档
4. 点击"Distribute App"按钮
5. 选择"Development"分发方式
6. 按照向导完成剩余步骤
7. 选择保存IPA文件的位置

### App Store版IPA（用于上架）

1. 在Xcode中，选择"Product" > "Archive"
2. 等待归档过程完成
3. 在出现的Organizer窗口中，选择刚创建的归档
4. 点击"Distribute App"按钮
5. 选择"App Store Connect"分发方式
6. 按照向导完成剩余步骤
7. 上传完成后，您可以在App Store Connect网站上继续配置应用信息

## 安装到设备

### 使用Xcode直接安装

按照[真机测试](#真机测试)部分的步骤操作。

### 使用TestFlight安装（推荐）

1. 将应用上传到App Store Connect（参见[App Store版IPA](#app-store版ipa用于上架)）
2. 登录[App Store Connect](https://appstoreconnect.apple.com/)
3. 导航到"TestFlight"选项卡
4. 添加内部或外部测试人员
5. 测试人员将收到邀请邮件，并可通过TestFlight应用安装您的应用

### 使用企业分发安装（仅企业账号）

1. 生成企业分发IPA（在归档后选择"Enterprise"分发方式）
2. 将IPA文件和manifest.plist上传到您的服务器
3. 创建安装链接：`itms-services://?action=download-manifest&url=https://your-server.com/path/to/manifest.plist`
4. 用户可通过Safari访问此链接安装应用

## 上传到App Store

1. 确保您已在[App Store Connect](https://appstoreconnect.apple.com/)创建了应用记录
2. 按照[App Store版IPA](#app-store版ipa用于上架)部分的步骤生成并上传IPA
3. 上传完成后，登录App Store Connect配置以下信息：
   - 应用截图
   - 应用描述
   - 关键词
   - 支持URL
   - 隐私政策URL
   - 年龄分级
4. 提交审核
5. 等待Apple审核（通常需要1-3天）

## 常见问题

### 证书和配置文件问题

**问题**: Xcode显示签名错误或证书问题  
**解决方案**: 
1. 确保您的开发者账号有效
2. 在Xcode中选择"Preferences" > "Accounts"，点击您的账号，然后点击"Manage Certificates..."
3. 如有需要，创建新的开发和分发证书
4. 尝试在项目设置中取消勾选"Automatically manage signing"，然后重新勾选

### 相册权限问题

**问题**: 应用无法正确请求相册权限  
**解决方案**:
1. 确认Info.plist中包含`NSPhotoLibraryUsageDescription`键
2. 确保描述文本清晰说明为何需要相册权限
3. 在iOS设置中重置应用权限：设置 > 通用 > 重置 > 重置位置与隐私

### 网页加载问题

**问题**: 应用无法加载指定网站  
**解决方案**:
1. 确认目标URL是否可访问
2. 检查Info.plist中是否允许HTTP连接（如果网站是HTTP而非HTTPS）
3. 在ViewController.swift中检查WebView配置

### 上传到App Store被拒

**问题**: Apple拒绝了应用  
**解决方案**:
1. 仔细阅读拒绝原因
2. 常见拒绝原因包括：
   - 元数据不完整
   - 应用功能有限
   - 隐私政策缺失
   - 应用崩溃或性能问题
3. 修复问题后重新提交

## 结语

按照本指南操作，您应该能够成功打包和部署U查查iOS应用。如果遇到任何问题，请参考[Apple开发者文档](https://developer.apple.com/documentation/)或在开发者论坛寻求帮助。
