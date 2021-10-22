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

