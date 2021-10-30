# Android Layout布局

Android 常用布局：

1. ConstraintLayout // 约束布局
2. LinearLayout // 线性布局（Horizontal | Vertical）
3. RelativeLayout // 相对布局
4. FrameLayout // 帧布局
5. TableLayout // 表格布局
6. TableRow // 表格行
7. Space // 空间

## LinearLayout 线性布局

### 常用属性

* `android:id`
* `android:layout_width` 宽
* `android:layout_height` 高
* `android:background` 背景
* `android:layout_margin` 外边距
* `android:layout_padding` 内边距
* `android:orientation` 方向
* `android:gravity`  **LinearLayout** 中子元素的对齐方式(用在 LinearLayout上)
* `android:layout_gravity` 当前元素在父 LinearLayout 中的对齐方式（用在 LinearLayout的子元素上）
* `android:layout_weight`
* `android:baselineAligned` 当设置为**false**时，防止布局对齐其子元素的基线。  
* 

## RelativeLayout 相对布局

### 常用属性

* android:layout_toLeftOf
* android:layout_toRightOf
* android:layout_alignBottom
* android:layout_alignParentBottom
* android:layout_below

## ConstraintLayout // 约束布局

