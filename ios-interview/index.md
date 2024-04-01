# iOS面试题-OC

<!--more-->
## 前言
整理一些ios面试题，方便复习
## OC
### OC对象
{{< admonition open=false type=question title="什么是内存对齐，内存对齐规则是什么样的？">}}
内存对齐说白了就是为了提高CPU寻址操作性能的一种规则。就像砖块一样,如果砖块有大有小毫无规律的摆放在一起,那么我们想要找到某一块砖块就会很麻烦,但是如果我们把砖块按照一定的规则整齐摆放,那么我们就可以很快的找到某一块砖块了,这就是内存对齐的原理。<br>
<br>
**内存对齐规则:**<br>
1.结构体的成员相对于结构体首地址的偏移量必须是成员大小的倍数，即后续数据成员的起始位置是该数据成员所占内存大小的整数倍；<br>
2.结构体的大小必须是最大成员大小的倍数<br>

**各数据类型所占字节大小如下(64位):**<br>

| 数据类型 | 大小 |
| :---: | :---: |
| char | 1 |
| short | 2 |
| int | 4 |
| long | 8 |
| long long | 8 |
| float | 4 |
| double | 8 |
| long double | 16 |
| 指针(void *) | 8 |

**OC 对象的结构体中的数据成员的顺序：**<br>
1. 首先是父类对象的结构体<br>
2. 然后是自己本身的数据成员,按照所占内存大小从小到大排列<br>
所以OC对象对应的结构体第一个数据成员始终是isa指针。<br>

**相关函数:**<br>
1. class_getInstanceSize表示成员变量的所占的内存大小<br>
2. malloc_size表示实际分配的内存大小<br>
3. sizeof表示变量或者类型的大小，传入结构体，返回的则是结构的大小，传入指针（这里的指针表示C指针，OC的引用）即传入值，则返回传入值的类型大小<br>

**分析一个结构体例子:**<br>
```objc
struct Struct1 {
    double a; // 占用8字节,从偏移量0开始
    char b; // 运用规则1,占用1字节,从偏移量8开始
    int c; // 运用规则1,占用4字节,因为当前偏移量9不是4的倍数,所以从偏移量12开始
    short d; // 运用规则1,占用2字节,因为当前偏移量16是2的倍数,所以从偏移量16开始
    struct Struct2 { // 规则2,结构体的大小必须是最大成员大小的倍数,当前偏移量18不是8的倍数,所以从偏移量24开始
        double a; // 运用规则1,占用8字节,从偏移量24开始
        int b; // 运用规则1,占用4字节,因为当前偏移量32是4的倍数,所以从偏移量32开始
        char c; // 运用规则1,占用1字节,因为整体系数是8,33补齐到8的,则整体占用40字节
    }str2;
}str1;
```
`sizeof(str1)`的结果是40<br>
{{< /admonition >}}

{{< admonition open=false type=question title="instance对象，class对象，mate-class对象的区别与关系? 在内存中各自存储哪些信息">}}
**instance对象**<br>
instance对象就是通过类alloc出来的对象，每次调用alloc都会产生新的instance对象<br>
内存中存储着:isa指针，其他成员变量<br>

**class对象**<br>
class对象是类对象，每个类在内存中都有唯一的一个class对象，class对象是一个结构体，里面存储着类的一些信息，比如类的成员变量、属性、方法、遵守的协议等等<br>

**meta-class对象**<br>
meta-class对象是元类对象，每个类在内存中都有唯一的一个meta-class对象，meta-class对象也是一个结构体，里面存储着类方法、遵守的协议等等<br>

**关系**<br>
instance对象的isa指针指向class对象，class对象的isa指针指向meta-class对象，meta-class对象的isa指针指向基类的meta-class对象，基类的meta-class对象的isa指针指向自己，形成一个闭环<br>
{{< /admonition >}}

{{< admonition open=false type=question title="怎么判断一个Class对象是否为meta-class？">}}
通过class_isMetaClass函数判断<br>
{{< /admonition >}}

{{< admonition open=false type=question title="isKindOfClass 和 isMemberOfClass的区别">}}
**isKindOfClass:** 判断是否为当前类或者子类的实例<br>
**isMemberOfClass:** 判断是否为当前类的实例<br>
{{<link href="https://juejin.cn/post/6873793240655462413" content="【图解isKindOfClass和isMemberOfClass方法】">}}
{{< /admonition >}}

{{< admonition open=false type=question title="new与alloc/init的区别？">}}
**new:** new方法是一个类方法，内部会调用alloc方法和init方法，返回一个实例对象<br>
**alloc:** alloc方法是一个类方法，内部会开辟一块内存空间，返回一个实例对象<br>
**init:** init方法是一个实例方法，初始化对象，返回一个实例对象<br>
{{< /admonition >}}

### OC分类

{{< admonition open=false type=question title="Category底层结构是怎么样的❓">}}
引用MJ课件中的一张图<br>
<center>
{{<image src="https://cdn.jsdelivr.net/gh/andy90s/blog-image@master/blog/images/category_t.png" title="Category" width="100%">}}
<div style="color:#717171;font-size:14px;font-weight:normal"> <b> Category </b>  </div>
</center>

**clang编译文件得到的:**<br>
```cpp
struct _category_t {
    const char *name;
    struct _class_t *cls;
    const struct _method_list_t *instance_methods; // Instance对象方法
    const struct _method_list_t *class_methods;// 类方法
    const struct _protocol_list_t *protocols;// 协议
    const struct _prop_list_t *properties; // 属性
};
```
**编译时:**<br>

- 每一个Category都会生成一个category_t结构体对象，记录着所有的属性、方法和协议信息
- 将所有的category_t对象放在一个category_list数组中

**运行时:**<br>

- 通过Runtime加载某个类的所有Category数据
- 把所有Category的对象方法、类方法、属性、协议数据，分别合并到一个二维数组中，并且后面参与编译的Category数据，会在数组的前面
- 将合并后的Category数据（方法、属性、协议），插入到类原来数据的前面

**调用顺序:**<br>

后编译的Category -> 先编译的Category -> 原类(本质原因是它们在方法栈中的顺序不同,优先级不同) -> 父类<br>

**详解参考:**<br>
{{<link href="https://juejin.cn/post/7096480684847415303" content="【Category底层结构】">}}
{{<link href="https://juejin.cn/post/6963889820363915300" content="【Objective-C之Category的底层实现原理】">}}
{{< /admonition >}}

{{< admonition open=false type=question title="为什么说不能添加属性？">}}
在iOS中，分类（Category）是一种将方法添加到现有类中的方式，而不是通过子类化来创建新的类。在Objective-C中，分类可以为现有的类添加方法，但不能添加实例变量或属性。<br>

这是因为Objective-C的运行时系统在类加载时分配内存空间，并将实例变量和属性的偏移量计算到类的内部数据结构中。由于分类是在编译时定义的，它们不能更改类的内部数据结构，因此不能直接添加实例变量或属性。<br>

不过，在Objective-C 2.0中，可以使用关联对象（Associated Object）来向分类中添加属性。关联对象允许将属性与现有的对象相关联，而不是将其存储在对象本身中。这样可以在运行时动态地向现有对象添加属性。关联对象是通过Objective-C运行时API实现的。<br>

其实本质原因是底层结构中没有`ivars`成员列表，所以不能添加属性<br>

**对比原类结构:**<br>

<center>
{{<image src="https://cdn.jsdelivr.net/gh/andy90s/blog-image/blog/images/struct_objc_class%E7%BB%93%E6%9E%84.png" title="原类结构" width="100%">}}
<div style="color:#717171;font-size:14px;font-weight:normal"> <b> 原类结构 </b>  </div>
</center>

<br>

<center>
{{<image src="https://cdn.jsdelivr.net/gh/andy90s/blog-image@master/blog/images/category_t.png" title="Category" width="100%">}}
<div style="color:#717171;font-size:14px;font-weight:normal"> <b> Category </b>  </div>
</center>

{{< /admonition >}}

{{< admonition open=false type=question title="load、initialize的区别">}}
**+load**<br>
当类或者分类被添加到Objective-C runtime时，就会调用+load方法。每个类、分类的+load方法在程序运行过程中只会调用一次，无论这个类有没有被用到，即使这个类没有被用到，也会调用这个类的+load方法，而且是在main函数之前调用。<br>

**+load调用顺序:**<br>
- 先调用类的+load方法，再调用分类的+load方法<br>
- 调用顺序是根据编译顺序决定的，先编译，先调用<br>
- 调用子类的+load方法之前，会先调用父类的+load方法<br>

**+initialize**<br>
+initialize方法会在类第一次接收到消息时调用

**+initialize调用顺序:**<br>
先调用父类的+initialize方法，再调用子类的+initialize方法<br>

**+initialize和+load的很大区别是，+initialize是通过objc_msgSend进行调用的，Load是根据方法地址直接调用的，并不是经过objc_msgSend函数调用，所以有以下特点:**<br>
- 如果子类没有实现+initialize方法，会调用父类的+initialize方法，所以父类的+initialize方法可能会被调用多次<br>
- 如果分类实现了+initialize方法，就会覆盖类本身的+initialize方法，因为分类的方法优先级高于类本身的方法<br>
{{< admonition tip "覆盖说明">}}
参考上面 `Category底层结构是怎么样的`中的调用顺序,可知分类的方法优先级高于类本身的方法,方法查找顺序是先查找分类,再查找类本身,一旦找到就结束查找,所以这里说分类的`+initialize`方法会覆盖类本身的方法
{{< /admonition >}}
{{< /admonition >}}

{{< admonition open=false type=question title="分类(类别)和扩展的区别是什么？">}}
- 类别中原则上只能增加方法（能添加属性的的原因只是通过runtime解决无setter/getter的问题而已）。
- 类扩展不仅可以增加方法，还可以增加实例变量（或者属性），只是该实例变量默认是@private类型的（用范围只能在自身类，而不是子类或其他地方）。
- 类扩展中声明的方法没被实现，编译器会报警，但是类别中的方法没被实现编译器是不会有任何警告的。这是因为类扩展是在编译阶段被添加到类中，而类别是在运行时添加到类中。
- 类扩展不能像类别那样拥有独立的实现部分（@implementation部分），也就是说，类扩展所声明的方法必须依托对应类的实现部分来实现。
- 定义在 .m 文件中的类扩展方法为私有的，定义在 .h 文件（头文件）中的类扩展方法为公有的。类扩展是在 .m 文件中声明私有方法的非常好的方式。
{{< /admonition >}}

### block

{{< admonition open=false type=question title="block是什么?">}}
block本质上是一个对象(它内部也有个isa指针)，封装了函数调用以及函数调用环境的OC对象。<br>

**block的底层结构如下图所示:**<br>

<center>
{{<image src="https://cdn.jsdelivr.net/gh/andy90s/blog-image/blog/images/block_layout.png" title="block" width="70%">}}
<div style="color:#717171;font-size:14px;font-weight:normal"> <b> block </b>  </div>
</center>

{{< /admonition >}}

{{< admonition open=false type=question title="block的类型有哪些?">}}
**全局Block:**<br>
没有访问auto变量的block，编译器会将这种block优化为全局block，存储在程序的数据区域<br>

**栈Block:**<br>
访问了auto变量的block，编译器会将这种block优化为栈block，存储在栈中<br>

**堆Block:**<br>
对栈block调用了copy方法，编译器会将这种block优化为堆block，存储在堆中<br>

<center>
{{<image src="https://cdn.jsdelivr.net/gh/andy90s/blog-image/blog/images/block%E7%B1%BB%E5%9E%8B.png" title="block的类型" width="70%">}}
<div style="color:#717171;font-size:14px;font-weight:normal"> <b> block的类型 </b>  </div>
</center>

{{< admonition tip "说明">}}
auto变量是指在函数内部定义的变量(局部变量)，不包括static修饰的变量
{{<link href="https://zh.wikipedia.org/wiki/%E8%87%AA%E5%8A%A8%E5%8F%98%E9%87%8F" content="【Wiki】">}}
{{< /admonition >}}

{{< /admonition >}}


{{< admonition open=false type=question title="block变量捕获有哪些情况？">}}
**auto变量的捕获**<br>

<center>
{{<image src="https://cdn.jsdelivr.net/gh/andy90s/blog-image/blog/images/block_capture.png" title="变量的捕获" width="95%">}}
<div style="color:#717171;font-size:14px;font-weight:normal"> <b> 变量的捕获 </b>  </div>
</center>

<!-- <center>
{{<image src="https://cdn.jsdelivr.net/gh/andy90s/blog-image/blog/images/block_clang.png" title="auto变量的捕获" width="100%">}}
<div style="color:#717171;font-size:14px;font-weight:normal"> <b> auto变量的捕获 </b>  </div>
</center> -->

{{< /admonition >}}

{{< admonition open=false type=question title="ARC环境下，哪些情况编译器会根据情况自动将栈上的block复制到堆上">}}
- block作为函数返回值时 `return ^{ return 10; };`
- block作为Cocoa API中方法名含有usingBlock的方法参数时 `@[@1,@2,@3].enumerateObjectsUsingBlock:^(id  _Nonnull obj, NSUInteger idx, BOOL * _Nonnull stop) {}`
- block作为GCD API的方法参数时 `dispatch_async(dispatch_get_global_queue(0, 0), ^{ NSLog(@"hello world"); });`
- block作为OC方法中的强引用的循环引用时 `self.block = ^{ NSLog(@"hello world"); };`
{{< /admonition >}}

{{< admonition open=false type=question title="block内部为什么不能修改局部变量，__block为什么能？">}}
**block内部为什么不能修改局部变量:**<br>
block内部不能修改局部变量，是因为block内部会对局部变量进行copy操作，而copy操作是将局部变量的值拷贝到block结构体中，而不是引用，所以block内部修改的是block结构体中的值，而不是局部变量的值<br>

**__block为什么能:**<br>
__block修饰的变量是一个结构体，结构体中有一个isa指针，指向一个对象，这个对象中有一个变量，这个变量的值就是局部变量的值，所以block内部修改的是这个对象中的变量，而不是局部变量的值<br>

```objc
struct __Block_byref_i_0 {
    void *__isa;
    __Block_byref_i_0 *__forwarding;
    int __flags;
    int __size;
    int val; //变量名
};
```
{{< /admonition >}}

### 内存管理

{{< admonition open=false type=question title="OC中内存分区？">}}
**OC中内存分区从高到低:**<br>

**1. 栈区(stack)**<br>

栈是用于存放本地变量，内部临时变量以及有关上下文的内存区域。程序在调用函数时，操作系统会自动通过压栈和弹栈完成保存函数现场等操作，不需要程序员手动干预。<br>

栈是一块连续的内存区域，栈顶的地址和栈的最大容量是系统预先规定好的。能从栈获得的空间较小。如果申请的空间超过栈的剩余空间时，例如递归深度过深，将提示stackoverflow。<br>

栈是机器系统提供的数据结构，计算机会在底层对栈提供支持：分配专门的{{<link href="https://www.jianshu.com/p/6d7a57794122" content="寄存器">}}存放栈的地址，压栈出栈都有专门的指令执行，这就决定了栈的效率比较高。<br>

**2. 堆区(heap)**<br>

堆是用于存放除了栈里的东西之外所有其他东西的内存区域，当使用malloc和free时就是在操作堆中的内存。对于堆来说，释放工作由程序员控制，容易产生memory leak。<br>

堆是向高地址扩展的数据结构，是不连续的内存区域。这是由于系统是用链表来存储的空闲内存地址的，自然是不连续的，而链表的遍历方向是由低地址向高地址。堆的大小受限于计算机系统中有效的虚拟内存。由此可见，堆获得的空间比较灵活，也比较大。<br>

对于堆来讲，频繁的new/delete势必会造成内存空间的不连续，从而造成大量的碎片，使程序效率降低。对于栈来讲，则不会存在这个问题，因为栈是先进后出的队列，永远都不可能有一个内存块从栈中间弹出。<br>

堆都是动态分配的，没有静态分配的堆。栈有2种分配方式：静态分配和动态分配。静态分配是编译器完成的，比如局部变量的分配。动态分配由alloca函数进行分配，但是栈的动态分配和堆是不同的，他的动态分配是由编译器进行释放，无需我们手工实现。<br>

计算机底层并没有对堆的支持，堆则是C/C++函数库提供的，同时由于上面提到的碎片问题，都会导致堆的效率比栈要低。<br>

**3. 全局区(静态区)**<br>

全局区又分为`.bss`段和`.data`段，内存地址一般由0x1开头：
- `.bss`段：`block started by symbol`,未初始化的全局变量和静态变量，程序执行前会自动清0
- `.data`段：`data`,已初始化的全局变量和静态变量

**4. 常量区(.rodata)**<br>

`.rodata`: `read only data`(只读),常量字符串就是放在这里的，程序执行前会自动加载到内存中，程序结束后由系统释放<br>

常量区的内存在编译时就已经确定，主要存放已经使用过的，且没有指向的字符串常量(因为字符串常量可能在程序中多次被使用，所以在程序运行之前就会提前分配内存)。常量区的常量在程序结束后，由系统释放<br>

**5. 代码区(.text)**<br>

存储程序代码，在编译时加载到内存中，代码会被编译成二进制的形式进行存储

{{< /admonition >}}

{{< admonition open=false type=question title="OC中的内存管理机制?">}}
在 Objective-C 中，对象通常是使用 alloc 方法在堆上创建的。 `[NSObject alloc]` 方法会在对堆上分配一块内存，按照NSObject的内部结构填充这块儿内存区域。<br>

一旦对象创建完成，就不可能再移动它了。因为很可能有很多指针都指向这个对象，这些指针并没有被追踪。因此没有办法在移动对象的位置之后更新全部的这些指针。<br>

**MRC和ARC**<br>

Objective-C中提供了两种内存管理机制：MRC（MannulReference Counting）和 ARC(Automatic Reference Counting)，分别提供对内存的手动和自动管理，来满足不同的需求。现在苹果推荐使用 ARC 来进行内存管理。<br>

**MRC**<br>

|对象操作|OC中对应的方法|对应的 retainCount 变化|
| :---: | :---: | :---: |
|创建对象|alloc/new/copy/mutableCopy|+1|
|对象引用|retain|+1|
|对象释放|release|-1|
|对象释放后置空指针|dealloc|0|

**四个法则:**<br>

- 自己生成的对象，自己持有
- 非自己生成的对象，自己也能持有
- 不再需要自己持有的对象时释放
- 非自己持有的对象无法释放

如下是四个黄金法则对应的代码示例：<br>

```objc
// 自己生成的对象，自己持有
NSString *string1 = [[NSString alloc] init];
// 非自己生成的对象，自己也能持有
NSString *string2 = [NSString stringWithFormat:@"hello"];
// 不再需要自己持有的对象时释放
[string1 release];
// 非自己持有的对象无法释放
[string2 release]; // crash
```

autorelease 使得对象在超出生命周期后能正确的被释放(通过调用release方法)。在调用 release 后，对象会被立即释放，而调用 autorelease 后，对象不会被立即释放，而是注册到 autoreleasepool 中，经过一段时间后 pool结束，此时调用release方法，对象被释放。<br>

在MRC的内存管理模式下，与对变量的管理相关的方法有：retain, release 和 autorelease。retain 和 release 方法操作的是引用记数，当引用记数为零时，便自动释放内存。并且可以用 NSAutoreleasePool 对象，对加入自动释放池（autorelease 调用）的变量进行管理，当 drain 时回收内存。<br>

**ARC**<br>

ARC 是编译器特性，编译器会在编译时自动在合适的地方插入 retain/release/autorelease 代码，以此来管理内存。ARC 会在编译时根据代码的上下文来判断应该插入哪些内存管理代码，这样就不需要程序员手动管理内存了。<br>

**变量标识符**<br>

|标识符|说明|
| :---: | :---: |
|__strong|默认的标识符，表示强引用|
|__weak|表示弱引用，不会改变引用计数，当对象被释放后，会自动将指针置为nil|
|__unsafe_unretained|表示弱引用，不会改变引用计数，当对象被释放后，不会自动将指针置为nil|
|__autoreleasing|表示自动释放，一般用于传递参数，表示传递的参数是一个autorelease对象,在函数返回时会被自动释放掉|

**属性标识符**<br>

|标识符|说明|
| :---: | :---: |
|strong|表明属性定义一个拥有者关系。当给属性设定一个新值的时候，首先这个值进行 retain ，旧值进行 release ，然后进行赋值操作|
|weak|表明属性定义了一个非拥有者关系。当给属性设定一个新值的时候，这个值不会进行 retain，旧值也不会进行 release， 而是进行类似 assign 的操作。不过当属性指向的对象被销毁时，该属性会被置为nil。|
|copy|类似于 strong，不过在赋值时进行 copy 操作而不是 retain 操作。通常在需要保留某个不可变对象（NSString最常见），并且防止它被意外改变时使用。|
|assign|表明 setter 仅仅是一个简单的赋值操作，通常用于基本的数值类型，例如CGFloat和NSInteger。|
|unsafe_unretained|语义和 assign 类似，不过是用于对象类型的，表示一个非拥有(unretained)的，同时也不会在对象被销毁时置为nil的(unsafe)关系。|

{{< /admonition >}}

{{< admonition open=false type=question title="讲下自动释放池">}}
Autorelease Pool 提供了一种可以允许你向一个对象延迟发送release消息的机制,当你想要释放一个对象时,只需要将这个对象放入到Autorelease Pool中,系统会在Autorelease Pool被销毁时,向池中所有的对象发送release消息。<br>

所谓的延迟发送release消息指的是，当我们把一个对象标记为autorelease时:<br>
```objc
NSString* str = [[[NSString alloc] initWithString:@"hello"] autorelease];
```
这个对象的 retainCount 会 +1,但是并不会立即release,而是当这段语句所处的autoreleasepool被销毁时,所有标记了autorelease的对象才会被release。<br>

**Autorelease Pool的实现原理**<br>

Autorelease Pool的本质上是一个双向链表。双向链表中每一页为一个AutoreleasePoolPage，AutoreleasePoolPage最大为4096B，每当AutoreleasePoolPage中因为存储变量总大小超过4096B之后，就会分配一个新的AutoreleasePoolPage<br>

objc_autoreleasePoolPush()<br>

每个 AutoreleasePoolPage 对象会开启 4096字节（4kb）内存，除了自身实例变量所占空间，剩下的空间全部拿来存储 autorelease 对象的地址。<br>

每当进行一次 objc_autoreleasePoolPush 调用时，runtime 都会向当前的 AutoreleasePoolPage 中添加一个哨兵对象，值为 nil，添加完哨兵对象后，将 next 指针指向下一个添加 Autorelease 对象的位置。当当前 AutoreleasePoolPage 满了，开启一个新的 AutoreleasePoolPage，并更新 child 和 parent 指针，以组成双向链表。<br>

objc_autoreleasePoolPop()<br>

objc_autoreleasePoolPush 会有个返回值，这个返回值正是前面提到的哨兵对象。<br>
objc_autoreleasePoolPop() 调用时会把哨兵对象作为入参。之后根据传入的哨兵对象地址找到哨兵对象对应的 AutoreleasePoolPage；在当前 page 中，对所有晚于哨兵对象插入的 Autorelease 对象发送 release 消息，到哨兵对象后，销毁当前 page；再根据 parent         向前继续进行 pop，直到第一个哨兵对象所在 page 释放完成。<br>

**Autorelease Pool的用处:**

- 编写不基于UI框架的程序，如命令行工具
- 编写一个创建许多临时对象的循环
- 生成辅助线程（必须在线程开始执行后立即创建Pool，否则将泄露对象。非Cocoa程序创建线程时才需要）
- 长时间在后台运行的任务。

在ARC下,我们并不需要去手动调用autorelease相关方法,甚至可以完全不知道autorelease的存在,就可以正确的管理好内存.因为Cocoa Touch 的Runloop中,每个runloop circle 中系统都自动加入了Autorelease Pool的创建和释放.<br>

当我们需要创建和销毁大量的对象时,使用手动创建的Autorelease Pool可以减少内存峰值,提高程序的性能。因为如果不手动创建的话,外层系统创建的pool会在整个runloop circle结束时才进行drain,而手动创建的pool可以在我们需要的时候进行drain,这样就可以减少内存峰值,提高程序的性能。<br>

```objc
for (int i = 0; i < 100000000; i++)
{
    @autoreleasepool
    {
        NSString* string = @"abc";
        NSArray* array = [string componentsSeparatedByString:string];
    }
}
```
如果不使用autoreleasepool,需要再循环结束之后释放100000000个字符串,如果使用的话,则会在每次循环结束时释放字符串,这样就可以减少内存峰值,提高程序的性能。<br>

{{< /admonition >}}

{{< admonition open=false type=question title="delloc方法会进行哪些操作？">}}

- 删除这个对象所有关联对象
- c++析构函数
- 删除引用计数表中obj对应的引用计数
- 将弱引用表中的弱引用指针置为nil

{{< /admonition >}}

### runtime

{{< admonition open=false type=question title="什么是Runtime，有什么作用？常用在什么地方">}}
定义：RunTime实际上是一个库，这个库使我们可以在程序运行时动态的创建对象、检查对象，修改类和对象的方法。他的作用其实就是在程序运行时做一些事情。<br>

**方法交换**<br>

```objc
Person *person = [[Person alloc] init];
Method m1 = class_getInstanceMethod(person.class, @selector(eat));
Method m2 = class_getInstanceMethod(person.class, @selector(run));
method_exchangeImplementations(m1, m2);
```

特别是交换系统自带的方法，可以在不改变原有代码的基础上，给系统自带的方法添加一些功能，比如给`ViewController`添加一个`swiviewDidLoad:`方法，替换系统方法并且不影响原系统方法<br>

```objc
#import <objc/runtime.h>

@interface ViewController ()

@end

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    NSLog(@"原方法");
}

@end

@implementation UIViewController (SwizzleViewDidLoad)

+ (void)load {
    static dispatch_once_t onceToken;
    dispatch_once(&onceToken, ^{
        Class class = [self class];
        SEL originalSelector = @selector(viewDidLoad);
        SEL swizzledSelector = @selector(swiviewDidLoad);
        
        Method originalMethod = class_getInstanceMethod(class, originalSelector);
        Method swizzledMethod = class_getInstanceMethod(class, swizzledSelector);
        
        BOOL didAddMethod = class_addMethod(class, swizzledSelector, method_getImplementation(originalMethod), method_getTypeEncoding(originalMethod));
        
        if (didAddMethod) {
            class_replaceMethod(class, swizzledSelector, method_getImplementation(originalMethod), method_getTypeEncoding(originalMethod));
        } else {
            method_exchangeImplementations(originalMethod, swizzledMethod);
        }
    });
}

- (void)swiviewDidLoad {
    NSLog(@"替换的方法,%@",[self class]);
    [self swiviewDidLoad];
}

@end
```

**动态添加方法**<br>

```objc
- (void)eat {
    NSLog(@"eat");
}

// 这里利用动态方法解析，给Person类添加一个run方法
+ (BOOL)resolveInstanceMethod:(SEL)sel {
    if (sel == @selector(eat)) {
        class_addMethod(self, sel, class_getMethodImplementation(self, @selector(run)), "v@:");
    }
    return [super resolveInstanceMethod:sel];
}

- (void)run {
    NSLog(@"run");
}
```

**动态添加属性**<br>

```objc
// 这里利用关联对象，给类添加一个name属性
- (void)setName:(NSString *)name {
    objc_setAssociatedObject(self, @selector(name), name, OBJC_ASSOCIATION_COPY_NONATOMIC);
}

- (NSString *)name {
    return objc_getAssociatedObject(self, _cmd);
}
```

**字典转模型**<br>

```objc
+ (instancetype)modelWithDict:(NSDictionary *)dict {
    id objc = [[self alloc] init];
    unsigned int count = 0;
    Ivar *ivarList = class_copyIvarList(self, &count);
    for (int i = 0; i < count; i++) {
        Ivar ivar = ivarList[i];
        NSString *ivarName = [NSString stringWithUTF8String:ivar_getName(ivar)];
        NSString *key = [ivarName substringFromIndex:1];
        id value = dict[key];
        if (value) {
            [objc setValue:value forKey:key];
        }
    }
    return objc;
}
```

**常用的方法**<br>

|方法|说明|
| :---: | :---: |
|objc_msgSend|向一个对象发送消息|
|class_getName|获取类名|
|class_getSuperclass|获取父类|
|class_getInstanceSize|获取实例大小|
|class_getInstanceVariable|获取实例变量|
|class_getClassVariable|获取类变量|
|class_getInstanceMethod|获取实例方法|
|class_getClassMethod|获取类方法|
|class_addMethod|添加方法|
|class_copyMethodList|获取方法列表|
|class_replaceMethod|替换方法|
|class_getMethodImplementation|获取方法实现|
|class_respondsToSelector|判断类是否实现了某个方法|
|objc_getAssociatedObject|获取关联对象|
|objc_setAssociatedObject|设置关联对象|
|objc_removeAssociatedObjects|移除关联对象|

{{< /admonition >}}

{{< admonition open=false type=question title="OC消息机制?">}}

消息机制分为两个部分：**消息传递** 和 **消息转发**<br>

**消息传递:**<br>

首先了解下OC中对象等的底层结构：<br>

```c++
//对象
struct objc_object {
    Class isa  OBJC_ISA_AVAILABILITY;
};
//类
struct objc_class {
    Class isa  OBJC_ISA_AVAILABILITY;
#if !__OBJC2__
    Class super_class                                        OBJC2_UNAVAILABLE;
    const char *name                                         OBJC2_UNAVAILABLE;
    long version                                             OBJC2_UNAVAILABLE;
    long info                                                OBJC2_UNAVAILABLE;
    long instance_size                                       OBJC2_UNAVAILABLE;
    struct objc_ivar_list *ivars                             OBJC2_UNAVAILABLE;
    struct objc_method_list **methodLists                    OBJC2_UNAVAILABLE;
    struct objc_cache *cache                                 OBJC2_UNAVAILABLE;
    struct objc_protocol_list *protocols                     OBJC2_UNAVAILABLE;
#endif
} OBJC2_UNAVAILABLE;
//方法列表
struct objc_method_list {
    struct objc_method_list *obsolete                        OBJC2_UNAVAILABLE;
    int method_count                                         OBJC2_UNAVAILABLE;
#ifdef __LP64__
    int space                                                OBJC2_UNAVAILABLE;
#endif
    /* variable length structure */
    struct objc_method method_list[1]                        OBJC2_UNAVAILABLE;
}                                                            OBJC2_UNAVAILABLE;
//方法
struct objc_method {
    SEL method_name                                          OBJC2_UNAVAILABLE;
    char *method_types                                       OBJC2_UNAVAILABLE;
    IMP method_imp                                           OBJC2_UNAVAILABLE;
}
```

OC中的方法调用其实都是转换成了objc_msgSend函数的调用，objc_msgSend函数的定义如下：<br>

```objc
id objc_msgSend(id self, SEL op, ...)
```

objc_msgSend函数的作用是向一个对象发送消息，它会根据对象的isa指针找到对象对应的类，查找会先从缓存中查找，如果缓存中没有，然后在类中的方法列表中查找方法，如果找到就调用，如果没有找到就从父类中查找，一直找到NSObject类，如果还是没有找到，就会调用`doesNotRecognizeSelector:`抛出异常。<br>

**消息转发:**<br>

消息转发是指当一个对象无法响应某个消息时，会把这个消息转发给另一个对象来处理。消息转发一般分为三个步骤：<br>

- 动态方法解析:<br>

当一个对象无法响应某个消息时，首先会调用`+resolveInstanceMethod:`或者`+resolveClassMethod:`方法，让我们有机会提供一个函数实现。如果能添加函数，就调用`objc_msgSend`重新发送消息，如果还是没添加成功，就会进入下一步，备用接收者。<br>

- 备用接收者:<br>

当一个对象无法响应某个消息时，会调用`- (id)forwardingTargetForSelector:(SEL)aSelector`方法，让我们指定一个备用对象来响应这个消息，如果返回的不是nil或者self，就会调用`objc_msgSend`重新发送消息，如果还是没响应，就会进入下一步，完整转发。<br>

- 完整转发:<br>

当一个对象无法响应某个消息时，会调用`- (void)forwardInvocation:(NSInvocation *)anInvocation`方法，让我们把这个消息转发给其他对象处理，如果我们不处理，就会调用`doesNotRecognizeSelector:`抛出异常。<br>

<center>
{{<image src="https://cdn.jsdelivr.net/gh/andy90s/blog-image/blog/images/message_forward.png" title="消息转发" width="95%">}}
<div style="color:#717171;font-size:14px;font-weight:normal"> <b> 消息转发 </b>  </div>
</center>
{{< /admonition >}}

{{< admonition open=false type=question title="synthesize和dynamic分别有什么作用？">}}

**synthesize:**<br>

@synthesize是编译器指令，告诉编译器自动生成getter和setter方法的实现，如果没有指定实例变量的名字，编译器会自动生成一个名为`_变量名`的实例变量。<br>

**dynamic:**<br>

@dynamic是编译器指令，告诉编译器不自动生成getter和setter方法的实现，而是由用户自己实现，如果没有指定实例变量的名字，编译器不会自动生成实例变量。<br>

**区别:**<br>

假如一个属性被声明为 @dynamic var，然后你没有提供 @setter方法和 @getter 方法，编译的时候没问题，但是当程序运行到 instance.var = someVar，由于缺 setter 方法会导致程序崩溃；或者当运行到 someVar = var 时，由于缺 getter 方法同样会导致崩溃.<br>

动态绑定:编译时没有问题，运行时才执行相应的方法。<br>

{{< /admonition >}}

{{< admonition open=false type=question title="在有了自动合成属性实例变量之后，@synthesize还有哪些使用场景？">}}
回答这个问题前，我们要搞清楚一个问题，什么情况下不会autosynthesis（自动合成）？<br>

- 同时重写了setter和getter方法
- 重写了只读属性的getter方法
- 使用了@dynamic
- 在protocol中定义的所有属性
- 编译器不会为带有`__attribute__((NSObject))`属性的属性合成实例变量
- 在category中定义的所有属性
- 重载的属性

```objc
@interface ViewController ()

@property (nonatomic, copy) NSString *name;

@end

@implementation ViewController {
    NSString *_name; // 这里创建了一个实例变量
}

@synthesize name = _name; // 这里手动绑定了实例变量

- (void)setName:(NSString *)name {
    _name = name;
}

- (NSString *)name {
    return _name;
}

@end
```
这里同时重写了setter 和 getter 方法, 需要手动绑定ivar 或者 自己创建ivar<br>

{{< /admonition >}}

### runloop

{{< admonition open=false type=question title="什么是runloop？">}}

**定义:**<br>

Runloop是一个对象，这个对象管理了其需要处理的事件和消息，并提供了一个入口函数来执行Event Loop的逻辑。线程执行了这个函数后，就会一直处于这个函数内部 “接受消息->等待->处理” 的循环中，直到这个循环结束（比如传入quit的消息），函数返回。<br>

EventLoop<br>

没有消息需要被处理时, 系统会将当前线程所有权转化为内核态, 当有消息需要处理时, 系统会将当前线程的状态切换回用户态.<br>

所以RunLoop的循环并不是一个单纯的死循环, 而是通过状态切换, 达到没有消息时休眠, 有消息时唤醒的这样一个事件循环机制.<br>

**作用:**<br>

保持程序的持续运行，处理App中的各种事件（比如触摸事件、定时器事件、Selector事件、Source事件），同时也负责调用开发者提供的代码。<br>

**结构:**<br>

在 OC 中实际提供了两个 RunLoop 的。<br>
一个是 NSRunLoop，一个是 CFRunLoop。<br>
NSRunLoop 是对 CFRunLoop 的封装，提供了一些面向对象的 API。<br>
NSRunLoop 是位于 Foundation 当中的，CFRunLoop 位于 CoreFoundation 当中的。<br>

runloop的数据结构主要有三个:<br>

- CFRunLoop
- CFRunLoopMode
- Source/Timer/Observer

`CFRunLoop:`<br>

```c++
//源码
struct __CFRunLoop {
  pthread_t _pthread;            // runloop所在线程,一一对应(RunLoop和线程的关系)
  CFMutableSetRef _commonModes;     // 存放的是NSRunLoopCommonModes表示的mode,我们也可以添加自定义mode到这个set里面
  CFMutableSetRef _commonModeItems; // 存放的是添加到NSRunLoopCommonModes表示的mode里面的item(source/timer/observer)
  CFRunLoopModeRef _currentMode;    // Current Runloop Mode
  CFMutableSetRef _modes;           // 改runloop包含的mode
  ...
};
```

由此可以看出，CFRunLoop是一个结构体，里面含有很多属性。看一下这个结构体里面我们需要关注的几个参数。每一个RunLoop都有自己的模式（Mode），而且不止一个模式。模式（Mode）里面存储的是RunLoop要处理的事件源，事件源有三种，Source、Timer、Observer这三种，下面会有详细介绍。RunLoop有很多模式，但是某一个时刻只能有一个确定的Mode，就是_currentMode，下面第二条讲述的就是RunLoop 的Mode，在RunLoop结构体里面几个与模式（Mode）相关的参数 ：<br>

_currentMode,，表示该RunLoop当前所处的模式；<br>
_modes表示该RunLoop中所有的模式；<br>
另外RunLoop里面有一个Mode是NSRunLoopCommonModes，这个Mode并没有什么含义，它只是对几个模式（Mode）进行标记的一个集合；<br>
_commonModes表示NSRunLoopCommonModes这个模式下保存的Mode，我们也可以将自定义的Mode添加到这个set里面；<br>
_commonModeItem表示添加到NSRunLoopCommonModes里面的Source/Timer等；<br>

`CFRunLoopMode:`<br>

```c++
struct __CFRunLoopMode {
    CFStringRef _name;            // Mode Name, 例如 @"kCFRunLoopDefaultMode"
    CFMutableSetRef _sources0;    // Set  
    CFMutableSetRef _sources1;    // Set 
    CFMutableArrayRef _observers; // Array
    CFMutableArrayRef _timers;    // Array
    ...
};
```

`Source/Timer/Observer:`<br>

CFRunLoopSource<br>

在 CF 框架当中官方名称叫 CFRunLoopSource ，有两种 source0 和 source1<br>
唤醒线程就是从内核态切换到用户态<br>

- Source0:非基于Port的，用于用户主动唤醒RunLoop，例如：performSelector:onThread:方法就是通过向对应线程的RunLoop的Source0发送消息来唤醒对应线程的RunLoop的。只包含了一个回调（函数指针），它并不能主动触发事件。使用时，你需要先调用 CFRunLoopSourceSignal(source)，将这个 Source 标记为待处理，然后手动调用 CFRunLoopWakeUp(runloop) 来唤醒 RunLoop，让其处理这个事件。
- Source1:基于Port的，用于系统内核事件的接收，例如：点击事件、触摸事件、Selector事件、Source事件等。包含了一个 mach_port和一个回调（函数指针），被用于通过内核和其他线程相互发送消息。这种 Source 能主动唤醒 RunLoop 的线程

CFRunLoopTimer<br>

CFRunLoopTimer 是基于时间的触发器，它和 NSTimer 是toll-free bridged 的，可以混用。其包含了一个时间长度和一个回调（函数指针），当其加入到 RunLoop 时，RunLoop会注册对应的时间点，当时间点到时，RunLoop会被唤醒以执行那个回调。<br>

CFRunLoopObserver<br>

观察者，能够监听RunLoop的状态改变，比如即将进入Loop、即将处理 Timer、即将处理 Source0、即将处理 Source1、即将休眠等。<br>

runloop在运行过程中有以下几种状态：<br>

```c++
typedef CF_OPTIONS(CFOptionFlags, CFRunLoopActivity) {
    kCFRunLoopEntry = (1UL << 0), // 即将进入Loop：1
    kCFRunLoopBeforeTimers = (1UL << 1), // 即将处理 Timer：2
    kCFRunLoopBeforeSources = (1UL << 2), // 即将处理 Source：4
    kCFRunLoopBeforeWaiting = (1UL << 5), // 即将进入休眠：32
    kCFRunLoopAfterWaiting = (1UL << 6), // 刚从休眠中唤醒：64
    kCFRunLoopExit = (1UL << 7), // 即将退出Loop：128
    kCFRunLoopAllActivities = 0x0FFFFFFFU // 监听全部状态改变
};
```
可以给一个RunLoop添加观察。通过监测RunLoop的状态判断是否出现卡顿。创建一个Observer观察者，将创建好的观察者添加到主线程RunLoop的CommonMode模式下观察，创建一个持续的子线程专门用来监控主线程的RunLoop状态，一旦发现进入睡眠前的KCFRunLoopBeforeSource状态，或者唤醒后的状态KCFRunLoopAfterWaiting，在设置的时间阈值内一直没有变化，即可判断为卡顿，dump出堆栈的信息，从而进一步分析出具体是哪个方法的执行时间长。

{{< /admonition >}}

{{< admonition open=false type=question title="runLoop中的Mode">}}

- kCFRunLoopDefaultMode:App的默认Mode，通常主线程是在这个Mode下运行的。
- UITrackingRunLoopMode:界面跟踪Mode，用于ScrollView追踪触摸滑动，保证界面滑动时不受其他Mode影响。
- UIInitializationRunLoopMode:在刚启动App时第进入的第一个Mode，启动完成后就不再使用。
- GSEventReceiveRunLoopMode:接受系统事件的内部Mode，通常用不到。
- kCFRunLoopCommonModes:这是一个占位的Mode，没有实际作用。

RunLoop启动的时候只能选择其中一个Mode，作为currentMode，如果需要切换Mode，只能退出当前Mode，再重新选择一个Mode进入。<br>
到这里，基于以上CFRunLoop和CFRunLoopMode的理解，RunLoop中保存的是RunLoopMode，而RunLoopMode中保存的才是实际执行的任务。<br>

{{< /admonition >}}

### 多线程

{{< admonition open=false type=question title="iOS中常见的多线程方案?">}}

|方案|简介|语言|线程生命周期|使用频率|
| :---: | :---: | :---: | :---: | :---: |
|pthread|一套通用的多线程api,适用Unix\Linux\Windows等系统,跨平台,可移植,但是使用难度大|C|需要自己管理|低|
|NSThread|面向对象的多线程api|OC|需要自己管理|低|
|GCD|基于C语言的一套多线程api,替代NSThread等线程技术,充分利用系统的多核|C|自动管理|高|
|NSOperation|基于GCD的面向对象的多线程api|OC|自动管理|高|

{{< /admonition >}}

{{< admonition open=false type=question title="GCD并行 串行, 同步 异步?">}}

**并行和串行:任务的执行方式**<br>

并发:多个任务同时执行<br>
串行:一个任务执行完毕后,再执行下一个任务<br>

**同步和异步的主要区别:能不能开启新的线程**<br>

同步:在当前线程中执行任务,不具备开启新线程的能力<br>
异步:在新的线程中执行任务,具备开启新线程的能力<br>

各种队列的执行情况:<br>

||并发队列|串行队列|主队列|
| :---: | :---: | :---: | :---: |
|同步(sync)|没有开启新线程,串行执行任务|没有开启新线程,串行执行任务|主线程调用:死锁卡住不执行,其他线程调用:没有开启新线程,串行执行任务|
|异步(async)|有开启新线程,并发执行任务|有开启新线程,串行执行任务|没有开启新线程,串行执行任务|

使用sync函数往**当前串行队列**中添加任务,会卡住当前的串行队列,导致后面的任务无法执行,造成死锁.<br>

主队列是GCD自带的一种特殊的串行队列,放在主队列中的任务,都会放到主线程中执行.<br>

```objc
// 主队列使用sync函数添加任务,会卡住主线程,导致后面的任务无法执行,造成死锁
dispatch_sync(dispatch_get_main_queue(), ^{
    NSLog(@"同步执行任务");
});
// 主队列比较特殊实用async函数添加任务,不会开启新线程,任务串行执行
dispatch_async(dispatch_get_main_queue(), ^{
    NSLog(@"异步执行任务");
});
```

{{< /admonition >}}

{{< admonition open=false type=question title="异步并发执行任务1、任务2，等任务1、任务2都执行完毕后，再回到主线程执行任务3怎么实现？">}}
```objc
// 创建一个队列组
dispatch_group_t group = dispatch_group_create();
// 创建一个并发队列
dispatch_queue_t queue = dispatch_queue_create("com.andy90s", DISPATCH_QUEUE_CONCURRENT);
// 将任务1添加到队列组中
dispatch_group_async(group, queue, ^{
    NSLog(@"任务1");
});
// 将任务2添加到队列组中
dispatch_group_async(group, queue, ^{
    NSLog(@"任务2");
});
// 将队列组中的任务都执行完毕后,回到主线程执行任务3
dispatch_group_notify(group, dispatch_get_main_queue(), ^{
    NSLog(@"任务3");
});
```
{{< /admonition >}}



{{< admonition open=false type=question title="自旋锁和互斥锁的区别？递归锁，条件锁是什么？">}}
**自旋锁:**<br>

自旋锁是指当一个线程尝试获取某个锁时，如果该锁已被其他线程占用，就一直循环检测锁是否被释放，而不是进入线程挂起或睡眠状态,也就是忙等。因为不会引起调用者睡眠,所以效率高于互斥锁。<br>

**互斥锁:**<br>

互斥锁是一种常用的线程同步机制，它保证了在任何时刻，只有一个线程访问某个特定的数据。互斥锁是一种“悲观锁”，它假设最坏的情况，即每次访问数据时都会发生冲突。因此，它每次都会进行加锁和解锁操作，这样就会带来一些额外的开销。<br>

**递归锁:**<br>

递归锁是一种特殊的互斥锁，它可以被同一个线程多次获取。如果使用普通的互斥锁，当一个线程试图对一个已经由它自己持有的互斥锁加锁时，这种情况称为死锁。而递归锁则允许线程对由它自己持有的互斥锁再次加锁，这种情况下，会将该锁的计数器加1。只有所有的加锁操作全部完成后，才能进行解锁。<br>

**条件锁:**<br>

条件锁是一种特殊的互斥锁，它允许线程在满足特定条件时才进行加锁。条件锁需要和条件变量配合使用。条件变量是一种线程间的通信机制，它可以使线程在满足特定条件时才进行加锁。条件变量可以和互斥锁配合使用，也可以单独使用。<br>

{{< /admonition >}}

{{< admonition open=false type=question title="iOS中的锁有哪些?">}}

- @synchronized
- NSLock
- NSRecursiveLock
- NSCondition
- NSConditionLock
- pthread_mutex
- dispatch_semaphore
- OSSpinLock(iOS10后已废弃)
- os_unfair_lock

{{< /admonition >}}

### UI

{{< admonition open=false type=question title="事件响应链是如何传递的?如何扩大按钮响应范围?">}}

**事件传递的过程:**<br>

- 当用户触摸屏幕时，系统会将触摸事件加入到UIApplication管理的事件队列中。
- UIApplication会从事件队列中取出事件，然后通过UIWindow的hitTest:withEvent:方法找到最合适的view。
- 然后通过view的pointInside:withEvent:方法判断触摸点是否在view上。
- 如果在view上，就会调用view的touchesBegan:withEvent:方法。
- 如果不在view上，就会调用view的hitTest:withEvent:方法找到最合适的子view，然后重复上面的步骤。
- 如果没有找到合适的view，事件就会被丢弃。

**扩大按钮响应范围:**<br>

重写UIButton的 `pointInside:withEvent:` 方法，扩大按钮的响应范围。<br>

```objc
- (BOOL)pointInside:(CGPoint)point withEvent:(UIEvent *)event {
    CGRect bounds = self.bounds;
    bounds = CGRectInset(bounds, -20, -20);
    return CGRectContainsPoint(bounds, point);
}
```

{{< /admonition >}}

{{< admonition open=false type=question title="什么是异步渲染?">}}

异步渲染是指在子线程中进行绘制，然后将绘制好的内容显示到屏幕上。异步渲染可以提高界面的流畅度，避免主线程阻塞。<br>

{{< /admonition >}}

{{< admonition open=false type=question title="离屏渲染是什么？怎么避免离屏渲染？">}}

**离屏渲染:**<br>

离屏渲染是指在当前屏幕之外新开辟一个缓冲区进行绘制，然后再将绘制好的内容显示到屏幕上。离屏渲染会消耗更多的内存和CPU资源，所以要尽量避免。<br>

**避免离屏渲染:**<br>

- shadow 
如果需要阴影效果,使用shadowPath属性
- 圆角
cornerRadius+clipsToBounds, 避免同时使用backgroundColor和layer的content层
- group opacity
透明度是由父视图和子视图的透明度共同决定的,如果父视图和子视图的透明度都小于1,就会触发离屏渲染
- mask
和group opacity类似

{{<link href="https://zhuanlan.zhihu.com/p/72653360" content="【关于iOS离屏渲染的深入研究】">}}

{{< /admonition >}}

{{< admonition open=false type=question title="layoutSubviews是在什么时机调用的?">}}

- init初始化不会触发layoutSubviews
- addSubView会触发layoutSubviews
- 设置view的frame会触发layoutSubviews
- 滚动一个UIScrollView会触发layoutSubviews
- 旋转Screen会触发父UIView上的layoutSubviews事件
- 改变一个UIView大小的时候也会触发父UIView上的layoutSubviews事件

改变子视图的大小，会触发父视图的layoutSubviews方法，改变父视图的大小，也会触发父视图的layoutSubviews方法。<br>

{{< /admonition >}}

{{< admonition open=false type=question title="从磁盘一张图片的展示经历了哪些步骤?">}}

**1.加载图片**

- 从磁盘中加载一张图片
- 然后将生成的UIImage对象赋值给UIImageView的image属性
- 接着一个隐式的CATransaction会被创建，然后在下一个runloop周期中，这个CATransaction会被提交，从而触发一次图层树的重新布局和显示
- 分配内存缓冲区用于管理文件IO和解压缩操作,将文件数据读取到内存中

**2.解码图片**

- 将压缩的图片数据解码成位图数据,这是一个非常耗时操作默认是在主线程中进行的

**3.图片渲染**

- Core Animation 会将解压后的位图数据转换成GPU纹理,然后将纹理上传到GPU中进行渲染

{{<link href="https://juejin.cn/post/6844904115995148295" content="【iOS开发图片格式选择】">}}

{{< /admonition >}}

### 性能优化

{{< admonition open=false type=question title="对tableview进行性能优化的方式有哪些?">}}

- cell的重用
- cell的高度缓存
- 异步绘制
- 避免动态添加图层 善用hidden
- 避免离屏渲染
- 预加载

{{< /admonition >}}

{{< admonition open=false type=question title="Xcode的Instruments都有哪些常用调试工具?">}}

- Allocations:检测内存泄漏
- Leaks:检测内存泄漏
- Time Profiler:检测CPU使用情况
- Zombies:检测野指针
- File Activity:检测文件读写情况
- Network:检测网络请求情况
- Core Animation:检测界面渲染情况
- Energy Log:检测电量消耗情况
- Metal System Trace:检测GPU使用情况

{{< /admonition >}}

{{< admonition open=false type=question title="造成卡顿的原因❓如何检测卡顿,都有哪些方法❓">}}

**造成卡顿的原因:**<br>

- 复杂的UI 图文混排的制作量过大
- 大量离屏渲染
- 主线程上大量IO操作
- 主线程上大量计算操作
- 死锁和主线程抢锁 优先级反转

**线下解决方案?:**<br>
- 利用CADisplayLink检测帧率
- 利用RunLoop监控主线程卡顿
- 线下使用instrument的Time Profiler检测CPU使用情况

**线上如何用runloop监控?**

监听主线程的runloop的状态来判别是否会出现卡顿,大概思路如下:<br>

- 创建一个子线程,在子线程中创建一个runloop,并且添加一个CFRunLoopObserverContext观察者
- 每隔1秒检查主线程runloop中的状态,如果发现主线程runloop的状态在kCFRunLoopBeforeSources和kCFRunLoopBeforeWaiting之间停留超过一定时间,就认为主线程卡顿
- 获取堆栈信息,然后在主线程runloop睡眠的时候,添加上报任务

```swift
class RunloopChecker {
    static let shared = RunloopChecker()
    private var runloopThread: Thread?
    private var isMonitoring = false
    private var timeoutCount = 0
    private var runloopObserver: CFRunLoopObserver?
    
    private init() {
        
    }
    
    func start() {
        if isMonitoring {
            return
        }
        isMonitoring = true
        runloopThread = Thread(target: self, selector: #selector(runloopThreadEntryPoint), object: nil)
        runloopThread?.start()
    }
    
    func stop() {
        if !isMonitoring {
            return
        }
        isMonitoring = false
        runloopThread?.cancel()
        runloopThread = nil
    }
    
    @objc private func runloopThreadEntryPoint() {
        autoreleasepool {
            let runloop = CFRunLoopGetCurrent()
            let observer = CFRunLoopObserverCreateWithHandler(kCFAllocatorDefault, CFRunLoopActivity.allActivities.rawValue, true, 0, { (observer, activity) in
                switch activity {
                case CFRunLoopActivity.entry:
                    break
                case CFRunLoopActivity.beforeTimers:
                    break
                case CFRunLoopActivity.beforeSources:
                    self.timeoutCount = 0
                case CFRunLoopActivity.beforeWaiting:
                    break
                case CFRunLoopActivity.afterWaiting:
                    break
                case CFRunLoopActivity.exit:
                    break
                default:
                    break
                }
            })
            CFRunLoopAddObserver(RunLoop.main.getCFRunLoop(), observer, .commonModes)
            
            let timer = Timer(timeInterval: 1, target: self, selector: #selector(timerFired), userInfo: nil, repeats: true)
            RunLoop.current.add(timer, forMode: .common)
            RunLoop.current.run()
        }
    }
    
    @objc private func timerFired() {
        if timeoutCount > 3 {
            print("主线程卡顿")
        }
        timeoutCount += 1
    }
}

```

{{< /admonition >}}

{{< admonition open=false type=question title="iOS启动优化的二进制重排的核心依据是什么">}}

app启动时候,会调用各种函数,由于这些函数分布在各个TEXT段且不连续,此时需要`page fault`创建分页,导致启动时间变长,二进制重排的核心依据是将这些函数按照调用顺序进行重排,使得这些函数在内存中连续存放,减少`page fault`的发生,从而提高启动速度.

{{< /admonition >}}

{{< admonition open=false type=question title="你是如何排查内存泄漏问题的?">}}

借助三方工具MLLeaksFinder,FBRetainCycleDetector,Instruments等工具,检测内存泄漏

{{< /admonition >}}




















