# AnyHook 
## 前言
在compose的设定中，只能获取到PreviewActivity的实例，其继承于ComponentActivity。写了这个库，为了在compose的Preview功能时，通过LocalContext.current能获取到对应activity子类的实例。主要是用了动态代理和反射，在ActivityThread最后启动activity时，把要启动的PreviewActivity信息换成我们自己的。

## 使用方法
  在根目录下的build.gradle中添加
  ```gradle
  allprojects {
    repositories {
        ...
        maven { url 'https://jitpack.io' }
    }
  }
  ```
  在app的build.gradle中添加
  ```gradle
      debugImplementation 'com.github.phcdevelop.anyhook:anyhook:latestVersion'
      releaseImplementation 'com.github.phcdevelop.anyhook:anyhook-no-op:latestVersion'
  ```
  
  在application中调用
  ```kotlin
      override fun onCreate() {
        super.onCreate()
        PreviewHook.instance.init(this,MainActivity::class.java)
}
  ```
  
  在自己创建的ComponentActivity的子类（不需要在manifest中注册）中调用
  ```kotlin
  class MPreviewAct: FragmentActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        PreviewHook.instance.onActCreate(this)
    }
}
```

这样，在调用compose的preview功能时通过 LocalContext.current 可以获取到自己的activity实例
