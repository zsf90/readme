# 信号与槽函数的使用

这个例子主要是对不了解Qt中信号和槽的新手，通过这个例子可以自己实现定义槽函数、连接槽函数。

## 定义槽函数

首先在 .h 都文件中定义槽函数原型

```c++
#ifndef MAINWINDOW_H
#define MAINWINDOW_H

#include <QMainWindow>

QT_BEGIN_NAMESPACE
namespace Ui { class MainWindow; }
QT_END_NAMESPACE

class MainWindow : public QMainWindow
{
    Q_OBJECT

public:
    MainWindow(QWidget *parent = nullptr);
    ~MainWindow();

private:
    void initSS(void);

private:
    Ui::MainWindow *ui;

/* 在这里定义槽函数的原型，指定返回类型和参数类型 */
private slots:
    void slotSetting(void);
    void slotBtnDemo(void);
};
#endif // MAINWINDOW_H
```

## 在 .cpp 文件中实现槽函数

```c++
#include "mainwindow.h"
#include "ui_mainwindow.h"
#include "ui_settingdialog.h"

MainWindow::MainWindow(QWidget *parent)
    : QMainWindow(parent)
    , ui(new Ui::MainWindow)
{
    ui->setupUi(this);
    this->initSS();
}

MainWindow::~MainWindow()
{
    delete ui;
}

/* slot开头的函数都是槽函数 */
void MainWindow::slotSetting()
{
    qDebug() << "你点击了设置按钮" << Qt::endl;
}

void MainWindow::slotBtnDemo()
{
    qDebug() << "Hello Qt" << Qt::endl;
}

void MainWindow::initSS(void)
{
    connect(ui->action_serial_setting, SIGNAL(triggered()), this, SLOT(slotSetting()));
    connect(ui->pbtn_test, SIGNAL(clicked()), this, SLOT(slotBtnDemo()));
}

```

## 连接槽函数

连接槽函数用到了 connect()

```c++
void MainWindow::initSS(void)
{
    connect(ui->action_serial_setting, SIGNAL(triggered()), this, SLOT(slotSetting()));
    connect(ui->pbtn_test, SIGNAL(clicked()), this, SLOT(slotBtnDemo()));
}
```

几种方式：

```c++
void MainWindow::initSS(void)
{
    connect(ui->action_serial_setting, SIGNAL(triggered()), this, SLOT(slotSetting()));
    /* 老写法 */
//    connect(ui->pbtn_test, SIGNAL(clicked()), this, SLOT(slotBtnDemo()));
    /* 新写法 */
//    connect(ui->pbtn_test, &QPushButton::clicked, this, &MainWindow::slotBtnDemo);
    /* Lambda */
    connect(ui->pbtn_test, &QPushButton::clicked, this, []{ qDebug() << "Lambda 表达式"; });
}
```

