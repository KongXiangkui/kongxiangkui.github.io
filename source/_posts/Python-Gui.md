---
title: Python Gui
date: 2020-02-28 22:58:57
tags: Python
---
## Tkinter

Python默认GUI库

> 使用回调(callback)机制传递消息

## PyQt(PySide)

PyQt a module to make desktop software with Python, a port of the Qt library (C++).

modules:

- Qtcore contains the core non-GUI code.This module is used for working with time, files and directories, various data types, streams, URLs, mime types, threads or processes. It contains classes for windowing system integration, event handling, 2D graphics, basic imaging, fonts and text.
- QtGui has everything for window management like event handling and graphics.
- QtWidgets has a many UI widgets like buttons, labels, textinput and other things you’d see in a desktop window.
- QtMultimedia for multimedia content and camera.
- QtBluetooth scan bluetooth devices and connect.
- QtNetwork a cross-platform solution for network programming. Set up a socket server or client that works on all desktop systems. Supports both the TCP/IP stack and UDP.
- QtPositioning determine a position by using a position (WiFi, Satellite)
- QtWebSockets implementation of the websocket protocol.
- QtWebKit web browser implementation. You can use this to render a webpage. This is based on WebKit2. WebKit is used in the Safari browser, by KDE and others.
- QtWebKitWidgets Deprecated. WebKit1 version of web browser implementation.
- QtXml use XML files, reading/writing and so on.
- QtSvg svg graphics (Scalable Vector Graphics (SVG). A type of image format.
- QtSql work with databases.
- QtTest unit testing

使用信号/槽（signal/slot）进行通信（传递消息和事件）——灵活（程序越大越明显）
按钮点击后，发出clicked信号，该信号与窗口的close函数相关联，窗口收到clicked信号后，执行close函数，关闭窗口

符合MVC（模型-视图-控制器）设计模式，显示与任务分离

GUI Application

- a main window
- several dialogs
  - Modal:This dialog is one that blocks the user from interacting with other parts of the application. The dialog is the only part of the application that the user can interact with. Until the dialog is closed, no other part of the application can be accessed.
  - Modeless: This dialog is the opposite of a modal dialog. When a modeless dialog is active, the user is free to interact with the dialog and with the rest of the application.
- a top-level widget
  - QDialog——application based on the Dialog template
  - QWidget——application based on the Widget template
  - QMainWindow——application based on the Main Window template
- the rest widgets(top-level widget's children)

### Qt Designer设计流程

- start Designer
- choose template
- resize the form and drag amd drop widgets(design a graphical interface), set its properties.
- export design to UI(.ui文件)
- convert the ui code to a python file(pyuic5 uiname.ui -o uiname.py)
- create another file that load the ui file (import uiname)

类型
 
- QWidget is the base class for all GUI elements in the PyQt5
- QDialog is used for asking the user about something, like asking the user to accept or reject something or maybe asking for an input and is based on QWidget.
- QMainWindow is the bigger template where you can place your toolbar, menubar, status bar, and other widget and it doesn't have a built-in allowance for buttons like those in QDialog.

UI
- Loading .ui file: 
```python
from PyQt5 import QtWidgets, uic
import sys
app = QtWidgets.QApplication([])
w = uic.loadUi("mydesign.ui")    # specify the location of your .ui file
w.show()
sys.exit(app._exec())
```

- convert .ui to .py
```python
pyuic5 mydesign.ui -o mydesign.py
```
> 第二种方法不需要在运行时解析XML文件，速度更快，更安全

pyuic5生成的代码
The code is structured as a single class that is derived from the Python object type. The name of the class is the name of the toplevel object set in Designer with Ui_ prepended. (In the C++ version the class is defined in the Ui namespace.) We refer to this class as the form class.

The class contains a method called setupUi(). This takes a single argument which is the widget in which the user interface is created. The type of this argument (typically QDialog, QWidget or QMainWindow) is set in Designer. We refer to this type as the Qt base class.

**Writing Qt Designer Plugins**
Qt Designer can be extended by writing plugins. Normally this is done using C++ but PyQt5 also allows you to write plugins in Python.


### PyQt5 Resource System
A .qrc resource collection file is a XML file used to specify which resource files are to be embedded. The application then refers to the resource files by their original names but preceded by a colon.

pyrcc5 is PyQt5's equivalent tp Qt's rcc utility and is used in exactly the same way. pyrcc5 reads the .qrc file, and the resource files, and generates a Python module that only needs to be imported by the applicaiton in order for those resources to be made available just as if they were thee original files.

### Internationalisation of PyQt5 Application

PyQt5 and Qt include a comprehensive set of tools for tranlating applicaitons into local languages.

- The programmer use **pylupdate5** to create or update a .ts translation file for each language that the application is to be translated into. A .ts file is an XML file that contains the strings to be translated and the corresponding translations that have already been made.
- The translator uses Qt Linguist update the .ts files with tranlations of the strings.
- The release manager then use Qt's lrelease utility to convert the .ts files to .qm files which are compact binary equivalents used by the application. if an application cannot find an appropriate .qm file, or a particular string hasn't been translated, then the strings used in the original source code are used instead.
- The release manage may optionally use pyrcc5 to embed the .qm files, aloong with other applicaion resources such as icons, in a Python module.

### pyqtdeploy
A tool for deploying PyQt applications. It supports deployment to desktop platform(Linux, windows,macos) and mobile platform(ios,android)

### QMainWindow

- 菜单栏：菜单，一组命令的集合
- 状态栏：显示一些状态的信息，通常在应用的底部
- 工具栏：常用工具按钮

### QWidget

```python
self.ui.label.setFont(QtGui.QFont('SansSerif', 30))   # change font type and size
self.ui.label.setGeometry(QtCore.QRect(10, 10, 200, 200)) # change label geometry
self.ui.label.setText("Like Geeks") # change label text
```
#### QLineEdit widget
```python
self.ui.linEdit.setText("")
self.ui.lineEdit.setMaxLength(10)   # set maximum length
self.ui.lineEdit.setEchoMode(QtWidgets.QLineEdit.Password)  # password input
self.ui.lineEdit.setReadOnly(True)  # QLineEdit read-onnly

# insert the colort like web pages CSS values.
# the setStyleSheet() method can be used with all PyQt widgets to change the style.(Font type and size, Text color, Background color, Border color, Border top color, Border buottom color, Border right color, Border left color, Selection color, Selection background color)
self.ui.lineEdit.setStyleSheet("color: rgb(28, 43, 255);")  # change text color
self.ui.lineEdit.setStyleSheet("background-color: rgb(28,43,255)") # change QLineEdit background color
```

#### QPushButton Widget

#### QComboBox Widget

addItem() method to add items to the QComboBox

setCurrentIndex() method to select an item from QComboBox
setCurrentText("Second Item") method to select by text

#### QTableWidget
QTableWidget consists of cells, each cell is an instance of QTableWidgetItem class.

setRowCount(number1) method to add rows to the QTableWidget.
seColumnCount(number2) method to add columns to the QTableWidget.

```python
# a pushbutton to clear QTableWidget content.
def clear():
   self.ui.tableWidget.clear()
   self.ui.pushButton,clicked.connect(clear)

# populate QTableWidget by code
# create a Python list of thress tuples.
data = []
data.append(('Populating', 'QtableWidget'))
data.append(('With data', 'In Python'))
data.append(('Is easy', 'Job'))

# inside the constructor of the main window, we set the rows and columns count.
self.ui.tableWidget.setRowCount(3)
self.ui.tableWidget.setColumnCount(2)
self.ui.tableWidget.setHorizontalHeaderLabels(('Column 1', 'Column 2'))  # set column header text
self.ui.tableWidget.setVerticalHeaderLabels(('Row 1', 'Row 2', 'Row 3'))
# set row header text
# iterate over the list and get every tuple on the list to fill the table cells using setItem() method.
row=0
for tup in data:
   col=0
   for item in tup:
      cellinfo = QTableWidgetItem(item)
      cellinfo.setFlags(QtCore.Qt.ItemIsSelectable | QtCore.Qt.ItemIsEnabled) # make cell not editable
      self.ui.tableWidget.setItem(row, col, cellinfo)
      col+=1
   row += 1
self.ui.tableWidget.setSortingEnabled(True)  #  make your QTableWidget sortable
self.ui.tableWidget.sortByColumn(0, QtCore.Qt.AscendingOrder)  # sort by the first column
 self.ui.tableWidget.sortItems(0,QtCore.Qt.DescendingOrder)
```


```python
from PyQt5.QtWidgets import QTableWidgetItem
from mydesign import * 
import sys 
data = ['PyQt5','Is','Awesome']

class mywindow(QtWidgets.QMainWindow):
   def __init__(self):
   super().__init__()
   self.ui = Ui_MainWindow()
   self.ui.setupUi(self)
   self.ui.tableWidget.setRowCount(3)
   self.ui.tableWidget.setColumnCount(2)
   row=0
   for item in data:
      cellinfo=QTableWidgetItem(item)
      combo = QtWidgets.QComboBox()
      combo.addItem("First item")
      combo.addItem("Second item")
      self.ui.tableWidget.setItem(row, 0, cellinfo)
      self.ui.tableWidget.setCellWidget(row, 1, combo)
      row += 1

app = QtWidgets.QApplication([])
win = mywindow()
win.show()
sys.exit(app._exec())
```

#### QCheckbox

#### QProgressBar


### Sinal/Slot
use the connect() method to the emitted signalwith a slot.

the event handling process continues to work until you close your form or main widget.

signal/slot editor:
connect a signal to any predefined slot without coding in the PyQt5 designer.

**How to emit a signal**
- define your event with type pyqtSignal
- call emit() method at the place you want your event to be fired.
```python
from PyQt5.QtCore import pyqtsignal, QObject
class nut(QObject):
   cracked = pyqtSignal()
   def __init__(self):
      QObject.__inti__(self)
   def craack(self):
      self.cracked.emit()
```
**How to use a signal**
```python
def crackit():
   print("hazelnut cracked!")

hazelnut = nut()
hazelnut.cracked.connect(cracket)   # connecting the cracked signal with crackit slot
hazelnut.crack()
```

**Signal(event) overriding**
```python
# if you want to close the main window when the user presses a specific key, you can overrid the KeyPressEvent inside your main window like this.
def keyPressEvent(self,e):
   if e.key() == Qt.Key_F12:
      self.close()
```


#### pipinstaller
```shell
# Executable will be generated on a floder called dist on yout Python directory.
$ pyinstaller test.py

# generate one executable file using one file mode
$ pyinstaller --onefile test.py

# Each time you run your executable a console window appear.
# use -w or noconsole window:
$ pyinstaller -w test.py

$ pyinstaller --help

```
## wxPython

> Provide a wrapper for wxDigets, a cross-platform GUI library.
> Open source toolkit

## Kivy

> Open source Python library for rapid development of applications that make use of innovative user interfaces, such as multi-touch apps. 
> Provide a design language called Kv for GUI design

Python写逻辑，Kv写界面