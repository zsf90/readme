# QSettings

`QSettings` 类提供了持久化保存用户配置参数的功能。如：软件重启后从配置文件中读取参数。



## QSettings 对象的创建

`QSettings` 对象即可以创建在栈上，也可以创建在堆上，构建和销毁也非常快。`QSettings` 类的构造函数的重载允许我们用各种方法来初始化`QSettings`对象，比如可以在当创建`QSettings`对象时，通过指定公司或组织名称以及产品名称，也可以通过指定文件路径和`Format`类型，这里我们介绍使用指定文件路径和`Format`类型的方式构造`QSettings`对象。

## 在栈上创建对象

```c++
QSting filename = QDir::homePath() + "/.config/eightplus/qsettings-demo.ini";
QSettings m_settings(filename, QSettings::IniFormat);
m_settings.setIniCodec("UTF-8");
```

## 在堆上创建对象

```c++
QString filename = QDir::homePath() + "/.config/eightplus/qsettings-demo.ini";
QSettings *m_settings = new QSettings(filename, QSettings::IniFormat);
m_settings->setIniCodec("UTF-8");
```

## Format 枚举类型

```c++
enum Format {
    NativeFormat,
    IniFormat,
    InvalidFormat=16,
    CustomFormat1,
    CustomFormat2,
    CustomFormat3,
    CustomFormat4,
    CustomFormat5,
    CustomFormat6,
    CustomFormat7,
    CustomFormat8,
    CustomFormat9,
    CustomFormat10,
    CustomFormat11,
    CustomFormat12,
    CustomFormat13,
    CustomFormat14,
    CustomFormat15,
    CustomFormat16,
}
```

## QSettings 存储参数

`QSettings` 使用`setValue()`函数存储一系列设置，每个设置包括`key`（字符串）和一个与该`key`关联的`value`（`QVariant`），如：

```c++
m_settings->beginGroup("General");
m_settings->setValue("name", m_name);
m_settings->setValue("male", m_male);
m_settings->setValue("age", m_age);
m_settings->endGroup();
m_settings->sync();
```

`sync()`:如果存在相同的`key`，现有的值将被新值覆盖。为了提高效率，这些变化可能不会被立即保存到永久存储（可以随时调用`sync()`来提交更改）。

## QSettings 读取

`QSettings` 使用`value()`函数得到一个`key`的`value`，如：

```c++
m_settings->beginGroup("General");
m_name = m_settings->value("name", m_name).toString();
if(m_name.isEmpty()){
    m_name = "lixiang";
}
m_male = m_settings->value("male", m_male).toBool();
m_age = m_settings->value("age", m_age).toInt();
m_settings->endGroup();
```

## QVariant类型

`QSettings` 的`value`为`QVariant`类型，`QVariant`是一种数据类型的集合，是`Qt Core`模块的一部分，它支持大部分通常的`Qt`数据类型转换，如：`toInt()`、`toString()`、`toBool()`、`toPoint()`、`toSize()`等。但它不能提供`Qt GUI`那一部分的`Qt`数据类型转换，如：`QColor`、`QImage`、`QPixmap`，即`QVariant`中没有`toColor()`、`toImage()`、`toPixmap()`等接口，此时可以使用`QVariant::value()`或`qVariantValue()`模板函数，如下所示：

```c++
QColor color = m_settings->value("color").value<QColor>();
```

既然上面说到`QVariant`中没有`toColor()`等接口，那如果是`QColor`等类型，是否可以直接用`setValue()`去存储呢？答案是可以的，包括`Qt Core`和`Qt GUI`在类的相关类型，`QVariant`都支持，如下所示：

```c++
QColor color = palette().background().color();
m_settings->setValue("color", color);
```

另外，使用 `qRegisterMetaType()`和`qRegisterMetaTypeStreamOperators()`注册的自定义类型，也可以使用`QSettings`进行存储。

## ini 配置文件示范

```ini
[%General]
age=18
male=true
name=lixiang
```

