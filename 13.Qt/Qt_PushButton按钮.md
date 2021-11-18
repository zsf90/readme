## 信号槽

```cpp
#include "mainwindow.h"
#include "./ui_mainwindow.h"
#include <QDebug>

MainWindow::MainWindow(QWidget *parent)
    : QMainWindow(parent)
    , ui(new Ui::MainWindow)
{
    ui->setupUi(this);

    setWindowTitle("QT Demo"); /* 设置主窗口标题 */

    init_connect();
}

MainWindow::~MainWindow()
{
    delete ui;
}

void MainWindow::init_connect(void)
{
    /* C++ Lambda 方式槽函数 */
    connect(ui->pushButton_test, &QPushButton::clicked, this, [=]() {
        qDebug() << "Hello, QT!";
    });
}
```

