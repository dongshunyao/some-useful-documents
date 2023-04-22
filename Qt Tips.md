# Qt Tips

*2019.4.10*

**感谢郑老师和周巨佬的大力滋兹，Orz%%%**

- [Qt Tips](#qt-tips)
	- [字符](#字符)
	- [槽函数](#槽函数)
	- [非阻塞式延迟](#非阻塞式延迟)
	- [GUI](#gui)
	- [文件资源管理器](#文件资源管理器)

---

## 字符

```cpp
// 在头文件中加入以防止乱码
#ifdef WIN32 
#pragma execution_character_set("utf-8")
#endif

// 编码转换
QString s;
const auto code = QTextCodec::codecForName("GB2312");
string ss = code->fromUnicode(s).data();

// 将数字转换为字符串
QString::number(number);
```

---

## 槽函数

```cpp
// 槽函数自动连接控件
private slots:
    void on_button_clicked();
// button为控件名字，clicked为单击事件，自动连接

connect(Sender, SIGNAL(signal), Receiver, SLOT(slot));
// 使用宏：Sender为发送信号的控件名，Receiver为接收信号的对象名，signal为信号，slot为执行的槽函数

connect(test_button, &QPushButton::clicked, this, &test_window::test_slot);
// 使用指针：test_button为发送信号的控件名，this为接收信号的对象名，&QPushButton::clicked为信号的指针，这里是按钮单击，&test_window::test_slot为执行的槽函数的指针

// 上面的方法槽函数里不能有参数，有参数可以用lambda表达式实现
connect(test_button, &QPushButton::clicked, this, [=] { test_slot(); });
```

---

## 非阻塞式延迟

```cpp
void delay(const int seconds)
{
    const auto reach_time = QTime::currentTime().addSecs(seconds);
    while (QTime::currentTime() < reach_time) QCoreApplication::processEvents(QEventLoop::AllEvents, 100);
}
```

---

## GUI

```cpp
// 样式表指定范围
类名#对象名
{
    // 指定对象，指定该对象实例，对于所有符合条件的设置
    // 类名和对象名不写则通配
}

// 固定大小
setMinimumSize(100, 100);
setMaximumSize(100, 100);

// 设置透明
setFlat(true);
// 样式表设置透明
background-color: rgba(0, 0, 0, 0);

// 样式表设置文字颜色
color:rgb(255, 255, 255);

// QMainiWindow设置背景
#centralWidget{border-image:url(:/image/Resources/background.jpg);}

// 设置窗口关闭自动释放对象
setAttribute(Qt::WA_DeleteOnClose, true);

// 设置窗口为模态
setWindowModality(Qt::ApplicationModal);
```

---
## 文件资源管理器

```cpp
// 打开并选中文件
void open_file_browser(QString url)
{
    url.replace("/", "\\");
    QProcess::startDetached(QString("explorer.exe /select,\"%1\"").arg(url));
}

// 打开目录
void open_file_browser(QString url)
{
    url.replace("/", "\\");
    QProcess::startDetached(QString("explorer.exe \"%1\"").arg(url));
}

// 取项目路径
QDir::currentPath();
```
