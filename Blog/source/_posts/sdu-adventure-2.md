---
title: 山大迷踪移动日志2
date: 2022-10-08 22:27:32
tags: 学线实训
---

# 日志

| 日期 | 任务                                       |
| ---- | ------------------------------------------ |
| 7.20 | 后端没出，安照i山大的接口，做了一下登录    |
| 7.22 | 画UI                                       |
| 7.24 | 完善登录，封装网络请求                     |
| 7.25 | 上传头像，修改昵称                         |
| 7.27 | 导航栏，个人信息UI                         |
| 7.28 | 个人信息，修改个人信息，迷踪贴审核         |
| 7.29 | 管理员功能，完成个人信息板块               |
| 7.30 | 消息通知，迷踪社区，校区地图没出，首页待定 |
| 7.31 | 更新获取自己迷踪贴的接口                   |

# 踩坑

## 订正上一篇帖子

出现不带引号的response值（

```
{code: 0, message: success, data: [6afe0c0544625b5204b2588f9021adc5,0b49ef4b6388c3303082800980b6bca4]}
```

）的原因是没有设置ResponseType，添加如下语句后，返回的response值就是json字符串的格式了

```dart
BaseOptions options = BaseOptions();
options.responseType = ResponseType.plain;
```

此外，上一篇的对字符串处理的方法是错误的，当遇到复杂的JSON时会出bug

## Appbar的颜色

一般颜色可以这样改

```dart
ThemeData(
        primarySwatch: Colors.xxx,
      ),
```

但不能是Colors.white或Colors.black，因为它们不被认为是MaterialColor，此外也不能使用自定义的颜色，但是我们可以通过自定义一个产生MaterialColor的函数来自定义主题色

```dart
MaterialColor createMaterialColor(Color color) {
  List strengths = <double>[.05];
  Map<int, Color> swatch = {};
  final int r = color.red, g = color.green, b = color.blue;
  for (int i = 1; i < 10; i++) {
    strengths.add(0.1 * i);
  }
  strengths.forEach((strength) {
    final double ds = 0.5 - strength;
    swatch[(strength * 1000).round()] = Color.fromRGBO(
      r + ((ds < 0 ? r : (255 - r)) * ds).round(),
      g + ((ds < 0 ? g : (255 - g)) * ds).round(),
      b + ((ds < 0 ? b : (255 - b)) * ds).round(),
      1,
    );
  });
  return MaterialColor(color.value, swatch);
}
```

这样就可以写成下面这种形式

```dart
ThemeData(
        primarySwatch: createMaterialColor(Color(0x9c0c13)),//山大红
      ),
```

## 弹出键盘导致图形溢出

当有输入框的时候，点击弹出键盘之后，会压缩实际屏幕的区域，有可能导致控件的溢出。

![控件溢出](/img/sdu-adventure-2/overflow.jpeg)

解决方法有两种：

1.输入框抵住键盘使内容不随键盘滚动

在Scaffold中加入

```dart
resizeToAvoidBottomPadding: false,
```

2.使用SingleChildScrollView

在你希望不被遮挡的地方，套上一层SingleChildScrollView

## 瀑布流

依赖：flutter_staggered_grid_view

网上大多数的说法是使用StaggeredGridView，但是目前StaggeredGridView已被弃用了，所以我们使用SingleChildScrollView + StaggeredGrid的方式实现瀑布流。

```dart
Container(
    color: Colors.grey[200],
    padding: const EdgeInsets.all(20),
    child: SingleChildScrollView(
        scrollDirection: Axis.vertical,
        child: StaggeredGrid.count(
            mainAxisSpacing: 10,
            crossAxisSpacing: 10,
            crossAxisCount: 2,
            children: _list(),
        ),
    ),
)
```

这样实现的是整齐的网格，要实现高低错落的样子，（我想到的）有两种方法：

1.把高度设置为基础高度+随机高度

2.把children的List的第一个设置为一个有高度的空箱子，其他的高度相同

## get与dio的冲突

上传图片的时候，我采用了以下的方式：

```dart
Asset a = resultList[0];
ByteData byteData = await a.getByteData();
List<int> imageData = byteData.buffer.asUint8List();
MultipartFile multipartFile = MultipartFile.fromBytes(
imageData,
filename: 'head_img.jpg',
contentType: MediaType('image','jpg')
);
```

报错：The name 'MultipartFile' is defined in the libraries 'package:dio/src/multipart_file.dart (via package:dio/dio.dart)' and 'package:get/get_connect/http/src/multipart/multipart_file.dart

然鹅，来自get的MultipartFile没有fromBytes这个方法

我的做法是：

```dart
import 'package:get/get.dart' as Getx;
```

这样，MultipartFile就默认来自dio了，然后在使用Get进行跳转的时候，写成

```dart
Getx.Get.to();
```

## 渐变色

```dart
Container(
    decoration: const BoxDecoration(
        gradient: LinearGradient(
            begin: Alignment.topLeft,
            end: Alignment.bottomRight,
            colors: [
                Color.fromRGBO(245, 156, 242, 0.1),
                Color.fromRGBO(225, 110, 64, 0.1),
                Color.fromRGBO(64, 196, 255, 0.1),
                Color.fromRGBO(178, 255, 89, 0.1),
            ]
        )
    )
)
```

## 绝对布局

Stack和Positioned配合使用（对不起，我今天才知道

