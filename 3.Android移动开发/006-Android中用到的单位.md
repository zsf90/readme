## Android 常用单位有：

### 1.`dp(dip)` 

即设备无关像素(device independent pixels)，这种尺寸单位在不同设备上的物理大小相同。

### 2.`px`

即像素(pixel)，这个不用多说。

### 3.`pt`

通常用来作为字体的尺寸单位，1 pt相当于1/72英寸。

### 4.`inch`

英寸，1 英寸约等于2.54厘米，主要用来描述手机屏幕的大小。

### 5.`sp`

大部分人只知道它通常用作字体的尺寸单位，实际大小还与具体设备上的用户设定有关。(如果你对"sp"的了解停留于此，那么看完这篇文章后你会更透彻的理解它^ _ ^)



在上面几种常见的尺寸单位，`dp`和`sp`可以看做是`虚拟`尺寸。其中`dp`是与`设备无关`的虚拟像素单位，开发者为`UI`控件指定以`dp`为单位的大小后，在不同屏幕密度的`Android`设备上便能够具有相同的物理尺寸。`dp`的出现让开发者无需关注屏幕密度、物理像素之间的换算关系。`sp`则与`dp`相似，但它主要用作字体的尺寸单位，与`dp`的区别是：`Android`系统支持用户设定字体大小，因而`sp`的实际大小还会根据用户设定在原基础上进行缩放。

## DPI（屏幕密度 dots per inch）

| 名称   | 描述                                                         |
| ------ | ------------------------------------------------------------ |
| ldpi   | 对应的dpi范围为0 ~ 120，也就是说每英寸有0到120个像素点的屏幕的屏幕密度都属于ldpi |
| mdpi   | dpi范围为120 ~ 160                                           |
| hdpi   | dpi范围为160 ~ 240                                           |
| xhdpi  | dpi范围为240~320                                             |
| xxhdpi | dpi范围为320~480                                             |

在实际开发中，通常以dpi值120、160、240、320、480分别指代ldpi、mdpi、hdpi、xhdpi、xxhdpi。通常屏幕密度越大的手机显示的图像会越细腻。可以通过如下代码获取当前Android设备的屏幕密度：

```java
private void getDpi() {
    DisplayMetrics dm = getResources().getDisplayMetrics();
    Log.i("TAG", "density = " + dm.density);
    Log.i("TAG", "densityDpi = " + dm.densityDpi);
}
```

若我们在一台屏幕密度为320dpi的Android手机上运行以上代码，会得到如下输出：

```shell
density = 2
densityDpi = 320
```

上面输出中的densityDpi就是Android手机屏幕的dpi值，那么density是什么呢？实际上它代表的是当前屏幕的dpi值与基准dpi值的比值，这个基准dpi值为160。

## dp

上面我们提到了选择dpi值160作为基准屏幕密度，这个基准屏幕密度人为建立起了dp与px间的关系：在dpi为160的Android设备上，1 dp = 1px。假设x为某UI控件以px为单位的大小，y为同一UI控件以dp为单位的大小，densityDpi表示屏幕密度，则x与y的关系为：x = y * densityDpi / 160。

## 尺寸单位详解之sp

在介绍sp之前，我们先来一起看下TypedValue类中包含的一个用户将dp、sp等单位转换为px的静态方法：

```java
public static float applyDimension(int unit, float value,DisplayMetrics metrics) {
    switch (unit) {
        case COMPLEX_UNIT_PX:
        	return value;
        case COMPLEX_UNIT_DIP:
        	return value * metrics.density;
        case COMPLEX_UNIT_SP:
        	return value * metrics.scaledDensity;
        case COMPLEX_UNIT_PT:
        	return value * metrics.xdpi * (1.0f/72);
        case COMPLEX_UNIT_IN:
        	return value * metrics.xdpi;
        case COMPLEX_UNIT_MM:
        	return value * metrics.xdpi * (1.0f/25.4f);
    }
    return 0;
}
```

若要将dp转换为px，会执行如下代码：

```java
return value * metrics.density;
```

`density`我们在前面介绍过，指的是当前`dpi`与基准`dpi(160)`的比值。`density`的计算方式就是当前屏幕的`dpi`除以`160`。也就是说，在屏幕的`dpi`为`120`、`160`、`320`、`480`时，`density`的值分别为`0.75`、`1`、`2`、`3`。

若要将`sp`转换为`px`，则会执行如下代码：

```java
return value * metrics.scaledDensity;
```

可以看到，`sp`转换为`px`的计算公式与`dp`转换为`px`时相似，那么`scaledDensity`是什么呢？实际上，`scaledDensity`不同于`density`，`scaledDensity`是可以动态改变的，当用户改变了`Android`设备的字体缩放比例时，`scaledDensity`的值就会发生变化。`scaledDensity`的计算公式为：`scaledDensity = density * fontScale`。其中`fontScale`代表用户设定的`Android`设备字体缩放比例，默认为`1`。也就是说，当用户没有改变`Android`设备的字体缩放比例时，`sp`、`dp`与`px`的换算是相同的。

## 多分辨率之殇

市面上存在着的各种不同分辨率的`Android`设备为广大`Android`开发者挖了众多的坑，比如：

需要为不同分辨率的`Android`设备单独维护一套`dimens`文件；

通常UI设计师只会针对某种特定分辨率的设备为我们标注`UI`控件的像素大小，相信不少小伙伴都受够了手动换算不同分辨率设备上`UI`控件像素大小的痛苦；

通常我们需要为每种分辨率的`Android`设备维护一个`drawable`文件夹以获得比较好的图片显示效果，这会导致`apk`文件尺寸的臃肿；而且若某个`drawable`文件夹下的图片需要修改，那么就需要替换其他所有`drawable`文件夹中对应的图片。如果不小心漏掉了某个`drawable`文件夹下的图片，则会导致该图片在某些分辨率的手机上失真。

