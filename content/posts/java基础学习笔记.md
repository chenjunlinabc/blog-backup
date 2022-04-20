---
title: "java基础学习笔记"
categories: [ "默认" ]
tags: [ "java" ]
draft: false
slug: "19"
date: "2021-06-16 09:35:00"
---

java基于java虚拟机（Java Virtual Machine，JVM）和java应用编程接口（Application Programming Interface，API）构成


java的跨平台得益于java虚拟机，只需要编译一次java程序，就可以在不同系统的java虚拟机上运行


java分为Java SE、Java EE 和 Java ME。





类的命名：必须是英文开头，后可字母，数字和下划线

一般为大写字母开头

例如：

public class Abc


public是访问修饰符，表示该class为公开，如果没有写pulic，也可以正常编译，但是这个class将不能在命令行中执行


在class内部可以定义方法，例如:

public static void main(String[] args){}

这个例子的方法名为main，返回值为void，表示没有返回值，空

static是一个关键字，同样也是修饰符，表示静态方法


java入口程序必须是静态方法，方法名也必须为main，参数必须为String数组


在方法内部，语句才能执行，每一行程序都必须要以分号结束;


// 单行注释 



/*

多行注释

*/



/**
* 
* 一样是多行注释
*
*/

java的变量的类型和JavaScript一样，变量分两种，基本类型变量和引用类型变量
java有8种基础类型，分别是整型（4种），字符型（1种），浮点型（2种），布尔型（1种）

变量必须先定义，后赋值使用，例如：

int a = 1;


定义为int整数类型，名称为a，初始值为1


变量的特点就是可以重新赋值

int i = 1;  // 定义int类型，名称为i，赋值为1

i = 100;  // 重新赋值为100

int x = i; // 变量x的值为i，因为i的值为100，所以x的值为i






整型

整型类型：int，byte，short，long

整型类型的数就是整数，整数默认为int，在数值后加上L就代表为long，例如：long abc = 123L;

因为java的整型是有符号类型的，0表示正数，1表示为负数，所以取值范围是从负多少到正多少，不能超过取值范围

整数的值的表示支持二进制，八进制，十进制，十六进制

byte：-128 ~ 127   长度8位
short: -32768 ~ 32767    长度16位
int: -2147483648 ~ 2147483647     长度32位
long: -9223372036854775808 ~ 9223372036854775807    长度64位



浮点型

浮点型类型：float（长度32位），double（长度64位）

浮点型默认为double，在数值后加F，代表为float类型，例如：float abc = 3.14F;

浮点型支持科学计数法（E或者e），e2就是代表为10的2次方，例如：3.14e2 = 3.14x100

float：3.4E-038 ~ 3.4E+038
double：1.7E-308 ~ 1.7E+308



浮点类型的数就是小数

小数值默认位double类型

如果想将一个小数类型设置为float类型，需要在小数后加f，例如：float abc = 3.14 f

float（单精度）精度是7位有效数字，第7位数字后面的会四舍五入

float a = 0.987654321f;  // 实质上为0.9876543 ，2和1被四舍五入了，使用float类型，要在后面加上f

double（双精度）精度是16位有效数字

在计算机中，使用符号位（sign）+指数（exponent）+小数位（fraction），来表示浮点数（浮点表示法）

符号位表示正负，指数表示取值范围，小数位表示计算精度

例如： float类型为32位，符号位占1位，指数位占8位，小数位占23位，小数位不足位数的补0

double类型为64位，符号位占1位，指数占11为，小数位占52位，小数位不足位数的补0


布尔类型

java布尔类型和很多程序语言一样，用来表示真与假，布尔类型只有两个值，true和false，长度为1位

可以赋值变量为布尔类型，也可以通过关系运算来返回

例如：

boolean a = true;

boolean b = 1>2;


字符类型


字符类型char表示一个字符，char类型不但可以表示标准的ASCII，还可以表示一个Unicode字符

char类型使用单引号''来表示，而且只能存储一个字符，长度为16位



字符串

字符串类型String是一个引用类型，例如：

String a = "abc";

引用类型的变量是类似于c语言的指针，内部保存了一个地址，指向某一个对象在内存的位置



\\代表为转义符，例如：\\n

类型转换

精度小的类型可以自动转换为精度大的类型
精度大的类型，需要强制转换，才能转换到精度小的类型，而且还可能溢出

例如：

自动转换

int a= 100;

long b;

a =b;

强制转换

int a = 999;

byte a = (byte)b;

因为byte类型取值范围为：-128 ~ 127，999超过了byte类型的取值范围

如果一个变量在类下声明，那么这个变量整个类都可以访问，其作用域就在其声明的那个类中，覆盖整个类

当一个变量在一个方法内部被定义，那么这个变量只能作用于该方法内部，这也叫为局部变量，方法一执行结束，该变量就被销毁


常量

使用final修饰符定义常量，例如

final int a = 123;

常量在定义时初始化后就不能重新赋值了，常量的命名习惯为全部大写

var 关键字

var关键字会自动推断变量的类型，使用var定义变量只是少写了变量类型，例如

var a = 123;


运算

整数的运算，是精确的，因为只取结果的整数部分


溢出：指一个数超过数据类型的取值范围了，只要这个数超出了取值范围，那么就会产生溢出，溢出不会报错，会返回一个数，这个数会通过二进制运算来处理

+=，-=，*=，/=

例如：
a+=2  // 相当于 a = a + 2

++和--运算

a++ // 相当于 a = a + 1
a--   // 相当于 a = a - 1


a++和++a

++a ，表示先加1，再引用a，a++，表示引用a，再加1

\>大于，>=大于或等于，<小于，<=小于或等于，==是否相等，!=是否不相等

&：与，当俩边都为真时，为真，两边的表达式都处理
&&：与，当俩边都为真时，为真，只要第一个表达式的值为假，第二个就不进行处理
|：或，当两边都为假时，为假，任意一个表达式的值为真，则为真，两边的表达式都处理
||：或，当两边都为假时，为假，任意一个表达式的值为真，则为真，只要第一个表达式的值为真，第二个就不进行处理
!：取反，当表达式的值为真时，返回假，表达式的值为假时，返回真
^：异或，表达式的值不同时，返回真，相同时返回假


移位运算

因为在计算机中整数是以二进制表示的，所以可以通过移位运算

Integer.toBinaryString() 方法可以将十进制转换为二进制，例如：

int a =10;
String b = (Integer.toBinaryString(a));

b的值为1010

例如：

int abc = 100;

int a = abc << 3;  // 向左移动3位 a = 800

int b = abc >> 3;  // 向右移动3位，b = 12

int c = -1 >>> 2; // 无符号向右移，c = 1073741823


位或|：当对应的二进位中，有一个为1时，结果为1，例如

1|2 ： 1的二进制为1，2的二进制为10，那么得到的二进制结果为11，转换为十进制为3

位与&：当对应的二进位中，俩边都为1时，结果为1，例如

1&2 ：二进制结果为10，十进制结果为2

异或^：当对应的二进位中，两边的值都不为1时，结果为1，例如：

1^2：二进制结果为01，十进制结果为1

取非~：将二进制位倒反，将0改为1，将1改为0，例如：

int a = 10 // 10的二进制为1010

~a  // 值为0101，十进制结果为5

= ：赋值
+= ： 自加，例如i+=1，那么等于i=i+1
-=：自减，例如i-=1，那么等于i=i-1
还有%=，&=，/=，|=，^=，<<=，>>=，>>>=都是类似的

三元操作符

基础语法为：

a=x>c?值1:值2

用if语句来看就等于:

if(x>c){
a=值1
}else{
a=值2
}


if判断

根据条件来执行或者不执行某段语句，基本语法例如：

if（条件）{
// 当条件满足时执行的语句
}else{
// 当条件不满足时执行的语句
}

例如：
if(a>=100){
System.out.println("a大于或者等于100");
}else{
System.out.println("a不大于或者等于100");
}


switch判断

根据表达式的结果来执行相对应的语句，例如：

switch(a){
case 1:
System.out.println("a等于1");
break;
case 2:
System.out.println("a等于2");
break;
default:
System.out.println("不知道a等于多少");
}

switch也可以匹配字符串


while循环

循环就是根据条件进行循环处理，当条件满足时循环处理，当条件不满足时退出循环

例如：

while (a>=100){
System.out.println(a);
a++;
}
while循环是先判断后循环处理，当初始条件不满足时，那么一次循环都不会发生

do...while就是先执行再判断条件，例如：

do{
System.out.println(a);
a++;
}while (a>=100);

先执行do里面的语句，再判断条件是否满足，所以do...while最低也会执行一次，哪怕初始条件也是不满足的



for循环

for和while不一样，它是使用计数器（自增或者自减）来实现循环，例如：

for (int a=1;a<=100;a++){
system.out.println(a);
}

for可以用于数组遍历


break语句用于退出当前循环

continue语句用于提前结束本次循环，直接执行下次循环

例如：

for (int a=1;a<=100;a++){
system.out.println(a);
if(a==2){
break;
}
if(a==3){
continue;
}

数组

数组的定义及遍历

int[] arr = {5,1,3,7,8,2,6,0,10,4,9};

for (int i=0;i<arr.length;i++){
system.out.println(arr[i]);
}

循环遍历的另一种方式

for(int a:b){
System.out.println(a);
}



数组的每一个元素都可以通过索引来访问，数组的索引是从0开始，例如数组的第一个元素，就是arr[0]，通过for循环，当i不小于时停止循环，i为索引值，arr.length为该数组的长度，从而达到输出数组的元素

int[] arr = {5,1,3,7,8,2,6,0,10,4,9};

for (int i;arr){
system.out.println(i);
}
	
i:arr方法可以让变量i拿到arr数组的元素，而不是通过索引获取arr数组的值


多维数组的定义及遍历

Int[][] arr={{1,4,6},{,3,5,7},{9,2,2}};

for (int i=0;i<arr.length;i++){
for(int r=0;r<arr[i].length;r++){
system.out.println(arr[i][r]);

}
}

通过for嵌套，达到遍历二维数组及多维数组



数组的排序

冒泡排序

int[] arr = {5,1,3,7,8,2,6,0,10,4,9};

for (int i=0;i<arr.length-1;i++){
for(int r=0;r<arr.length-i-1;r++){
if(arr[r]>arr[r+1]{
int a = arr[r];
arr[r]=arr[r+1];
arr[r+1] =a;
)
}
}
system.out.println(Arrays.toString(arr));

通过循环，将相邻的数的位置互换，最大的数被换到末尾，对数组的排序是会修改数组本身，最好新建个数组，将数组的元素放入新的数组里，操作新的数组

arraycopy(被复制的数组,从被复制数组中复制数组的开始位置,需要复制的目标数组,复制到目标的数组的开始位置,复制的长度)

二维数组

int a[][] = new int[][]{
{1,2,3},
{9,8,7}
};

int a[][] = new int\[9][8];  # 有9个一维数组，每个一维数组的长度为8，

a\[3][6]=88; # 可以直接访问已经定义好的数组，并且赋值


众所周知，java是一门面对对象的语言

面向对象的实现是继承性和多态性

面向对象的概念是方法，类，实例


面对对象基础

类是实例的构造，类是实例模板，定义了创建实例的方法，实例是根据类创建


定义类

class Hallo{
	public int a;
	public String b;
	public double c;
}

一个类可以包含字段，字段用于描述类的特征，这里定义了三个字段，分别是int类型的，命名为a，string类型的，命名为b，double类型的，命名为c，通过将这一组数据放在一个对象上，达到数据封装


创建实例

Hallo abc = new Hallo();

建立一个Hallo类的实例，通过变量abc指向该实例，Hallo abc是定义Hallo类型的变量abc，new Hallo()才是创建Hallo实例

实例可以通过变量.字段方法来访问到实例变量，例如：

Go maina = new Go();
        maina.a = 18;
        maina.b ="hallo";
        maina.c = 1.2;
        System.out.println(maina.b);

    }
}

class Go {
    public int a;
    public String b;
    public double c;
}



注意：在不同的实例中定义的变量，即便变量名相同，但是在内存中，是不同的，互不干扰，可以理解为局部变量的意思


并不建议在实例中直接操作字段，这样会破坏封装性

方法

使用private修饰的字段，外部程序是不能访问这些字段的，为了处理这个问题，需要用到方法来间接处理这些字段


Go maina = new Go();
        maina.setA(18);
        maina.setB("hallo");
        System.out.println(maina.getA()+maina.getB);
    }
}
class Go {
    private int a;
    private String b;

    public int getA(){
        return this.a;
    }

    public void setA(int a) {
        this.a = a;
    }

    public String getB(){
        return this.b;
    }

    public void setB(String b) {
        this.b = b;
    }
}


外部程序通过调用方法getA()和getB()来间接修改字段


一个类通过定义方法，应该给外部程序提供可以操作的api，也要保证内部的逻辑性

调用方法：实例.方法(参数);


private方法

该方法不允许外部调用，主要是在内部方法调用

public class hallo {
    public static void main(String[] args) {
        go oo = new go();
        oo.setAge(2002);
        System.out.println(oo.getAge());

    }
}
class go{
    private  String name;
    private  int age;

    public void setAge(int age){
        this.age = age;
    }
    public int getAge(){
        return abc(2020);
    }
    private int abc(int xyz){
        return xyz - this.age;
    }
}

abc() 就是一个private方法


this变量


在方法内部，有一个变量this，这个变量始终指向当前的实例，例如this.age

一般来说，命名没有冲突，可以省略this，但是如果字段同名或者有局部变量，那么必须要加上this



方法参数

方法可以提供0个或者任意个参数，方法参数用于提供变量给方法
例如
class hallo{
    public void abc(int age, String name){}

参数必须要符合个数要求和数据类型要求

package包

package hallo; # 声明该类处在那个包中

import hallo.yes; # 导入类中需要的类(用来导入其他包的类)


访问修饰符

修饰符有四种：

private # 私有的

package/friendly/default # 不可写的

protected # 受保护的

public # 公共的


例如：public String yes;  


类与类之间的关系

继承：指一个类继承另一个类的功能，来增加本身的功能

实现：指一个类实现接口功能，是类和接口之间最常见的关系

依赖：指一个类使用了另一个类，而依赖于另一个类，另一个类发生改变有可能导致影响当前类

关联：和依赖关系类似，不过是更强的依赖关系，关联关系可以单向关联，也可以双向关联

聚合：和关联关系类似，不过整体和部分可以分离，有独自的生命周期

组合：和关联关系类似，不过整体和部分不可以分离，整体的生命周期结束后那么部分的生命周期也结束了





private：本身可以访问，但是不能继承类，不能访问同包下的类和其他包下的类

package/friendly/default ：本身可以访问，同包下的类可以继承，不同包下的类不能继承，可以访问同包下的类，不能访问不同包下的类

protected：本身可以访问，同包下的类可以继承，不同包下的类可以继承，可以访问同包下的类，不能访问不同包下的类

public：本身可以访问，同包下的类可以继承，不同包下的类可以继承，可以访问同包下的类，可以访问不同包下的类


访问修饰符一般用来封装，将每一个类的作用明确化


实例属性和类属性

当一个属性声明为类属性，那么所有的对象都可以使用这个值，例如：

    public class Untitled {
        public String name; // 对象属性
        static String yes; // 类属性

        public static void main(String[] args) {
            Untitled main =  new Untitled();
            main.name = "yes";
            Untitled.yes = "gg";
            System.out.println(main.name);
            System.out.println(main.yes);
            Untitled max =  new Untitled();
            max.name = "no";
            System.out.println(max.name);    
            System.out.println(max.yes);
        }
    }


输出yes，gg，no，gg


可以通过main.yes和UntUntitled.yes来访问类属性

只有当一个类的所有对象的某个值都相同时，可以使用类属性

当一个类的对象的某个值不同时，使用实例属性，让这个值可以根据对象的不同来自定义


类方法和实例方法

实例方法必须要有对象才能调用，但是类方法可以直接通过类来调用，不需要对象的存在

例如：

    public class Untitled {
        public String name; 
        protected String yes;
        public void hallo(){
            yes = "hallo word";
            System.out.println(yes);
        }
        public static void name() {
            System.out.println("hallo java");
        }
        
         
        public static void main(String[] args) {
            Untitled main =  new Untitled();
            main.hallo();
            Untitled.name();
        }
    }
    
    

可以通过main.name()或者UntUntitled.name()来调用类方法

如果在一个方法中调用了对象属性，那么使用实例方法，如果在一个方法中没有调用任何对象属性，那么使用类方法

---

java输出中文乱码，可能是javac读取java文件使用的字符集错误

javac -encoding utf-8 *.java

