# iOS面试题-Swift

<!--more-->

### Swift数据类型，常量、变量、元组

{{< admonition open=false type=question title="值类型和引用类型区别，swift中值类型有哪些，引用类型有哪些。和OC相比有什么区别">}}

**定义值类型和引用类型**<br>

- **值类型** 存储在栈区。 每个值类型变量都有其自己的数据副本，并且对一个变量的操作不会影响另一个变量。值类型包括：struct，enum，tuple。
- **引用类型** 存储在其他位置（堆区），我们在内存中有一个指向该位置的引用。 引用类型的变量可以指向相同类型的数据。 因此，对一个变量进行的操作会影响另一变量所指向的数据。引用类型包括：class，closure，function。

**和OC相比有什么区别**<br>

- OC 中仅有基本数据类型、基础 struct 是值类型。
- Swift 中的 struct、enum、tuple 都是值类型。(Array、Dictionary、String 都是struct)

{{< /admonition >}}

{{< admonition open=false type=question title="Optional可选类型属于引用类型还是值类型？如何实现的?">}}

Optional 是枚举类型，它有两个值：None 和 Some(T)。None 表示没有值，Some(T) 表示包含一个 T 类型的值。Optional 是值类型，它的本质是一个枚举类型，它的值是枚举类型的一个 case，所以它是值类型。

**Optional 的本质**<br>

```swift
enum Optional<T> {
    case none
    case some(T)
}
```

{{< /admonition >}}

{{< admonition open=false type=question title="可选类型解包方式有哪些?">}}

- **强制解包**：在变量名后面加一个感叹号，强制解包，如果可选类型为 nil，强制解包会导致运行时错误。
- **可选绑定**：使用 if let 或者 guard let 语句进行可选绑定，判断可选类型是否包含值，如果包含就把值赋给一个临时的常量或者变量，用于后续语句中使用。

{{< /admonition >}}

{{< admonition open=false type=question title="Swift中的元组有什么特点？">}}

- 元组是一种数据结构，可以用于存储多个值，可以是不同类型的值。
- 元组中的值可以是任意类型，不要求是相同类型。
- 元组中的值可以通过名字或者数字索引来获取。
- 元组中的值可以是可选类型，也可以是非可选类型。

{{< /admonition >}}

{{< admonition open=false type=question title="什么是字面量，字面量协议可以做什么？">}}

**字面量类型(Literal Type)**就是支持通过字面量进行实例初始化的数据类型,比如整型字面量、浮点型字面量、布尔型字面量、字符串字面量、数组字面量、字典字面量、空字面量等。

```swift
let a = 10
let b = "hello"
let c = [1, 2, 3]
```
以上代码中, 10、"hello"、[1, 2, 3] 就是字面量,用于表达源代码中一个固定值的表示法（notation）。<br>

**字面量协议(Literal Protocol)**就是用来约束字面量类型的协议,Swift 包含一系列ExpressibleByLiteral协议，用于使用匹配的文字初始化自定义类型.

可以利用字面量协议来实现自定义类型的字面量初始化,比如自定义一个结构体,实现字面量协议,就可以使用字面量来初始化该结构体.<br>

```swift
struct MyStruct: ExpressibleByIntegerLiteral {
    var value: Int
    init(integerLiteral value: Int) {
        self.value = value
    }
}
let a: MyStruct = 10
```

{{< /admonition >}}

### Swift流程控制

{{< admonition open=false type=question title="Swift中有哪些流程控制语句？">}}

- **if 语句**：用于基于特定条件选择性执行代码。
- **guard 语句**：用于基于特定条件选择性执行代码，guard 语句总是有一个 else 分句，如果条件不满足则执行 else 分句中的代码。
- **switch 语句**：用于基于多个条件选择性执行代码。
- **for-in 语句**：用于重复执行一系列代码。
- **while 语句**：用于重复执行一系列代码直到条件不满足时停止执行。
- **repeat-while 语句**：用于重复执行一系列代码直到条件不满足时停止执行，和 while 语句的区别是 repeat-while 语句会先执行一次代码块。

{{< /admonition >}}

{{< admonition open=false type=question title="for in 在Swift上有什么特点？">}}

- for in 循环遍历一个区间范围内的值，比如数字范围、字符串中的字符、数组中的元素等。
- for in 循环遍历一个集合中的值，比如数组、字典、集合等。
- for in 循环遍历一个序列中的值，比如数组、字典、集合等。

{{< /admonition >}}

{{< admonition open=false type=question title="什么是区间类型？跨间隔的区间怎么实现？">}}

**区间类型(Range Type)**就是用来表示一个范围的类型,Swift 中的区间类型包括:闭区间、半开区间、单侧区间、单侧半开区间等.<br>

```swift
// 闭区间
let closedRange = 0...5
// 半开区间
let halfOpenRange = 0..<5
// 单侧区间
let singleSideRange = 0...
// 单侧半开区间
let singleSideHalfOpenRange = ..<5
```

**跨间隔的区间**<br>

```swift
// 利用stride, stride(from:to:by:) 从起始值到结束值,每次增加固定的值,直到结束值,但不包括结束值
let strideRange = stride(from: 0, to: 10, by: 2)
```

{{< /admonition >}}

{{< admonition open=false type=question title="Swift中switch怎么实现贯穿效果？">}}

Swift 中的 switch 语句默认不会贯穿，如果需要贯穿，需要在 case 分支中使用 fallthrough 关键字。

{{< /admonition >}}

{{< admonition open=false type=question title="switch与元组结合有什么效果？元祖与where结合呢？">}}

**switch 与元组结合**<br>

```swift
let point = (1, 1)
switch point {
case (0, 0):
    print("原点")
case (_, 0):
    print("x轴")
case (0, _):
    print("y轴")
case (-2...2, -2...2):
    print("在矩形内")
default:
    print("在矩形外")
}
```

**元组与 where 结合**<br>

```swift
let point = (1, 1)
switch point {
case let (x, y) where x == y:
    print("x == y")
case let (x, y) where x == -y:
    print("x == -y")
case let (x, y):
    print("(\(x), \(y))")
}
```

{{< /admonition >}}

{{< admonition open=false type=question title="switch区间匹配？">}}

```swift
let score = 90
switch score {
case 0..<60:
    print("不及格")
case 60..<80:
    print("及格")
case 80..<90:
    print("良好")
case 90...100:
    print("优秀")
default:
    print("成绩有误")
}
```

{{< /admonition >}}

{{< admonition open=false type=question title="guard..else与do..while有什么区别">}}

- guard..else 语句是用来检查条件是否成立，如果不成立就执行 else 分支中的代码，else 分支中的代码必须包含控制转移语句，比如 return、break、continue、throw 等，用来退出当前代码块。
- do..while 语句是用来重复执行一系列代码，直到条件不满足时停止执行，do..while 语句会先执行一次代码块，然后判断条件是否成立，如果成立就继续执行，否则就停止执行。

{{< /admonition >}}

### Swift结构体，类，枚举

{{< admonition open=false type=question title="枚举是否可以递归？">}}

枚举是可以递归的，但是不能直接递归，需要在枚举名前面加上 indirect 关键字，表示这个枚举是可以递归的。

{{< /admonition >}}

{{< admonition open=false type=question title="枚举值原始值和附加值分别是什么？内存占用怎么计算？">}}

**枚举值原始值**<br>

枚举值原始值是指在定义枚举时指定的值，原始值可以是整型、浮点型、字符串型，原始值可以是隐式的，也可以是显式的。

```swift
enum Direction: Int {
    case north = 1
    case south = 2
    case east = 3
    case west = 4
}
```

**枚举值附加值(关联值)**<br>

枚举值附加值是指在使用枚举值时指定的值，附加值可以是任意类型，附加值可以是隐式的，也可以是显式的。

```swift
enum Score {
    case point(Int)
    case grade(Character)
}
```

**内存占用**<br>

枚举类型变量的内存占用可以解释为1个字节存储成员值(标明是哪个成员)，加上占用字节数最大的**关联值**的字节数。比如上面原始值类型占用1字节, 关联值类型占用 1 + 16 = 9 字节, 由于内存对齐, 所以占用 24 字节.

{{< /admonition >}}

{{< admonition open=false type=question title="结构体自定义初始化方法和自动生成的初始化方法有什么关系？">}}

没有区别:<br>

```swift
struct Point {
    var x: Int
    var y: Int
    init(x: Int, y: Int) {
        self.x = x
        self.y = y
    }
}
```

```swift
struct Point {
    var x: Int
    var y: Int
}
```

{{< /admonition >}}

{{< admonition open=false type=question title="结构体能否继承？如果改变property，需要怎么做？">}}

结构体不能继承，结构体是值类型，类是引用类型，结构体是通过复制来传递的，类是通过引用来传递的。<br>

如果需要改变结构体中的属性，需要在方法前面加上 mutating 关键字，表示该方法可以修改结构体中的属性。

{{< /admonition >}}

{{< admonition open=false type=question title="类自动生成的初始化方法与结构体自动初始化方法有何区别？">}}

类的定义和结构体类似，但是编译器并没有为类自动生成可以传入成员值的初始化器。会生成一个无参的初始化器，
类中有初始值的话无参初始化器才会有用。<br>

所有结构体都有一个编译器自动生成的初始化器。
{{< /admonition >}}

{{< admonition open=false type=question title="struct 与 class有什么区别？">}}

- 结构体是值类型，类是引用类型。
- 结构体不能继承，类可以继承。
- 结构体在传递的时候是复制，类在传递的时候是引用。

{{< /admonition >}}

### Swift函数,闭包

{{< admonition open=false type=question title="实例方法和类型方法有什么区别？">}}

- 实例方法是通过实例调用的方法，类型方法是通过类型调用的方法。
- 实例方法是可以访问实例属性和类型属性的，类型方法是可以访问类型属性和类型方法的。

{{< admonition tip "">}}
有点类似OC中的实例方法和类方法
{{< /admonition >}}
{{< /admonition >}}

{{< admonition open=false type=question title="什么是闭包？闭包表达式是怎么样的？">}}

**闭包**是自包含的函数代码块，可以在代码中被传递和使用。Swift 中的闭包与 C 和 Objective-C 中的代码块（blocks）以及其他一些编程语言中的 lambdas 函数比较相似。<br>

**闭包表达式**是一种构建内联闭包的方式，它的语法简洁。在保证不丢失语境的情况下，可以通过引用已经存在的变量或者函数来创建闭包表达式。<br>

```swift
// 闭包表达式语法
{ (parameters) -> returnType in
    statements
}
```

{{< /admonition >}}

{{< admonition open=false type=question title="autoclosure 是什么? 怎么用">}}

**autoclosure** 是一种自动创建闭包的技术，可以将一句表达式自动封装成一个闭包，然后将这个闭包作为参数传递给函数，这样就省去了写闭包的大量代码，提高了代码的简洁性。<br>

```swift
func logIfTrue(_ predicate: () -> Bool) {
    if predicate() {
        print("True")
    }
}

logIfTrue({ return 2 > 1 })

// 使用 autoclosure
func logIfTrue(_ predicate: @autoclosure () -> Bool) {
    if predicate() {
        print("True")
    }
}

logIfTrue(2 > 1)
```

{{< /admonition >}}

{{< admonition open=false type=question title="什么是逃逸闭包和非逃逸闭包?">}}

**逃逸闭包**是指在函数返回之后才执行的闭包，逃逸闭包需要在闭包前面加上 `@escaping` 关键字。<br>

**非逃逸闭包**是指在函数返回之前就执行的闭包，非逃逸闭包需要在闭包前面加上 `@noescape` 关键字，Swift 3.0 之后默认就是非逃逸闭包，所以 `@noescape` 关键字已经被废弃了。

{{< /admonition >}}

{{< admonition open=false type=question title="Swift中的函数参数有哪些？">}}

- **默认参数**：在定义函数时可以给某个参数指定一个默认值，调用函数时如果没有传入该参数，则使用默认值。
- **可变参数**：一个函数最多只能有一个可变参数，可变参数需要在参数类型后面加上 `...`，可变参数在函数内部是一个数组。
- **输入输出参数**：函数参数默认是常量，如果想要在函数中修改参数的值，并且想要修改函数外部传入的参数的值，可以将参数定义为输入输出参数，在参数类型前面加上 `inout` 关键字，输入输出参数不能有默认值，可变参数不能标记为输入输出参数。

{{< /admonition >}}

### Swift属性,单例

{{< admonition open=false type=question title="什么是计算属性，什么是存储属性？只读计算属性，延迟存储属性呢？">}}

**计算属性**是通过某种方式计算得到的属性，计算属性可以用于类、结构体和枚举中，计算属性不直接存储值，而是提供一个 getter 和一个可选的 setter，来间接获取和设置其他的属性和值。<br>

**存储属性**是存储在特定类或结构体的实例里的一个常量或变量，存储属性可以是变量存储属性，也可以是常量存储属性，存储属性只能用于类和结构体中。<br>

**只读计算属性**只有 getter 没有 setter 的计算属性就是只读计算属性，只读计算属性只能用于类、结构体和枚举中。<br>

**延迟存储属性**是指当第一次被调用的时候才会计算其初始值的属性，延迟存储属性必须声明成变量（使用 var 关键字），因为属性的初始值可能在实例构造完成之后才会得到，而常量属性在构造过程完成之前必须要有初始值，因此不能声明成延迟存储属性。

{{< /admonition >}}

{{< admonition open=false type=question title="Swift中的属性观察器有哪些？">}}

- **willSet**：在设置新的值之前调用。
- **didSet**：在新的值被设置之后立即调用。

{{< /admonition >}}

{{< admonition open=false type=question title="枚举的原始值属于计算属性还是存储属性？">}}

计算属性, 因为枚举的原始值是通过计算得到的, 而不是存储的.

{{< /admonition >}}

{{< admonition open=false type=question title="实例属性和类型属性有什么区别？">}}

- 实例属性是属于某个类、结构体或者枚举类型实例的属性，每次创建一个新的实例，实例都拥有一套属于自己的属性值，实例之间的属性相互独立。
- 类型属性是属于类型本身的属性，不属于某个实例，每个类型只有一份类型属性，不管创建多少个该类型的实例，这些实例都共享同一个类型属性。

{{< /admonition >}}

{{< admonition open=false type=question title="Swift中的单例怎么实现？">}}

```swift
class Singleton {
    static let shared = Singleton()
    private init() {}
}
```
{{< /admonition >}}

{{< admonition open=false type=question title="存储类型属性有什么特点? 在什么时候初始化？多个线程同时访问呢？">}}

**存储类型属性**是指在类型的作用域内定义的属性，用 `static` 关键字修饰的属性就是存储类型属性，存储类型属性可以是变量存储属性，也可以是常量存储属性，存储类型属性只能用于类和结构体中。<br>

**存储类型属性的特点**<br>

- 存储类型属性是延迟初始化的，只有在第一次被访问的时候才会初始化。
- 存储类型属性会被多个线程同时访问的时候，保证只会初始化一次，并且不需要使用 lazy 关键字。

{{< /admonition >}}

{{< admonition open=false type=question title="Swift中的类型属性有哪些？">}}

- **存储类型属性**：用 `static` 关键字修饰的属性就是存储类型属性，存储类型属性可以是变量存储属性，也可以是常量存储属性，存储类型属性只能用于类和结构体中。
- **计算类型属性**：用 `class` 关键字修饰的属性就是计算类型属性，计算类型属性只能用于类中。

{{< /admonition >}}

### Swift泛型

{{< admonition open=false type=question title="什么是泛型？泛型有什么作用？">}}

**泛型**是指在定义函数、结构体、类、枚举、方法时，不需要指定具体的类型，使用的时候再指定具体的类型，泛型可以提高代码的复用性，减少代码的重复编写。(类型参数化)
{{< /admonition >}}

{{< admonition open=false type=question title="Swift中的泛型有哪些？">}}

- **泛型函数**：在函数名后面使用尖括号定义泛型类型，泛型类型可以是多个，多个泛型类型之间用逗号分隔。
- **泛型类型**：在类型名后面使用尖括号定义泛型类型，泛型类型可以是多个，多个泛型类型之间用逗号分隔。
- **泛型下标**：在下标后面使用尖括号定义泛型类型，泛型类型可以是多个，多个泛型类型之间用逗号分隔。

{{< /admonition >}}

{{< admonition open=false type=question title="什么是关联类型？有什么作用？">}}

**关联类型**是指在协议中定义的一个占位符名称，用于指定协议中的某个类型，关联类型通过 associatedtype 关键字来指定。<br>

```swift
protocol Container {
    associatedtype ItemType
    mutating func append(_ item: ItemType)
    var count: Int { get }
    subscript(i: Int) -> ItemType { get }
}
```

**关联类型的作用**<br>

- 作为协议的一部分，为某个类型提供一个占位名（或者说别名），其实际类型在协议被实现时才会被指定。
- 关联类型可以在协议中被指定为类型的占位符，而不是一个具体的类型。

{{< /admonition >}}

{{< admonition open=false type=question title="什么是协议类型，协议类型能否作为函数返回值？">}}

**协议类型**是指遵循了某个协议的类型，协议类型可以作为函数、方法或构造器中的参数类型或返回值类型，或者作为常量、变量或属性的类型，协议类型可以在类型后面加上 `&` 符号组合在一起。<br>

```swift
protocol Runnable {
    func run()
}

class Person: Runnable {
    func run() {
        print("Person run")
    }
}

class Car: Runnable {
    func run() {
        print("Car run")
    }
}

func get(_ type: Int) -> Runnable {
    if type == 0 {
        return Person()
    } else {
        return Car()
    }
}

let runnable = get(0)
runnable.run()
```

{{< /admonition >}}

{{< admonition open=false type=question title="泛型类型如何约束？">}}

- **类型约束**：在定义泛型的时候，可以对泛型进行类型约束，只有满足约束的类型才能使用这个泛型。
- **关联类型约束**：在协议中使用关联类型，可以给关联类型添加类型约束，只有满足约束的类型才能实现这个协议。

```
// 类型约束, T 必须是 Equatable 类型
func swapTwoValues<T: Equatable>(_ a: inout T, _ b: inout T) {
    if a == b {
        return
    }
    (a, b) = (b, a)
}
```

{{< /admonition >}}

{{< admonition open=false type=question title="什么是不透明类型？">}}

**不透明类型**是指在函数、方法或者构造器中，返回值的类型可以是协议类型，但是不能是泛型类型，也不能是关联类型，只能是具体的类型。<br>

```swift
func get() -> some Runnable {
    return Person()
}
```

{{< /admonition >}}

### swift 运算符

{{< admonition open=false type=question title="什么是溢出运算符？">}}

**溢出运算符**是为那些会产生溢出的运算提供额外的操作符，Swift 提供了三个溢出运算符：溢出加法运算符（&+）、溢出减法运算符（&-）和溢出乘法运算符（&*）。

{{< /admonition >}}

{{< admonition open=false type=question title="什么是运算符重载？">}}

**运算符重载**是指在自定义的类、结构体、枚举中，可以对已有的运算符进行重新定义，赋予它们更加特殊的功能，以适应自定义类型的需求。

{{< /admonition >}}

{{< admonition open=false type=question title="Equatable协议与==运算符有什么关系？Swift为哪些类型提供默认的 Equatable 实现？">}}

**Equatable**协议是用来判断两个对象是否相等的协议，Swift 为以下类型提供默认的 Equatable 实现：比较两个整数或浮点数是否相等，比较两个字符串是否相等，比较两个布尔值是否相等，比较两个枚举值是否相等，比较两个元组是否相等。

{{< /admonition >}}

{{< admonition open=false type=question title="如何自定义新的运算符？">}}

**自定义运算符**可以对已有的运算符进行重新定义，赋予它们更加特殊的功能，以适应自定义类型的需求，自定义运算符可以使用 `operator` 关键字在全局作用域内进行定义，也可以使用 `infix`、`prefix`、`postfix` 关键字在局部作用域内进行定义。

```swift
// 定义全局运算符
prefix operator +++
prefix func +++(value: Int) -> Int {
    return value + 2
}

// 定义局部运算符
struct Point {
    var x: Int
    var y: Int
}

extension Point {
    static prefix func -(point: Point) -> Point {
        return Point(x: -point.x, y: -point.y)
    }
}

let point = Point(x: 10, y: 20)
let newPoint = -point
print(newPoint)
```

{{< /admonition >}}

### swift 初始化器

{{< admonition open=false type=question title="指定初始化器和便捷初始化器有什么区别">}}

- **指定初始化器**是类中最主要的初始化器，一个类至少有一个指定初始化器，指定初始化器必须调用其直接父类的指定初始化器。
- **便捷初始化器**是类中比较次要的初始化器，可以定义便捷初始化器来调用同一个类中的指定初始化器，并为其参数提供默认值，也可以定义便捷初始化器来调用同一个类中的便捷初始化器。

{{< /admonition >}}

{{< admonition open=false type=question title="重写父类指定初始化器和便捷初始化器有何区别？">}}

```swift
class Person {
    var age: Int
    init(age: Int) {
        self.age = age
    }
    convenience init() {
        self.init(age: 0)
    }
}

class Student: Person {
    var score: Int
    init(age: Int, score: Int) {
        self.score = score
        super.init(age: age)
    }
    override convenience init(age: Int) {
        self.init(age: age, score: 0)
    }
}
```

- **重写父类指定初始化器**：子类可以通过重写父类的指定初始化器来实现父类的初始化器，子类的指定初始化器必须调用父类的指定初始化器。
- **重写父类便捷初始化器**：子类可以通过重写父类的便捷初始化器来实现父类的初始化器，子类的便捷初始化器必须调用同一个类中的指定初始化器或者便捷初始化器。

{{< /admonition >}}

{{< admonition open=false type=question title="初始化器中赋值会触发属性观察器么？">}}

初始化器中赋值不会触发属性观察器，属性观察器只有在给属性赋值的时候才会触发。

{{< /admonition >}}

{{< admonition open=false type=question title="什么是反初始化器？">}}

deinit

{{< /admonition >}}

### swift 内存管理

{{< admonition open=false type=question title="swift 中内存管理方案？ARC引用类型有几种？">}}

- **ARC**：ARC 是 Swift 中的内存管理方案，ARC 是 Automatic Reference Counting 的缩写，表示自动引用计数，ARC 会在类的实例不再被使用时自动释放其占用的内存。
- **引用类型**：Swift 中的引用类型有 class、closure、function。

{{< /admonition >}}

{{< admonition open=false type=question title="Swift闭包循环引用如何产生，怎么解决？">}}

**闭包循环引用**是指闭包和闭包捕获的值相互引用，造成内存泄漏，解决闭包循环引用可以使用闭包捕获列表，闭包捕获列表会在闭包和捕获的值之间创建一个弱引用或无主引用，从而打破循环引用。

weak 弱引用<br>
unowned 无主引用<br>

{{< /admonition >}}

{{< admonition open=false type=question title="能否在定义闭包属性的同时引用self">}}

可以, 但是需要在闭包前面加上 `lazy` 关键字, 因为闭包在初始化器之前被调用, 而此时 self 还没有初始化完成.

{{< /admonition >}}

{{< admonition open=false type=question title="如果lazy属性是闭包调用的结果，是否需要考虑循环引用问题？">}}

不需要, 因为闭包调用完毕后, 闭包会被释放, 闭包中的 self 也会被释放.

{{< /admonition >}}

{{< admonition open=false type=question title="逃逸闭包能否捕获inout参数？">}}

不能, 因为逃逸闭包的调用时机不确定, 有可能在函数返回之后才调用, 这时候 inout 参数已经被释放了.

{{< /admonition >}}

{{< admonition open=false type=question title="Swift中指针类型有哪几种？">}}

- **UnsafePointer<Pointee>**：指向内存中某个类型的指针，不允许修改内存中的值。
- **UnsafeMutablePointer<Pointee>**：指向内存中某个类型的指针，允许修改内存中的值。
- **UnsafeRawPointer**：指向内存中某个类型的指针，不允许修改内存中的值，不允许进行内存的访问。
- **UnsafeMutableRawPointer**：指向内存中某个类型的指针，允许修改内存中的值，不允许进行内存的访问。

{{< /admonition >}}

### Swift 扩展

{{< admonition open=false type=question title="Swift中扩展与OC中分类有什么区别？能添加什么，不能添加什么？">}}

类别只能扩充方法，不能扩展属性和成员变量（如果包含成员变量会直接报错）；<br>
如果分类中声明了一个属性，那么分类只会生成这个属性的set、get方法声明，也就是不会有实现<br>
分类中的方法会覆盖原来类中的方法，但是不会覆盖原来类中的属性<br>
扩展只能扩充计算属性，不能扩展存储属性，也不能扩展成员变量<br>
扩展有时候也称为匿名分类，因为扩展是没有名字的<br>
扩展不能添加指定构造器，但是可以添加便利构造器<br>
扩展不能添加属性观察器<br>
扩展不能添加父类<br>
扩展也不能添加反初始化器<br>
扩展可以给协议提供默认实现，也间接实现可选协议的效果<br>
{{< /admonition >}}

### Swift继承

{{< admonition open=false type=question title="如何限制不能被重写，或者不能被继承？">}}

**final**：可以修饰类、属性、方法、下标、协议，表示不允许对类、属性、方法、下标、协议进行继承、重写或者修改。

{{< /admonition >}}

{{< admonition open=false type=question title="是否可以重写存储属性？">}}

不可以, 因为存储属性是直接存储在实例中的, 重写的话会破坏继承链. 但是可以重写计算属性.

{{< /admonition >}}

{{< admonition open=false type=question title="let修饰的属性能否重写？">}}

不能, 因为 let 修饰的属性是常量, 不能被修改.

{{< /admonition >}}

{{< admonition open=false type=question title="static与class修饰的属性的异同?">}}

**不同点**<br>

class 修饰的计算属性可以被重写，static 修饰的不能被重写。<br>

static 可以修饰存储属性，static 修饰的存储属性称为静态变量(常量)。<br>

static 修饰的静态方法不能被重写，class 修饰的类方法可以被重写。<br>

class 修饰的类方法被重写时，可以使用static 让方法变为静态方法。<br>

class 修饰的计算属性被重写时，可以使用static 让其变为静态属性，但它的子类就不能被重写了。<br>

**相同点**<br>

可以修饰方法，static 修饰的方法叫做静态方法，class 修饰的叫做类方法。<br>

都可以修饰计算属性。

{{< /admonition >}}

### Swift模式匹配

{{< admonition open=false type=question title="Swift中什么是模式匹配？有哪些?">}}

**模式匹配**是指检查一个值是否满足某种模式的过程，Swift 中的模式匹配可以用在 switch 语句中，也可以用在 if、guard、for-in、while、do-while 语句中。

- **通配符模式**：用下划线（_）表示，可以匹配任意类型的值，但是并不将匹配的值绑定到临时的常量或变量。
- **标识符模式**：用于匹配任意值，如果匹配成功，会将匹配的值赋值给指定的标识符。
- **值绑定模式**：用于将匹配的值绑定到一个临时的常量或变量，以便在模式的执行体中使用。
- **元组模式**：用于匹配元组，如果匹配成功，会将元组中的值分解成单独的常量或变量，以便在模式的执行体中使用。
- **可选模式**：用于匹配可选值，如果匹配成功，会将可选值中的值分解成单独的常量或变量，以便在模式的执行体中使用。
- **枚举模式**：用于匹配枚举值，如果匹配成功，会将枚举值分解成单独的常量或变量，以便在模式的执行体中使用。
- **表达式模式**：用于匹配表达式，如果匹配成功，会将表达式的值分解成单独的常量或变量，以便在模式的执行体中使用。
- **类型转换模式**：用于匹配指定类型的值，如果匹配成功，会将值转换成指定类型，并赋值给指定的常量或变量。

{{< /admonition >}}

{{< admonition open=false type=question title="通配符匹配中_和_?有什么区别？">}}

- **_**：表示匹配任意类型的值。
- **_?**：非空任意值。

{{< /admonition >}}

{{< admonition open=false type=question title="枚举Case模式中if case语句是什么？">}}

**if case** 语句是用来判断某个可选类型是否有值，如果有值，可以将值绑定到一个临时的常量或变量，以便在语句体中使用。

```swift
enum Score {
    case point(Int)
    case grade(Character)
}

let score = Score.point(100)
if case let Score.point(i) = score {
    print(i)
}
```

{{< /admonition >}}

### Swift 协议

{{< admonition open=false type=question title="Swift中什么是协议？协议能添加什么？">}}

协议规定了用来实现某一特定功能所必需的方法和属性。

- **协议中可以定义方法、属性、下标、初始化器的声明，协议中的方法、属性、下标、初始化器不需要写访问级别修饰符，因为协议中声明的都是抽象的内容，具体实现的时候才需要指定访问级别修饰符。**
- **协议中可以定义类型方法和类型属性，用 static 关键字来修饰。**
- **协议中可以定义 mutating 方法，用 mutating 关键字来修饰。**
- **协议中可以定义初始化器，但是不能定义析构器。**
- **协议中可以定义下标，但是不能定义存储属性和计算属性。**
- **协议中可以定义关联类型，关联类型通过 associatedtype 关键字来指定。**

{{< /admonition >}}

{{< admonition open=false type=question title="swift协议中定义的内容是否必须全部都实现？如果想要实现可选协议呢？">}}

一般来讲是需要全部实现, 如果向要可选,可以使用`extension` 来实现. (或者利用OC的`@optional`)
```swift
protocol Runnable {
    func run()
    func run1()
}

extension Runnable {
    func run1() {
        print("run1")
    }
}
```

{{< /admonition >}}































