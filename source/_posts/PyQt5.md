---
title: PyQt5
date: 2021-12-17 01:13:16
tags: Python Qt PyQt5 GUI
---

## [PyQt5 tutorial](https://zetcode.com/gui/pyqt5/) ZetCode 笔记

1. Date and time

	1. Current date and time
		a. Qt.ISOData
		b. DefaultLocaleLongDate
	2. UTC time
	3. Number of days
	4. Difference in days
	5. Date arithmetic
	6. Daylight saving time
	7. Unix epoch
	8. Julian day
	便于时间相减，直接计算天数
	
2. Hello, world
	1. Simple example
	2. Icon
	3. Tooltip
		a. Btn.sizeHint()
	4. closeEvent
		qbtn.clicked.connect(QApplication.instance().quit)
	5. Message box
	6. Center

3. Menus and toolbar
	1. Statusbar
	2. Menu
	3. Sub menu
	4. Check menu
	5. Context menu
	6. Toolbar
	7. Main window
	
4. Layout
	1. QHBoxlayout
	2. QGridLayout
	
5. Event and signal
	1. LED demo
	2. Key event handler
	3. Event object, mouse event
	4. Push btn event sender
	5. Emitting signal

6. Dialog
	1. QDialog
	2. QcolorDialog and QFrame
	3. QFontDialog, btn.setSizePolicy
	4. QFileDialog, Qaction.triggered, QMainWindow

7. Qwidge1
	1. QCheckBox
	2. Toggle button, color demo, Qcolor.setRed
	3. Qslider
	4. QProgressBar
	5. QCalendarWidge


8. QWidget2
	1. Qpixmap, Qlabel.setPixmap
	2. QLineEdit
	3. Qsplitter
	4. QComBox 复选框

9. Drag and Drop 稍微还是有些不理解
	1. Qdrag
	2. QMineData
	3. dragEnterEvent仅获取对应的类型
	4. dropEvent设置drop时的动作
	
10. QPainter
	1. Draw text
	2. Draw point
	3. Draw color
	4. drawRect, setBrush
	5. Qpen(line, curves, rectangle)
	6. Qbrush
BezierCurve

## [Python and PyQt: Building a GUI Desktop Calculator](https://realpython.com/python-pyqt-gui-calculator/) 笔记

MVC典型流程，先画View，再设计控制逻辑Control，最后再设计计算模型Model，分步骤实现。

## [Qt Designer and Python: Build Your GUI Applications Faster](https://realpython.com/qt-designer-python/#getting-started-with-qt-designer) 笔记
coding一个相当完整的mainwindow

## 进一步熟悉

书籍《PyQt快速开发与实践》中介绍了一些基本内容，后续可以进一步掌握，包括设计师的使用，基础组件，布局管理，高级组件等，还是要结合实战进行更深入的学习和使用才行。

## 1. 参考链接

1. [The complete PyQt5 tutorial — Create GUI applications with Pyth on](https://www.pythonguis.com/pyqt5-tutorial/)
2. [PyQt5 Reference Guide](https://www.riverbankcomputing.com/static/Docs/PyQt5/) 官方文档
3. [PyQt5 tutorial](https://zetcode.com/gui/pyqt5/) ZetCode
4. [PyQt-Examples](https://github.com/janbodnar/PyQt-Examples) Github contains more in-depth examples for different topics
5. [PyQt5-Tutorial-Examples](https://github.com/janbodnar/PyQt5-Tutorial-Examples) Sources and images for ZetCode's PyQt5 tutorial
6. [PyQt5图形界面编程（目录）](https://zhuanlan.zhihu.com/p/48373518) 知乎
7. [Python and PyQt: Building a GUI Desktop Calculator](https://realpython.com/python-pyqt-gui-calculator/) 入门制作一个计算器，手把手指导，很不错
   1. 相关的repo [materials](https://github.com/realpython/materials)
8. [Qt Designer Manual](https://doc.qt.io/qt-5/qtdesigner-manual.html)
9.  [Qt Designer and Python: Build Your GUI Applications Faster](https://realpython.com/qt-designer-python/#getting-started-with-qt-designer) QtDesigner的简单使用
10. [[ PyQt入门教程 ] Qt Designer工具的使用](https://www.cnblogs.com/linyfeng/p/11223707.html) 使用qtdesigner实现登录框和业务逻辑代码