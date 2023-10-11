
# flutter

## 入门

* 中文社区电子书[地址](https://book.flutterchina.club/)
* [flutter pub社区包地址](https://pub-web.flutter-io.cn/)
* `flutter`安装,[官方说明](https://flutter.cn/docs/get-started/install/windows)
  * 下载`flutter`并解压
  * 添加`path`
  * 使用`flutter-doctor`进行环境判断
  * 添加编辑器`flutter`插件

## dart

### dart简介

* `dart`是`flutter`使用的开发语言,可以声明变量类型,也可以使用`var`使其可变,作为新出现的开发语言,与其他热门语言有相似之处,上手难度低
  * 最新版本已使用类似kotlin的空安全,使用`??`运算符(`a??'1'`等价于`a!=null?a:'1'`)和断言不为空运算符`!`
* 命名风格:[官方文档](https://dart.cn/guides/language/effective-dart/style)

!!! tip dart库引入注意事项
    dart中有许多核心库,大部分对于flutter创建的各平台应用都支持,但也存在部分库对原生移动端或web端不支持的库,需要根据创建的跨平台应用情况进行选择

## flutter组件

### 框架类

* `MaterialApp`:作为一个基础的框架,通过此
