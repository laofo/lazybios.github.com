---
layout: post
title: "使用Intent安装APK方法(兼容Android N)"
date: "2016-12-26"
---

Android N之前，通过Intent安装一个APK代码差多不是下面这个样子的:

```java
Intent install = new Intent(Intent.ACTION_VIEW);
install.setFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
File apkFile = new File(Environment.getExternalStorageDirectory() + "/download/" + "app.apk";
install.setDataAndType(Uri.fromFile(apkFile)), "application/vnd.android.package-archive");
startActivity(install)
```

上面代码在Android N(API 26)之前是没有问题的，但是从Android 7.0开始，系统修改了安全机制: 限定应用在默认情况下只能访问自身应用数据。所以当我们想通过File对象访问其它package数据时，就需要借助于ContentProvider、FileProvider这些组件，否则会报`FileUriExposedException`异常。

相应的，显然上面的代码在Android N中会有`FileUriExposedException`的问题，所以我们需要借助FileProvider做出下面的修改。

### 使用FileProvider的步骤

#### 添加Provider到AndroidManifest.xml 
```xml
<provider
    android:name="android.support.v4.content.FileProvider"
    android:authorities="应用包名.fileprovider"
    android:exported="false"
    android:grantUriPermissions="true">

    <meta-data
        android:name="android.support.FILE_PROVIDER_PATHS"
        android:resource="@xml/file_paths"/>

</provider>
```

#### 创建@xml/file_paths对应的资源文件
```xml
<?xml version="1.0" encoding="utf-8"?>
<paths>
    <external-path
        name="download"
        path="" />
    <external-files-path
        name="Download"
        path="" />
</paths>
```

paths里面用来放你需要访问的非本应用路径，只有通过这里申明后，才能在程序中使用不报错。

+ files-path 对应 `Context.getFilesDir()`
+ cache-path 对应 `getCacheDir()`
+ external-path 对应 `Environment.getExternalStorageDirectory()`
+ external-files-path 对应 `Context#getExternalFilesDir(String) Context.getExternalFilesDir(null)`
+ external-cache-path 对应 `Context.getExternalCacheDir()`

具体其中的path和name就需要你自己来根据需求确定了。

#### 应用代码
```java
Intent install = new Intent(Intent.ACTION_VIEW);
install.setFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
File apkFile = new File(Environment.getExternalStorageDirectory() + "/download/" + "app.apk";

if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.N) {
    install.setFlags(Intent.FLAG_GRANT_READ_URI_PERMISSION);
    Uri contentUri = FileProvider.getUriForFile(context, "应用报名.fileProvider", apkFile);
    install.setDataAndType(contentUri, "application/vnd.android.package-archive");
} else {
    install.setDataAndType(Uri.fromFile(apkFile), "application/vnd.android.package-archive");
}

startActivity(install);
```

-完-

### 参考引用
+ [http://t.cn/RIHOWTE](http://t.cn/RIHOWTE)
