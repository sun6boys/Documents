TableView性能优化

1.预设高度  会抖动
2.reloadSections cell不会重用
3.configwithData  外部是mutable 内部用copy
4.imageView 的url 记录一下 判断是否变了，变了再刷新图片
5.attbuiteString 外面创建好了传进来
6.html的东西 用第三方，系统的会卡主线程
7.