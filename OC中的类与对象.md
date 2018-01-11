
从编程发展史来看，早期软件比较简单，我们只需要面向过程编程：

- 定义函数
- 定义数据
- 各种函数和数据的操作

当软件发展起来，软件越来越大，代码量也越来越大，我们的编写就有麻烦了：

- 函数越来越多，命名容易冲突。
- 代码重复
- 变量生命周期难以管理

为了解决上述问题面向对象思想呼之而来。

### 1. 类、对象与模块
面向对象从传统意义上的解释是封装，继承和多态。打个比方现在需要盖一栋房子，分为三个步骤：

- 打地基
 - 需要用到工种：钢筋工、瓦工
 - 需要用到的材料：混凝土、钢筋
- 砌墙体
 - 需要用到工种：瓦工
 - 需要用到的材料：砖、混凝土
- 盖屋顶
 - 需要用到的工种：木工、瓦工
 - 需要用到的材料：木头、瓦片、混凝土


这几个步骤抽象出来：钢筋工、瓦工、木工、混凝土、钢筋、砖、瓦片等，然后你发现钢筋工、瓦工、木工有共同的行为行为work（干活），于是抽象出一个工人。砖、混凝土、钢筋、瓦片也有一些共同特性比如膨胀系数等，然后就从中衍生出抽象。

 - 遇到重复的逻辑，可实现抽象。
 - 粒度，即如果只被依赖某一部分，则表明可继续分解。

**递归是编程当中一个重要思想，递归当中有终止条件，模块设计与抽象（或函数调用）也有终止条件，否则如上所说，砖、混凝土、钢筋、瓦片还可以抽象出分子原子，但这对我们是毫无意义的。终止条件是什么，这个是靠靠工程师的经验来判断**

砖、混凝土、钢筋、瓦片从这个例子当中只有一些特性，没有行为，我认为算是一种数据结构不能称之为模块，而钢筋工、瓦工、木工会有work的行为，那这便可以称之模块。打地基、砌墙体、盖屋顶需要对一些模块以及数据结构进行处理，可以称之为复合或复杂性模块。之后盖房子也抽象出一个模块由打地基、砌墙体、盖屋顶三个模块构成。

由类创建出来的实例称为对象。

### 2. OC中的对象

上一章介绍的类和对象抽象描述。从物理层面上一个对象在内存当中的形态是怎样呢？

#####  数组的物理形态
这里先介绍下`数组`在内存当中的形态。以64位操作系统，且存放对象的数组为前提。

![icon](https://github.com/sun6boys/Documents/blob/master/Resources/array.png?raw=true)

如上图所示，数组的本质是一块连续内存，从array基地址第0-8个字节存放第一个元素的地址，第9-16个字节存放第二个元素的地址......

所以当我们用array[x] 取对象时会从array基地址 + x * 8字节 ~ array基地址 + （x + 1） * 8的内存中取出内容即为该对象的地址。

#####  OC对象的物理形态

假如现在定义一个Person类如下

```
@interface Person : NSObject
{
    NSString *_name;
    NSUInteger _age;
    NSString *_sex；
}
@property (nonatomic,copy) NSString *name;
@property (nonatomic, assign) NSUInteger age;
@property (nonatomic, copy) NSString *sex;
@end

```
那由Person类构建出来的person实例在内存当中的结构如下图，第一个isa是Person基类NSObject的成员变量。

![icon](https://github.com/sun6boys/Documents/blob/master/Resources/instance.png?raw=true)

也就是说，一个实例对象的内存大小，由自己本身的成员变量数量和父类成员变量数量，父类的父类一直到基类NSObject的成员变量数量总和决定的，一个成员变量分配8个字节（64位操作系统） 即使如NSUInteger这样的基本数据类型，也是分配的8个字节。我们可以通过`class_getInstanceSize`获取类的实例对象的内存大小

如果把person当成一个数组person.name 等同于person[1]，

> Objective-c是一个动态语言，可以在运行时添加方法或者交换方法等，但我们都知道一旦类注册后，便不能添加成员变量（包括category中也不能添加成员变量），这是因为如果可以随意添加成员变量，那在添加前创建的实例的内存大小由原先的成员变量数量决定，如果用老实例对象获取新成员变量一定会造成类似数组越界的野指针问题。除非苹果设计成类似数组那样当数组中元素数量达到扩容因子数值，重新开辟一块更大的内存，把原先的内容复制过去，但这好像不太现实。。。

Objcetive-c中的对象即使如NSProxy或者block，内存当中头8个字节一定存放的是名为isa指针。那isa是什么呢？我如何知道第9-16个字节存放的是哪个成员变量的地址呢？

#####  isa

isa可以理解成身份证明，每个实例对象的isa指针指向它所属的类（[person class]）,它是运行时环境加载完成便构建出来存放在内存data段。它是个结构体，也被称为类对象

```
struct objc_class {
    Class _Nonnull isa  OBJC_ISA_AVAILABILITY;

#if !__OBJC2__
    Class _Nullable super_class                              OBJC2_UNAVAILABLE;
    const char * _Nonnull name                               OBJC2_UNAVAILABLE;
    long version                                             OBJC2_UNAVAILABLE;
    long info                                                OBJC2_UNAVAILABLE;
    long instance_size                                       OBJC2_UNAVAILABLE;
    struct objc_ivar_list * _Nullable ivars                  OBJC2_UNAVAILABLE;
    struct objc_method_list * _Nullable * _Nullable methodLists                    OBJC2_UNAVAILABLE;
    struct objc_cache * _Nonnull cache                       OBJC2_UNAVAILABLE;
    struct objc_protocol_list * _Nullable protocols          OBJC2_UNAVAILABLE;
#endif

} OBJC2_UNAVAILABLE;

```

 - **Class _Nonnull isa** 类对象头8个字节也是一个isa指针，所以它也是一个对象，它指向的是meta class（此处略过不表）
 - **Class super_class** 父类对象的指针
 - **const char name** 类名
 - **long version** 类的版本信息
 - **long instance_size** 实例对象所占内存大小
 - **ivars** 成员变量集合
 - **methodLists** 实例方法列表
 - **cache** 最近调用的方法列表
 - **protocols** 协议列表

> Runtime的一些知识网上较多，此处略过不表，可以去查询比如oc中函数如何调用，交换方法，meta class等知识。

类对象当中`objc_ivar_list`存放的成员变量的集合`objc_ivar`,上一小节中提到person.name是如何知道name的存放地址距person基地址偏移量呢 ，可以通过`ivar_getOffset`来获得具体的偏移量。

### PS
本文讲述了类的抽象描述以及OC对象的内存形态。如果您通过这篇文章知道下面`void *p`为什么会被当做Person的实例对象，那我的目的便达到了。

```
Class personClass = [Person class];
void *p = &personClass;
```