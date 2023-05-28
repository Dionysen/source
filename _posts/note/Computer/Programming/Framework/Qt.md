---
title: Qt
date: 2023-05-25 17:34:01
categories: [Programming, Framework]
tags: [CPP]
comment: true
sticky: 999
---

Qt 经常被当做一个 GUI 库，用来开发图形界面应用程序，但这并不是 Qt 的全部；Qt 除了可以绘制漂亮的界面（包括控件、布局、交互），还包含很多其它功能，比如多线程、访问数据库、图像处理、音频视频处理、网络通信、文件操作等。

<!-- more -->

## 信号与槽

连接一个信号和槽函数，它们分别来自不同（也可以是相同）的对象，也即`connect`是对象之间快速通信的桥梁。

```cpp
#include <QtGui/QApplication> 
#include <QtGui/QPushButton> 

int main(int argc, char *argv[]) 
{ 
        QApplication a(argc, argv); 
        QPushButton *button = new QPushButton("Quit"); 
        QObject::connect(button, SIGNAL(clicked()), &a, SLOT(quit())); 
        button->show(); 
        return a.exec(); 
}
```

```cpp
QObject::connect(button, SIGNAL(clicked()), &a, SLOT(quit())); //连接button对象的clicked事件函数与程序a的quit函数 
```



## 绘画

通过绘画事件函数`void paintEvent(QPaintEvetn *event)`画图，可以通过设置状态来控制画什么。

`update()`函数用来更新绘画事件函数。

普通绘画：

```cpp
paint.setPen(QPen(QColor(Qt::black), 1)); // 画轮廓
paint.setBrush(Qt::black); //填充
paint.drawEllipse(100,100,300,300); // 参数为位置x,y，圆所占矩形的长和宽
```

绘制透明填充：

```cpp
QColor color = Qt::white;
color.setAlphaF(0.5); // 透明度设置为0.5
paint.setPen(QPen(color));
paint.setBrush(color);
paint.drawEllipse(100,100,300,300);
```

## 事件

可以通过事件来改变组件的默认操作

`QApplication`的`exec()`就是事件循环，用来监听所有事件；当事件发生时，Qt将创建一个事件对象，然后传递给`event()`。

事件函数需要在子类中重写。重写事件函数其实就是在告诉系统，当遇到这个事件的时候应该做什么。

`paintEvent()`在程序开始的时候会`update()`一次，后面绘制图形需要再调用`update()`。



#### 鼠标事件

鼠标悬浮事件可以使用`QWidget`的参数`setMouseTracking`完成，但必须在所有上层控件中都设置`setMouseTracking(true)`才可以实现；比如`QMainWindow`类的监控必须同时在`MainWindow`和其父类`QWidget`中都设置`setMouseTracking(true)`。

然后重写`mouseMoveEvent`函数。使用`event->HoverMove`判断事件类型。



### 消息对话框

#### 可选按键的消息对话框

可直接使用静态函数实现有可选按钮的消息对话框：

```cpp
QMessageBox mb;
    mb.setWindowTitle(tr("Black is Win!"));
    mb.setText(tr("Black is win!"));
    mb.setInformativeText(tr("Are you want to play again?"));
    mb.setStandardButtons(QMessageBox::Yes | QMessageBox::No | QMessageBox::Default);
    mb.setDefaultButton(QMessageBox::Yes);
    switch (mb.exec()) {
    case QMessageBox::Yes:
    	newGame();
    	break;
    case QMessageBox::No:
    	exit();
    	break;
    default:
    	break;
}
```

or

```cpp
QMessageBox::StandardButton defaultBtn = QMessageBox::NoButton;
QMessageBox::StandardButton result; // 返回选择的按钮
result = QMessageBox::question(
    this, "Black is Win!", "Game over! Are you want to play again?",
QMessageBox::Yes | QMessageBox::No, defaultBtn);
if (result == QMessageBox::Yes)
     ui->plainTextEdit->appendPlainText("Question消息框: Yes 被选择");
else if (result == QMessageBox::No)
     ui->plainTextEdit->appendPlainText("Question消息框: No 被选择");
else
     ui->plainTextEdit->appendPlainText("Question消息框: 无选择");
```

or

```c++
 QMessageBox::information(
    this, "Black is Win!", "Game over! Are you want to play again?");
```



### 控件设置

```cpp
QLabel *l = new QLabel(this);// 创建label
QFont font;
font.setFamily("Times New Roman");
font.setPixelSize(20);
l->setFont(font); // 设置字体 
QString s = QString::number(whiteTime);
l->setText(s + "s"); // 设置显示的内容
QPalette palette;
palette.setColor(QPalette::Window, QColor(0, 0, 0));
palette.setColor(QPalette::WindowText, Qt::white);
l->setPalette(palette); // 设置样式
l->show(); // 显示label
```

使用`l->update()`可以更新控件显示的内容。

#### QPalette

```cpp
QPalette::Window // 是指背景色，
QPalette::WindowText// 指的是前景色等。
QPalette::setColor()//函数对某个主题的颜色及状态进行设置。
QPalette::setBrush()//函数对显示进行更改，这样就有可能使用图片而不仅仅是单一的颜色来对主题进行填充了。
QPalette::setColor(ColorRole r,const Qcolor &c);//对某个主题颜色进行设置，并不区分状态
QPalette::setColor(ColorGroup gr,ColorRole r,const QColor &c);//对主题颜色进行设置的同时还区分了状态。
xxx->setAutoFillBackground(true);
//可以提取某个控件的调色板
Qpalette p=xxx->palette();
p.setColor(QPalette::Window,color);//p.setBrush(QPalette::Window,brush);
xxx->setPalette(p);
```

#### 定时器

```cpp
QTimer *t = new QTimer(this);
t->start(1000); //设置事件间隔为1000ms,这样每过一秒就会调用timeout()函数
connect(t, SIGNAL(timeout()), this, SLOT(time())); // 将timeout()函数作为信号与自定义槽函数time()连接起来，就可以实现一些功能
```

#### 字体

```cpp
xxx->setFont(QFont("宋体",20,QFont::Bold)); // 使用构造函数可以轻松改变字体的样式，这也同样应用于其他类型

QFont font;
//设置文字字体
font.setFamily("宋体");
//设置文字大小为50像素
font.setPixelSize(50);
//设置文字为粗体
font.setBold(true); //封装的setWeight函数
//设置文字为斜体
font.setItalic(true); //封装的setStyle函数
//设置文字大小
font.setPointSize(20);
//设置文字倾斜
font.setStyle(QFont::StyleItalic);
//设置文字粗细//enum Weight 存在5个值
font.setWeight(QFont::Light);
//设置文字上划线
font.setOverline(true);
//设置文字下划线
font.setUnderline(true);
//设置文字中划线
font.setStrikeOut(true);

//设置字间距
font.setLetterSpacing(QFont::PercentageSpacing,300);//300%,100为默认
//设置字间距像素值
font.setLetterSpacing(QFont::AbsoluteSpacing,20);//设置字间距为100像素
//设置首个字母大写（跟参数有关，也可以设置全部大写AllUppercase）
font.setCapitalization(QFont::Capitalize);


//通过QFontMetrics获取字体的值
QFontMetrics fm(font);
qDebug() << fm.height(); //获取文字高度
qDebug() << fm.maxWidth();//获取文字宽度

//通过QFontInfo获取也能获取字体信息

QFontInfo fInfo(font);
qDebug() << fInfo.family() <<"  "<<fInfo.style() << fInfo.pixelSize() << fInfo.overline();

//设可以单独置QPlainTextEdit字体
//ui->plainTextEdit->setFont(font);

//将当前设置的字体设置为默认字体
qApp->setFont(font);
```

#### QLabel显示富文本

```cpp
QString lineHeightStr = "<p style='line-height:%1px'>%2</p>";
QString fontColorStr = "<font color = #959595>%1</font><font color = #000000>%2</font>";
QString textStr = lineHeightStr.arg(scaleConver(30)).arg(fontColorStr.arg("您于14天内到达或途经：","浙江省杭州市，河南省许昌市，广州省广州市，辽宁省大连市"));
ui->label_city->setText(textStr);

label->setText(
    QObject::tr("<font color = red>%1</font>").arg("abc"))+
    QObject::tr("<font color = blue>%1</font>").arg("efg")+
    "hij"
    );

//或者这样：
QSize nSize(300,25);
m_pStatic = new QLabel((QWidget*)GetUIWnd());
m_pStatic->resize(nSize);
QString strText = QString::fromStdWString(_CS(L"<font style = 'font-size:14px; font-weight:bold'>You Can See it from this: </font> <font style = 'color:#2C5DFF; font-size:14px; font-weight:bold'> %1 </font> <font style = 'font-size:14px; font-weight:bold'>example.</font>")).arg(0);
m_pStatic->setText(strText);
```

