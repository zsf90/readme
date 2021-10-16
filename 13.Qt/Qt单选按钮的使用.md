# QRadioButton 单选按钮

`QRadioButton` 的父类是 `QAbstractButton`。具体文档可在 `Qt Creator` 中的帮助里边进行搜索查阅。

## 给单选按钮分组

`QRadioButton`的分组有多重方法，如采用`QButtonGroup`、`QGroupBox`、`QWidge`等。推荐使用`QButtonGroup`。



## QButtonGroup

要使用 `RadioButton` 单选按钮，就要给多个单选按钮进行分组，组中的单选按钮只有一个可以被选中。实现方式：我们可以使用 `Qt Creator` 以图形界面的方式使用按钮组`QButtonGroup`，也可以使用`代码`的方式使用。

这里使用了`QButtonGroup`的`buttonToggled`信号，该信号有2个参数，`button`代表哪个按钮，`checked`代表按钮是否选中。

```c++
/* m_colorBtnGroup */
connect(ui->m_colorBtnGroup, QOverload<QAbstractButton *, bool>::of(&QButtonGroup::buttonToggled), [=](QAbstractButton *button, bool checked){
    /**
         * button 选中
         */
    if(button->isChecked()){
        if(button == ui->m_redRadioButton){
            ui->m_colorGroupBox->setTitle("当前选择的颜色为：红色");
        }

        if(button == ui->m_greenRadioButton){
            ui->m_colorGroupBox->setTitle("当前选择的颜色为：绿色");
        }

        if(button == ui->m_blueRadioButton){
            ui->m_colorGroupBox->setTitle("当前选择的颜色为：蓝色");
        }
    } else { /* button 非选中 */

    }
}); /* m_colorBtnGroup END! */
```

