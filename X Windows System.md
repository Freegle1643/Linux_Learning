## X Windows System

##### 概要

X Windows System（X11，，或者简称为 X）是一种用于显示位图的视窗系统，常见于UNIX-like的电脑操作系统中。X提供了基本的GUI运行环境框架，例如窗口在显示器上的绘制和移动，以及和外设的交互等。

X是一种架构无关的系统，可用于远程的用户图形和输入设备交互。值得注意的是，X并不要求一个用户界面，而是每个单独的用户程序在管理这些。独立的程序会可以在没有用户界面的情况下调度X的图形能力，所以，不同程序的可视化样式会非常不同，这就带来了不同的程序可能呈现出非常不同的界面。

不像很多早期的显示协议，X专门就是为了网络连接的显示而非本地集成的显示设备而创造的。X有网络透明性的特点（Network transparency），也就意味着X程序可以在某处的电脑上运行，而其图像可以在另一个有**X server运行的联网电脑上运行**，若该电脑还提供了显示设备和输入输出外设的话，就可以实现一个远程化的交互。

需要注意的是，在这样一套协议里，用户接触的其实是X的server端而远程跑程序的确是客户端。因为用户的图形显示界面和输入输出设备的图像和交互正是由本地的X server才能为本地和远程的X客户程序所使用。

X的网络协议是继续X的命令原语的，它支持2D和3D（通过例如像[GLX](https://www.wikiwand.com/en/GLX)（Open**GL** Extension to the **X** Window System）），并借此能让X客户程序来加速X server的显示。