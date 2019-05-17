---
title: Qt之QTimer
date: 2019-05-12 16:18:06
tags:
---

- [第10篇 Qt5基础（十）Qt定时器和随机数](http://www.qter.org/portal.php?mod=view&aid=37)
- [QT之 QTimer使用方法](https://blog.csdn.net/zz2862625432/article/details/79550285)
- [Qt 之 QProgressBar](https://blog.csdn.net/liang19890820/article/details/52302879)

效果图如下，定时器设置为100ms触发，进度条也随之滚进。

![](Qt之QTimer/demo_QTimer.gif)

**MainWindow.h**

```C++
#pragma once

#include <QtWidgets/QMainWindow>
#include "ui_MainWindow.h"
#include <QTimer>
#include <QProgressBar>

class MainWindow : public QMainWindow
{
    Q_OBJECT

public:
    MainWindow(QWidget *parent = Q_NULLPTR);

private:
    Ui::MainWindowClass ui;
    QTimer *timer_;
    QProgressBar *progress_;
private slots:
    void UpdateProgress();
};
```


**MainWindow.cpp**

```C++
#include "MainWindow.h"
#include <QPushButton>
#include <QLayout>

MainWindow::MainWindow(QWidget *parent)
    : QMainWindow(parent) {
    ui.setupUi(this);
    timer_ = new QTimer(this);
    timer_->setInterval(100);

    progress_ = new QProgressBar(this);
    progress_->setRange(0, 100);
    progress_->setValue(50);

    QPushButton *okBtn = new QPushButton(this);
    okBtn->setText(tr("ok"));

    QPushButton *cancelBtn = new QPushButton(this);
    cancelBtn->setText(tr("cancel"));

    QHBoxLayout *btnLayout = new QHBoxLayout();
    btnLayout->addWidget(okBtn);
    btnLayout->addWidget(cancelBtn);
    QVBoxLayout *mainLayout = new QVBoxLayout();
    mainLayout->addWidget(progress_);
    mainLayout->addLayout(btnLayout);

    QWidget *widget = new QWidget(this);
    //setCentralWidget(widget);
    //centralWidget()->setLayout(mainLayout);

    widget->setLayout(mainLayout);
    setCentralWidget(widget);

    connect(okBtn, &QPushButton::clicked, timer_, static_cast<void (QTimer::*)()> (&QTimer::start));
    connect(cancelBtn, &QPushButton::clicked, timer_, &QTimer::stop);
    connect(timer_, &QTimer::timeout, this, &MainWindow::UpdateProgress);
}


void MainWindow::UpdateProgress() {
    int curVal = progress_->value();
    ++curVal;
    if (curVal >= 100) {
        curVal = 0;
    }
    progress_->setValue(curVal);
}
```

**main.cpp**

```C++
#include "MainWindow.h"
#include <QtWidgets/QApplication>

int main(int argc, char *argv[])
{
    QApplication a(argc, argv);
    MainWindow w;
    w.show();
    return a.exec();
}
```

