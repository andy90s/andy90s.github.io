# Android Interview



## 抽象类与接口的区别？

抽象类和接口都是为了规范子类的行为，但是它们之间有很大的区别。

抽象类和接口是面向对象编程中的两个重要概念，它们都可以用来定义方法的签名，但在实现细节和使用场景上有一些显著的区别。以下是它们之间的主要区别：

### 1. 定义方式

- **抽象类**：使用 `abstract` 关键字定义，可以包含抽象方法（没有方法体的方法）和具体方法（有方法体的方法）。

  ```kotlin
  abstract class Animal {
      abstract fun makeSound()
      fun sleep() {
          println("Sleeping")
      }
  }
  ```

- **接口**：使用 `interface` 关键字定义，只能包含抽象方法（在 Kotlin 中接口也可以包含默认方法实现）。

  ```kotlin
  interface Animal {
      fun makeSound()
      fun sleep() {
          println("Sleeping")
      }
  }
  ```

### 2. 实现方式

- **抽象类**：一个类只能继承一个抽象类（单继承），但可以实现多个接口。

  ```kotlin
  abstract class Animal {
      abstract fun makeSound()
      fun sleep() {
          println("Sleeping")
      }
  }

  class Dog : Animal() {
      override fun makeSound() {
          println("Bark")
      }
  }
  ```

- **接口**：一个类可以实现多个接口（多继承）。

  ```kotlin
  interface Animal {
      fun makeSound()
      fun sleep() {
          println("Sleeping")
      }
  }

  interface Pet {
      fun play()
  }

  class Dog : Animal, Pet {
      override fun makeSound() {
          println("Bark")
      }

      override fun play() {
          println("Playing")
      }
  }
  ```

### 3. 成员变量

- **抽象类**：可以包含成员变量，并且可以定义其初始值。

  ```kotlin
  abstract class Animal {
      var name: String = "Unknown"
      abstract fun makeSound()
  }
  ```

- **接口**：不能包含成员变量，但可以包含常量（`val`）。

  ```kotlin
  interface Animal {
      val name: String
      fun makeSound()
  }

  class Dog : Animal {
      override val name: String = "Dog"
      override fun makeSound() {
          println("Bark")
      }
  }
  ```

### 4. 构造函数

- **抽象类**：可以有构造函数。

  ```kotlin
  abstract class Animal(val name: String) {
      abstract fun makeSound()
  }

  class Dog(name: String) : Animal(name) {
      override fun makeSound() {
          println("Bark")
      }
  }
  ```

- **接口**：不能有构造函数。

### 5. 访问修饰符

- **抽象类**：可以使用各种访问修饰符（`public`, `protected`, `private`）。

  ```kotlin
  abstract class Animal {
      protected abstract fun makeSound()
  }
  ```

- **接口**：方法默认是 `public`，不能使用 `protected` 或 `private` 修饰符。

  ```kotlin
  interface Animal {
      fun makeSound() // 默认是 public
  }
  ```

### 6. 继承结构

- **抽象类**：适用于具有共同特性的类，提供部分实现。

  ```kotlin
  abstract class Animal {
      abstract fun makeSound()
      fun sleep() {
          println("Sleeping")
      }
  }

  class Dog : Animal() {
      override fun makeSound() {
          println("Bark")
      }
  }
  ```

- **接口**：适用于定义类的行为或能力，不提供实现细节。

  ```kotlin
  interface Animal {
      fun makeSound()
      fun sleep() {
          println("Sleeping")
      }
  }

  class Dog : Animal {
      override fun makeSound() {
          println("Bark")
      }
  }
  ```

### 总结

- **抽象类** 适用于具有共同特性的类，提供部分实现，支持单继承，允许定义成员变量和构造函数。
- **接口** 适用于定义类的行为或能力，支持多继承，不允许定义成员变量，但可以包含常量和默认方法实现。

在实际开发中，选择使用抽象类还是接口取决于具体的设计需求和场景。

## 什么是反射机制？反射机制的应用场景有哪些？

反射机制（Reflection）是指程序在运行时能够检查和操作自身结构的一种能力。它允许程序在运行时获取有关类、方法、属性等的详细信息，并且可以动态地调用方法、访问属性等。反射机制在许多编程语言中都有实现，包括 Java 和 Kotlin。

### 反射机制的基本概念

在 Kotlin 中，反射主要是通过 `kotlin.reflect` 包来实现的。以下是一些常用的反射操作：

- **获取类的引用**：通过类的引用可以获取类的所有信息。
  
  ```kotlin
  val kClass = MyClass::class
  ```

- **获取构造函数**：可以获取类的构造函数，并创建实例。

  ```kotlin
  val constructor = kClass.primaryConstructor
  val instance = constructor?.call()
  ```

- **获取属性**：可以获取类的属性，并进行读取和修改。

  ```kotlin
  val property = kClass.memberProperties.find { it.name == "propertyName" }
  val value = property?.get(instance)
  ```

- **调用方法**：可以获取类的方法，并进行调用。

  ```kotlin
  val method = kClass.memberFunctions.find { it.name == "methodName" }
  method?.call(instance, arg1, arg2)
  ```

### 反射机制的应用场景

反射机制在实际开发中有许多应用场景，以下是一些常见的应用：

1. **框架和库的开发**：
   - **依赖注入**：如 Dagger、Koin 等依赖注入框架，通过反射来创建和注入依赖对象。
   - **ORM（对象关系映射）**：如 Hibernate、Room 等 ORM 框架，通过反射来映射数据库表和对象之间的关系。
   - **序列化和反序列化**：如 Gson、Jackson 等 JSON 序列化库，通过反射来将对象转换为 JSON 字符串，或将 JSON 字符串转换为对象。

2. **动态代理**：
   - 动态代理可以在运行时创建代理对象，常用于 AOP（面向切面编程）和拦截器等场景。Java 中的 `java.lang.reflect.Proxy` 类和 Kotlin 中的 `kotlin.reflect.full.createType` 以及相关 API 都可以实现动态代理。

3. **测试和调试**：
   - **单元测试**：在测试中，通过反射可以访问和修改私有成员，进行更细粒度的测试。
   - **调试工具**：一些调试工具和框架通过反射获取对象的详细信息，帮助开发者分析和调试代码。

4. **插件和模块化架构**：
   - 在插件化和模块化架构中，通过反射可以动态加载和调用模块，增强系统的扩展性和灵活性。

5. **通用工具和库**：
   - **通用工具**：一些通用工具库通过反射来提供通用的功能，例如对象拷贝、对象比较等。
   - **配置管理**：某些配置管理工具通过反射来动态加载和应用配置。

### 反射的优缺点

**优点**：
- **灵活性高**：可以在运行时动态操作对象，提高代码的灵活性和扩展性。
- **通用性强**：可以编写通用的工具和框架，适用于不同的类和对象。

**缺点**：
- **性能开销**：反射操作通常比直接调用要慢，可能会影响性能。
- **安全性问题**：反射可以访问和修改私有成员，可能会破坏类的封装性和安全性。
- **代码复杂度**：反射代码通常比直接调用更复杂，可能增加代码的维护难度。

### 总结

反射机制是一种强大的工具，能够在运行时动态操作对象和类。它在框架开发、动态代理、测试调试、插件架构等方面有广泛的应用。然而，由于反射操作的性能开销和潜在的安全性问题，应谨慎使用反射，并在必要时进行性能优化和安全防护。

## 什么是线程池？如何创建一个线程池？

线程池（Thread Pool）是一种并发设计模式，用于管理和重用一组线程，以便执行任务。线程池通过减少线程创建和销毁的开销，提高了系统性能和资源利用率。线程池在处理大量短生命周期任务时尤其有用。

### 线程池的概念

- **线程复用**：通过复用线程池中的线程，减少了频繁创建和销毁线程的开销。
- **任务队列**：线程池通常包含一个任务队列，等待执行的任务会被放入队列中，空闲线程会从队列中取任务执行。
- **资源管理**：线程池可以限制并发线程的数量，防止资源耗尽和系统过载。

### 如何创建一个线程池

在 Java 和 Kotlin 中，可以使用 `java.util.concurrent` 包中的 `Executors` 工具类来创建线程池。以下是几种常见的线程池类型及其创建方法：

1. **固定大小线程池（Fixed Thread Pool）**：
   - 具有固定数量的线程池，当所有线程都在执行任务时，新任务会在队列中等待。
   
   ```kotlin
   val executor = Executors.newFixedThreadPool(4)
   ```

2. **缓存线程池（Cached Thread Pool）**：
   - 具有动态调整线程数量的线程池，适用于执行大量短期异步任务。
   
   ```kotlin
   val executor = Executors.newCachedThreadPool()
   ```

3. **单线程池（Single Thread Executor）**：
   - 只有一个线程的线程池，所有任务按顺序执行。
   
   ```kotlin
   val executor = Executors.newSingleThreadExecutor()
   ```

4. **调度线程池（Scheduled Thread Pool）**：
   - 具有定时和周期性任务执行功能的线程池。
   
   ```kotlin
   val executor = Executors.newScheduledThreadPool(4)
   ```

### 使用线程池执行任务

创建线程池后，可以通过提交任务来使用线程池。任务可以是实现了 `Runnable` 或 `Callable` 接口的对象。

1. **提交 `Runnable` 任务**：

   ```kotlin
   val executor = Executors.newFixedThreadPool(4)
   
   for (i in 1..10) {
       executor.execute {
           println("Task $i is running on thread ${Thread.currentThread().name}")
       }
   }
   
   executor.shutdown() // 关闭线程池
   ```

2. **提交 `Callable` 任务并获取结果**：

   ```kotlin
   val executor = Executors.newFixedThreadPool(4)
   val futureList = mutableListOf<Future<Int>>()
   
   for (i in 1..10) {
       val future = executor.submit(Callable {
           println("Task $i is running on thread ${Thread.currentThread().name}")
           i * i
       })
       futureList.add(future)
   }
   
   for (future in futureList) {
       println("Result: ${future.get()}")
   }
   
   executor.shutdown() // 关闭线程池
   ```

### 自定义线程池

有时需要更灵活的线程池配置，可以使用 `ThreadPoolExecutor` 类来创建自定义线程池：

```kotlin
val corePoolSize = 4
val maximumPoolSize = 10
val keepAliveTime = 60L
val timeUnit = TimeUnit.SECONDS
val workQueue = LinkedBlockingQueue<Runnable>()

val executor = ThreadPoolExecutor(
    corePoolSize,
    maximumPoolSize,
    keepAliveTime,
    timeUnit,
    workQueue
)

// 提交任务
for (i in 1..10) {
    executor.execute {
        println("Task $i is running on thread ${Thread.currentThread().name}")
    }
}

executor.shutdown() // 关闭线程池
```

### 关闭线程池

- **`shutdown()`**：平滑关闭线程池，已提交的任务会继续执行，但不再接受新任务。
  
  ```kotlin
  executor.shutdown()
  ```

- **`shutdownNow()`**：立即关闭线程池，尝试停止所有正在执行的任务，并返回等待执行的任务列表。
  
  ```kotlin
  executor.shutdownNow()
  ```

### 总结

线程池是一种高效的并发处理机制，通过复用线程和管理任务队列，显著提高了系统性能和资源利用率。通过 `Executors` 工具类，可以方便地创建不同类型的线程池，并通过提交 `Runnable` 或 `Callable` 任务来执行并发操作。在实际开发中，根据具体需求选择合适的线程池类型，并注意合理管理线程池的生命周期。

## 
