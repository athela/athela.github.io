---
layout: post
title: "Qt 使用中的几个问题"
description: Qt linux
category: "Qt"
tags: [qt linux]
---
{% include JB/setup %}

本文要讲三个问题：

1. *QFileDialog去掉默认按钮*

2. *redhat6 QMainWindow的所有menu的action不显示图标*

3. *linux下QComboBox弹出来的长度占满电脑屏幕高度*

这三个问题都是Qt Linux与windows显示风格不同的问题，虽然解决办法都很简单，但网上解答并不多，不知道之前答案并不好找，写下来，希望有用。

###QFileDialog去掉默认的Open/Save、Cancel按钮
有人采用创建一个QWidget来遮住Open、Cancel按钮的办法，我一直不大赞成这种刚好达到目的而没有正确保障的做法，这种做法在windows下可以，在Linux下无论如何遮不完全，有Open、Cancel的部分按钮露了出来。

(help1)[*http://www.qtforum.org/article/20841/how-to-add-a-qwidget-in-qfiledialog.html#post78422*]

(help2)[*http://stackoverflow.com/questions/16987916/add-widgets-to-qfiledialog*]

(help3)[*http://www.qtcentre.org/threads/42858-Creating-a-Custom-FileOpen-Dialog*]

参考了上面这三个有用的链接后，才知道QFileDialog这种固件，还是可以被拆的，真是拆除了脑洞限制呀。举一反三，以后对Qt中的其它类也不会客气啦。

	QDialogButtonBox* box = _filedialog->findChild<QDialogButtonBox *>();
	if(box)
	{
		box->clear();
	}

###QMainWindow的所有menu的action项的图标未显示
windows和redhat5下可以显示，唯有redhat6下不显示，帮助文档上有时会交待哪些版本之间不一致，但这个问题官方文档也没有说，有些问题注定可以解决却没有原始说明。

	action->setIconVisibleMenu(true);

网上发现很多人有问这个问题，但是却几乎没什么回答，还被传得难到无解的程度，在这儿替最先回答这个问题的人传播一下，毕竟我找了好久。



###QComboBox在Linux下显示时占满屏幕高度


![loading...](/images/qcombox1.JPG)

设置 `comboBox->setStyleSheet("QComboBox {combobox-popup:0;}")` 之后恢复正常如图（windows下不用设也正常）：

![loading...](/images/qcombox2.JPG)



