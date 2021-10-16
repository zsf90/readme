## 获取串口

获取所有串口号存储在 vector<QString> serial_number_list 中。

## 在定时器中间隔获取串口号

部分头文件代码

```c++
{
    private:
    Ui::Dialog *ui;
    QSerialPort *serial1;
    QTimer *m_timerGetSerialNumber;
}
```



```c++
void Dialog::getSerialNumber()
{
    m_timerGetSerialNumber = new QTimer(this); /* 实例化一个定时器 */
    m_timerGetSerialNumber->stop();
    m_timerGetSerialNumber->setInterval(1000);	/* 设置间隔 毫秒 */

    foreach (const QSerialPortInfo &info, QSerialPortInfo::availablePorts())
    {
        mSerialNameInfoList.append(info.portName());
        _serial_number.push_back(info.portName());
    }
    m_timerGetSerialNumber->start(); /* 开始定时器 */
}
```

槽函数

```c++
/**
 * @brief Dialog::m_slotTimerGetSerialNumber
 * @brief 在定时器中判断当前串口是否存在
 */
void Dialog::m_slotTimerGetSerialNumber()
{
    QStringList sltemp; /* 临时字符串列表 */
    foreach (const QSerialPortInfo &info, QSerialPortInfo::availablePorts())
    {
        sltemp.append(info.portName());
    }
//    m_settingFileObj->beginGroup("serial");
//    if(!sltemp.contains(m_settingFileObj->value("serial_number").toString())){
//        qDebug() << "串口不存在";

//    } else {
//        qDebug() << "串口存在";
//    }

//    m_settingFileObj->endGroup();
    std::vector<QString>::iterator it;
    for(it=_serial_number.begin(); it!= _serial_number.end(); it++)
    {

    }
    sltemp.clear();
}
```

判断 _serial_number 列表中的每个元素是否在 sltemp 列表中，如果不在则从交换两个列表的值。

判断下拉列表当前值是否不在 _serial_number，如果不在则让下拉列表显示一个存在的串口号。