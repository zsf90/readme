Android 多个 Activity 的使用。

创建一个项目 `Examples` ，把默认生成的 `MainActivity` 当成主页，在该页添加多个按钮，点击不同的按钮跳转到对应的页面（`Activity`）。



## 修改页面的顶部标题 Title

```kotlin
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        title = "多页面跳转主页"
    }
}
```

`Activity`类已经为我们提供了 `title`属性，可以直接修改。

![](./images/android_activity_1.jpg)

## 创建一个 Activity

这里我们创建一个 Activity 名为 ButtonDemoActivity，创建完成后，Android Studio 会在 manifests/AndroidManifest.xml 中注册新添加的 Activity。

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.zsf90.examples">

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.Examples">
        <activity <!- ButtonDemoActivity：按钮页 ->
            android:name=".ButtonDemoActivity"
            android:exported="false" />
        <activity <!- MainActivity：主页 ->
            android:name=".MainActivity"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>
</manifest>
```

## Intent 简介

`intent` 是用于表示要执行的某些操作的对象。`intent` 最常见（但肯定不是唯一）的用途是启动 `activity`。`intent` 分为两种类型：隐式和显式。显式 `intent` 非常具体，使用这类 `intent `时您知道要启动的具体 `activity`，通常是您自己应用中的屏幕。

隐式 `intent` 更抽象一些，您可以通过这类` intent` 告知系统要执行的操作类型（例如打开链接、撰写电子邮件或拨打电话），系统则负责确定如何执行相应请求。在操作过程中，您可能已经见过这两类 intent，但自己并不知道。通常情况下，如果要显示自己应用中的 `activity`，您可以使用显式 `intent`。

## 创建一个显式 `intent`

1. 创建一个 `Intent`，并传入 context 以及目标 activity 的类名称。

```kotlin
val intent = Intent(context, DetailActivity::class.java)
```

