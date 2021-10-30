先给一个不带图片的按钮

```xml
<Button
    android:id="@+id/btn_demo_toggle_stats_bar"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    app:layout_constraintTop_toTopOf="parent"
    app:layout_constraintLeft_toLeftOf="parent"
    android:text="显示状态栏"/>
```

带图标的按钮

```xml
<Button
    android:id="@+id/btn_demo_toggle_stats_bar"
    android:layout_width="wrap_content"
    android:layout_height="69dp"
    android:drawableRight="@drawable/play"
    android:text="显示状态栏"
    app:layout_constraintLeft_toLeftOf="parent"
    app:layout_constraintTop_toTopOf="parent" />
```



## 一个例子

```kotlin
package com.zsf90.examples

import android.app.Application
import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle
import android.view.View
import android.widget.Button
import android.widget.ImageButton
import android.widget.Toast

class ButtonDemoActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_button_demo)

        title = "按钮实例页面"

        val mBtnGeneral = findViewById<Button>(R.id.btn_demo_toggle_stats_bar)
        mBtnGeneral.setOnClickListener(View.OnClickListener {
            Toast.makeText(applicationContext,"你点击了普通按钮", Toast.LENGTH_SHORT).show()
        })

        val mBtnImageDemo = findViewById<ImageButton>(R.id.btn_demo_image_button)
        mBtnImageDemo.setOnClickListener(View.OnClickListener {
            Toast.makeText(applicationContext, "你点击了图片按钮", Toast.LENGTH_SHORT).show()
        })
    }
}
```

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".ButtonDemoActivity"
    android:background="@color/yellow_300"
    android:paddingHorizontal="4dp">

    <Button
        android:id="@+id/btn_demo_toggle_stats_bar"
        android:layout_width="wrap_content"
        android:layout_height="69dp"
        android:drawableRight="@drawable/play"
        android:text="显示状态栏"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <ImageButton
        android:id="@+id/btn_demo_image_button"
        android:layout_width="65dp"
        android:layout_height="47dp"
        app:layout_constraintBottom_toBottomOf="@+id/btn_demo_toggle_stats_bar"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toEndOf="@+id/btn_demo_toggle_stats_bar"
        app:layout_constraintTop_toTopOf="parent"
        app:srcCompat="@drawable/play" />

</androidx.constraintlayout.widget.ConstraintLayout>
```

