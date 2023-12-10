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










