包含串口头文件：

```c++
#include <QSerialPort>
#include <QSerialPortInfo>
```

## QSerialPort串口类

QSerialPort 类提供了创建一个串口实例并设置该实例各种参数的方法。

### Public Types

| 类型列表                                                     |
| ------------------------------------------------------------ |
| enum `BaudRate` { ==Baud1200==, ==Baud2400==, ==Baud4800==, ==Baud9600==, ==Baud19200==, …, ==UnknownBaud== } |
| enum `DataBits` { ==Data5==, ==Data6==, ==Data7==, ==Data8==, ==UnknownDataBits== } |
| enum `Direction` { ==Input==, ==Output==, ==AllDirections== } |
| flags `Directions`                                           |
| enum `FlowControl` { ==NoFlowControl==, ==HardwareControl==, ==SoftwareControl==, ==UnknownFlowControl==} |
| enum `Parity` { ==NoParity==, ==EvenParity==, ==OddParity==, ==SpaceParity==, ==MarkParity==, ==UnknownParity== } |
| enum `PinoutSignal` { ==NoSignal==, ==TransmittedDataSignal==, ==ReceivedDataSignal==, ==DataTerminalReadySignal==, ==DataCarrierDetectSignal==, …, ==SecondaryReceivedDataSignal== } |
| flags `PinoutSignals`                                        |
| enum `SerialPortError` { ==NoError==, ==DeviceNotFoundError==, ==PermissionError==, ==OpenError==, ==NotOpenError==, …, ==UnknownError== } |
| enum `StopBits` { ==OneStop==, ==OneAndHalfStop==, ==TwoStop==, ==UnknownStopBits== } |



### 属性列表

1. `baudRate`:qin32
2. `breakEnabled`:bool
3. `dataBits`:DataBits
4. `dataTerminalReady`:bool
5. `error`:SerialPortError
6. `flowControl`:FlowControl
7. `parity`:Parity
8. `requestToSend`:bool
9. `stopBits`:StopBits

### 构造函数

```c++
QSerialPort(const QSerialPortInfo &serialPortInfo, QObject *parent = nullptr);
QSerialPort(const QString &name, QObject *parent = nullptr);
QSerialPort(QObject *parent = nullptr);
```

### SET 方法

```c++
bool setBaudRate(qint32 baudRate, QSerialPort::Directions directions = AllDirections) // 设置波特率
bool setBreakEnabled(bool set = true) //
bool setDataBits(QSerialPort::DataBits dataBits) // 设置数据位
bool setDataTerminalReady(bool set) //设置数据发送
bool setFlowControl(QSerialPort::FlowControl flowControl) // 设置流控制
bool setParity(QSerialPort::Parity parity) // 设置校验位
void setPort(const QSerialPortInfo &serialPortInfo) // 设置端口
void setPortName(const QString &name) // 设置端口名
void setReadBufferSize(qint64 size) // 设置读数据缓存大小
bool setRequestToSend(bool set)
bool setStopBits(QSerialPort::StopBits stopBits) // 设置停止位
```



## QSerialPortInfo 类

QSerialPortInfo 类提供了获取所有可用串口设备信息的功能。

## 连接一个串口设备

首先用一个列表保存所有可有串口设备的信息（QSerialPortInfo），然后创建一个串口实例，创建实例时会用到保存串口信息的列表，从列表中选择一个要连接的串口。



