# iOS面试题

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

{{< admonition open=false type=question title="OC中内存分区从高到低是怎么样的？">}}
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

{{< admonition open=false type=question title="内存管理的原则是什么？">}}
**内存管理的原则:**<br>
- 谁持有,谁释放
- 谁创建,谁释放
- 自己生成的对象,自己持有
- 非自己生成的对象,自己也能持有
- 在不需要使用某个对象时,及时释放
- 非自己持有的对象,无权释放
{{< /admonition >}}

{{< admonition open=false type=question title="讲下自动释放池">}}
Autorelease Pool 提供了一种可以允许你向一个对象延迟发送release消息的机制,当你想要释放一个对象时,只需要将这个对象放入到Autorelease Pool中,系统会在Autorelease Pool被销毁时,向池中所有的对象发送release消息。<br>

所谓的延迟发送release消息指的是，当我们把一个对象标记为autorelease时:<br>
```objc
NSString* str = [[[NSString alloc] initWithString:@"hello"] autorelease];
```
这个对象的 retainCount 会 +1,但是并不会立即release,而是当这段语句所处的autoreleasepool被销毁时,所有标记了autorelease的对象才会被release。<br>

**Autorelease Pool的用处:**
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



















