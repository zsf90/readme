# QComboBox 控件的使用

## Q_SIGNALS

```c++
void editTextChanged(const QString &);
void activated(int index);
void currentIndexChanged(int index);
void currentTextChanged(const QString &text);
void highlighted(int index);
void textActivated(const QString &text);
void textHighlighted(const QString &text);
```

## Properties

| 属性                  | 类型             |
| --------------------- | ---------------- |
| count                 | const int        |
| currentData           | const QVariant   |
| currentIndex          | int              |
| currentText           | QString          |
| duplicatesEnabled     | bool             |
| editable              | bool             |
| frame                 | bool             |
| iconSize              | QSize            |
| insertPolicy          | InsertPolicy     |
| maxCount              | int              |
| maxVisibleItems       | int              |
| minimumContentsLength | int              |
| modelColumn           | int              |
| placeholderText       | QString          |
| sizeAdjustPolicy      | SizeAdjustPolicy |

