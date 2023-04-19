---
title: "Flutter框架学习笔记"
categories: [ "学习" ]
tags: [ "Flutter" ]
draft: false
slug: "141"
date: "2022-03-22 12:00:00"
---

Flutter是谷歌开源的跨平台UI框架，可以快速在iOS和Android上构建高质量的原生用户界面，可在Windows，Linux，Android，Web，iOS，Mac等6大平台上开发应用


闲鱼和Now直播，美团，快手都使用了Flutter


获取Flutter

https://storage.flutter-io.cn/flutter_infra_release/releases/stable/windows/flutter_windows_2.10.3-stable.zip

添加path环境变量

由于Flutter库是在google那，因此需要设置第三方可信镜像库

设置PUB_HOSTED_URL和FLUTTER_STORAGE_BASE_URL环境变量

PUB_HOSTED_URL设置为https://pub.flutter-io.cn

FLUTTER_STORAGE_BASE_URL设置为https://storage.flutter-io.cn

（flutter-io.cn所提供的镜像由中国的Flutter开发者社区提供和维护）

其他可信第三方镜像库：

腾讯云镜像

PUB_HOSTED_URL（https://mirrors.cloud.tencent.com/dart-pub）
FLUTTER_STORAGE_BASE_URL（https://mirrors.cloud.tencent.com/flutter）


清华大学镜像

PUB_HOSTED_URL（https://mirrors.tuna.tsinghua.edu.cn/dart-pub）
FLUTTER_STORAGE_BASE_URL（https://mirrors.tuna.tsinghua.edu.cn/flutter）





添加Flutter环境变量path，解压路径\flutter\bin


执行where.exe flutter dart，如果有反应，说明path环境配置完成

（如果要开发安卓的话，需要安装jdk，Android Studio，Android Jdk，可执行flutter doctor检查依赖（如果是X表示没依赖，需要安装））

这里用Visual Studio Code的Flutter插件

创建第一个demo（项目名必须全小写，可用_下划线）


flutter create flutterdemo


启动项目（编译执行）

flutter run


---


Dart是静态类型语言，它会在定义时绑定数据类型（var）

Dart允许一个类中有多个构造函数，在new初始化时，可选择类的某个构造函数

Dart库管理（pub.dev）,在pubspec.yaml添加库


实质上Dart和JavaScript很相似，只是它有抽象和泛型（ts也有泛型，抽象类就是类似于golang的接口，只定义不实现）

Dart也是单线程执行，主线程外也有宏任务队列和事件队列（可以理解为JavaScript中的宏任务）


Dart执行过程：执行main()函数，判断是微任务还是事件队列，是微任务则插入微任务队列，是宏任务则插入宏任务队列，执行完成后（主线程），会执行微任务队列和事件队列，以及判断微任务队列和事件队列是否为空，当为空时程序执行结束


---


flutter项目下的lib/main.dart，class MyApp类下

修改为 home: const MyHomePage(title: 'Hallo word'), // 当前title信息


在return Scaffold下的body: Center下的，修改为children: <Widget>[const Text('Hallo word',),


运行后可以看到一个title以及内容都为Hallo word的app


flutter自带了可视化工具，Dart DevTools



自己写一个main.dart


    void main() => runApp(MyApp());
    class MyApp extends StatelessWidget{
      @override
      Widget build(BuildContext context){
        return MaterialApp(
          title: "hallo word", // app的title
          theme: ThemeData(
              primarySwatch: Colors.orange, // 页面的主题颜色
          )
          home: Scaffold(
            appBar: AppBar(
              title: Text("hallo word"), // 当前页面的title
            ),
            body: Center(
              child: Text('hallo flutter'), // 当前页面的文本
            ),
          ),
        );
      }
    }


flutter run

（热更新，在执行中输入r或者R）（按p为显示网格，按q为退出，按o为切换android和ios的预览模式）

这边用安卓实机调试（也可以用安卓模拟器，例如夜神模拟器）

使用flutter devices命令检查是否寻找到该机，记得开启开发者模式，打开调试，允许调试，开启允许USB安装，数据线连接到电脑，并且安装和实机（虚拟机）中的安卓版本sdk，如果官网打不开，可去https://www.androiddevtools.cn/下载或者在Android Studio中安装，下载安装Google USB Driveer（也可以在Android Studio中安装）



---

flutter生命周期


flutter组件分为无状态组件和有状态组件，无状态组件就是单纯显示内容的，没有逻辑计算，因此只渲染一次，有状态组件就是具备逻辑交互功能的组件，会因为数据发生变化而多次渲染，这个概念和JavaScript是一样的

无状态组件的生命周期只有一个，build


有状态组件生命周期分别为

createState：该钩子在StatefulWidget中创建State的方法，当StatefulWidget被调用时会立即执行 createState

initState：该钩子为State初始化调用，一般用来初始化State变量的赋值，与服务端交互获取服务端数据以后调用setState来设置初始化State。

didChangeDependencies：该钩子在该组件依赖的State发生变化时会被调用（这里的State发生变化指全局State变化，例如当InheritedWidget发送变化），build跟着触发

build：返回需要渲染的Widget，一般有来返回Widget相关逻辑，build会被调用多次

setState：当状态发送变化时触发，该钩子被执行后，必然会调用build钩子

reassemble：该钩子在debug模式下，每次热重载都会执行该钩子，一般来用做一些debug操作

didUpdateWidget：该钩子主要是在组件重新构建时调用，例如热重载，该钩子被执行后，必然会调用build钩子

deactivate：该钩子在组件被移除节点后会被调用，如果没有插入到其他节点，会继续触发dispose钩子

dispose：永久移除该组件，释放组件资源，一个组件的生命终点


在无状态组件中，执行阶段中只有build，也只会执行build函数钩子，因此执行和效率比有状态组件好

注意：当动态组件更新时，将导致其子组件更新，导致性能问题

常见组件：


Text: 渲染文本组件

Image :图片显示组件

Icon : Icon库组件

AppBar：页面导航条组件

Row: 布局组件，使子元素在水平方横向布局

Column: 布局组件，使子元素在水平方向纵向布局

Container: 容器组件

Expanded：控制flex布局的占位（用在row或者column组件内部）

Stack：层叠布局组件，在当前组件层叠另一层（和css的z-index类似）

FadeInImage：加载时的占位组件

Padding：填充空白区域组件，和css的padding效果类似

ClipRRect：圆角组件，可将子组件处理成圆边角



Text组件，它有TextAlign属性，maxLines属性，overflow属性，style属性

TextAlign属性就是定义文本对齐方式的，例如：

    child:Text(
        'hallo word',
        textAlign:TextAlign.center,
    )

left左，center居中，right右

maxLines属性是定义文本显示的最大行数数，例如：

    child:Text(
        'hallo word',
        textAlign:TextAlign.center,
        maxLines: 1,
    )

overflow属性是定义文本溢出时的处理方式，例如：

    child:Text(
        'hallo word',
        textAlign:TextAlign.center,
        maxLines: 1,
        overflow: TextOverflow.ellipsis,
    )

注意：Text组件是没有宽度的，文本会撑开Text组件，因此还需要搭配Container组件使用，例如：

    Container(
        width: 10,
        child: Text(
            "hallo wordhallo wordhallo wordhallo wordhallo wordhallo word",
            textAlign: TextAlign.left,
            overflow: TextOverflow.ellipsis,
            maxLines: 1,
        ),
    ),


style属性可以理解成Flutter组件的css（实质上效果都类似css），例如：

    child:Text(
        'hallo word',
        textAlign: TextAlign.center,
        style: TextStyle(
            fontSize: 20.0, // 字体大小20
            color: Colors.red, // 字体颜色为红色
            decoration: TextDecoration.underline, // 文本下划线
        ),
    )




Container组件，有Alignment属性，padding属性，margin属性和decoration属性

Container组件宽度和高度，以及颜色可直接通过width和height属性，color属性来定义

Alignment属性：该属性是定义Container组件的内容的对齐方式，例如：

    child:Container(
        child:new Text('hallo word',style: color: Colors.red,),
        lignment: Alignment.center,
    ),


该属性可设置头部对齐方式，底部对齐方式，水平对齐方式，例如topCenter，center，bottomCenter

padding属性：该属性定义Container组件边缘和子内容的距离，和css的内边距类似，例如：

    child:Container(
        child:new Text('hallo word',style: color: Colors.red,),
        lignment: Alignment.center,
        padding:const EdgeInsets.all(20.0), // 上下左右边距都为20
    ),


如果想单独设置，可padding:const EdgeInsets.fromLTRB(10.0,20.0,30.0,40.0),

这4个值分别表示左，上，右，下


margin属性，自然是定义外边距的，例如margin: const EdgeInsets.all(20.0),

decoration属性，用于定义背景，边框，例如：

    child:Container(
        child:new Text('hallo word',style: color: Colors.red,),
        lignment: Alignment.center,
        padding:const EdgeInsets.all(20.0), // 上下左右边距都为20
        decoration:new BoxDecoration(
            gradient:const LinearGradient(
                colors:[
                    Colors.red, // 红色到黑色的渐变
                    Colors.black,
                ]
            ),
            border:Border.all(width:3.0,color:Colors.black) // 黑色边框，边框宽度为3
        ),
    ),



Image组件，加载图片有4种方式，分别为：

Image.asset 加载项目内图片（相对）
Image.file 加载本地图片（绝对）
Image.memory 加载Uint8List资源图片
Image.network 加载网络图片

加载网络图片，例如：
            
    child:new Image.network(
        'http://xiaochenabc123.test.com/1.jpg',
    ),


加载本地（或者项目）内图片，例如：

    child:new Image.file(
        File("./1.jpg")
    )

Image组件的ImageRepeat属性和fit属性

ImageRepeat属性可设置图片重复，例如铺满整个容器，fit属性可设置图片的拉伸和挤压，例如：全图显示，拉伸填满整个容器

例子：


    child:new Image.network(
        'http://xiaochenabc123.test.com/1.jpg',
        // ImageRepeat: ImageRepeat.repeat // 横向和纵向重复，直到填满容器
        fit: BoxFit.contain // 显示原比例图片
    ),


ImageRepeat.repeat: 横向和纵向重复，填满整个容器
ImageRepeat.repeatX: 横向重复，纵向不重复
ImageRepeat.repeatY：纵向重复，横向不重复



BoxFit.fill: 图片拉伸，并填满父容器。
BoxFit.contain: 显示原比例图片
BoxFit.cover：可能拉伸，裁切（图片填满整个容器，但是不变形）
BoxFit.fitWidth：宽度充满（横向填满），图片可能拉伸，裁切
BoxFit.fitHeight ：高度充满（竖向填满）,图片可能拉伸，裁切
BoxFit.scaleDown：显示原比例图片，但是此属性不允许超过源图片大小



Row组件：水平布局组件，该组件又分为灵活布局和非灵活布局

灵活布局：使用Expanded（类似于flex效果），解决非灵活布局的空余或者溢出的情况

非灵活布局：当子元素不足填满时，会有空余位置，当子元素溢出位置了，会警告

例如：


    body: Row(
        children: <Widget>[
            Expanded(
                flex: 1,
                child: Container(
                    Colors.black,
                ),
            ),
            Expanded(
                flex: 2,
                child: Container(
                    Colors.red,
                ),
            ),
        ]   
    )


Column组件：垂直布局组件，例如：



    body: Column(
        children: <Widget>[
            Text('hallo word'),
            Text('chenjunlinabc'),
        ],
        crossAxisAlignment: CrossAxisAlignment.start,
        mainAxisAlignment: MainAxisAlignment.center,
    ),




CrossAxisAlignment.star：向左对齐
CrossAxisAlignment.end：向右对齐
CrossAxisAlignment.center：居中对齐


Row和Column组件都存在主轴（main）和纵轴（Cross）

主轴（main）：在Row组件中水平就是主轴，在Column组件中垂直就是主轴，

纵轴（Cross）：和主轴（main）相反，在Row组件中垂直就是纵轴，在Column组件中水平就是纵轴



Stack组件

    Stack(
        children: <Widget>[
            Container(
                width: 100,
                height: 100,
                color: Colors.red,
            ),
            Container(
                width: 50,
                height: 50,
                color: Colors.black,
            ),
        ],
    ),


可以看到上面的两个子组件相交在一起了，Stack组件一样有Alignment属性，来表示对齐方式