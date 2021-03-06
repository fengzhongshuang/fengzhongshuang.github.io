---
title:  "Swift语法"
summary:  Swift语法简介。
categories: [iOS]
tags: [Swift]
---

### 1. 基础部分

用 `let` 声明常量，用 `var` 声明变量。可以不指定数据类型，Swift 通过**类型推断**获取变量或常量的类型。也可以指定类型。

声明指定类型的常量或变量：

```swift
  var welcomeMessage: String

  let pi: Double = 3.14
```

常量与变量名不能包含数学符号,箭头,保留的(或者非法的)Unicode 码位,连线与制表符。也不能以数字开 头,但是可以在常量与变量名的其他地方包含数字。

可以使用 `typealias` 关键字来定义类型别名。定义别名后就可以在任何使用该类型的地方换成类型别名。

```swift
  typealias AudioSample = UInt16
```

布尔值使用 `true` 和 `false`，而在OC中，使用 `YES` 和 `NO`。

**元组(tuples)** 把多个值组合成一个复合值。元组内的值可以是任意类型,并不要求是相同类型。可以把元组进行分解，对于要忽略的部分可以使用 `_` 来代替。也可以通过下标的方式访问其中的元素，下标从 0 开始。还可以对元组的元素进行命名，访问时就可以使用名称访问。

**可选类型**，用类型后加问号的形式表示。如 `Int?`.

Swift 的 nil 和 Objective-C 中的 nil 并不一样。在 Objective-C 中, nil 是一个指向不存在对象的指针。在 Swift 中, nil 不是指针——它是一个确定的值,用来表示值缺失。任何类型的可选状态都可以被设置为 nil ,不只是对象类型。

**可选绑定**(optional binding)来判断可选类型是否包含值,如果包含就把值赋给一个临时常量或者变量

```swift
  if let constantName = someOptional {
    statements
  }
```

**隐式解析可选类型** 把想要用作可选的类型的 后面的问号( String? )改成感叹号( String! )

### 2. 基本运算符

**空合运算符**( a ?? b )将对可选类型 a 进行空判断,如果 a 包含一个值就进行解封,否则就返回一个默认值 b .这 个运算符有两个条件：

  * 表达式 a 必须是Optional类型
  * 默认值 b 的类型必须要和 a 存储值的类型保持一致

**区间运算符**

  * 闭区间运算符( a...b )定义一个包含从 a 到 b (包括 a 和 b )的所有值的区间, b 必须大于等于 a
  * 半开区间( a..<b )定义一个从 a 到 b 但不包括 b 的区间

### 3. 字符串和字符

Swift 的 String 类型是值类型。

**字符串插值** 是一种构建新字符串的方式,可以在其中包含常量、变量、字面量和表达式。 您插入的字符串字面量 的每一项都在以反斜线为前缀的圆括号中:

```swift
  let multiplier = 3
  let message = "\(multiplier) times 2.5 is \(Double(multiplier) * 2.5)" // message is "3 times 2.5 is 7.5"
```

### 4. 集合类型

Swift 语言提供 `Arrays` 、`Sets`  和 `Dictionaries` 三种基本的集合类型用来存储集合数据。数组是有序数据的 集。集合是无序无重复数据的集。字典是无序的键值对的集。

**数组** 使用[]进行创建。通过`count`获取元素数量，`append`添加元素，使用下标获取元素，下标从 0 开始， `insert`插入元素，`removeAtIndex` 删除元素， `removeLast`移除最后一个元素，使用 `for-in`遍历，如果我们同时需要每个数据项的值和索引值,可以使用 `enumerate()` 方法来进行数组遍历。

```swift
  for (index, value) in shoppingList.enumerate() {
    print("Item \(String(index + 1)): \(value)")
  }
```

**集合** 一个类型为了存储在集合中,该类型必须是 **可哈希化** 的。Swift 的所有基本类型(比如 String、Int 和 Double 等)默认都是可哈希化的,可以作为集合的值的类型或者字典的键的类型。没有关联值的枚举成员值(在枚举有讲述)默认也是可哈希化的。

Set类型被写成 Set<类型>。可以使用数组字面量来创建集合。

```swift
  var favoriteGenres: Set<String> = ["Rock", "Classical", "Hip hop"]
  // favoriteGenres 被构造成含有三个初始值的集合
```

通过`count`获取元素数量，`append`添加元素，使用下标获取元素，下标从 0 开始， `insert`插入元素，`removeAtIndex` 删除元素，`contains`判断是否包含某个元素。`intersect`获取交集，`union`获取并集，`subtract`获取差集，`exclusiveOr`获取异或集合。`isSubsetOf`是否子集，`isSupersetOf`是否超集，`isDisjointWith`是否完全不包含相同元素。

**字典** 使用 `Dictionary<Key, Value>` 定义,也可以用 `[Key: Value]` 这样快捷的形式去创建一个字典类型。字典的key必须是可哈希的。

`updateValue`更新或设置某个key对应的值。`removeValueForKey`移除key对应的值。通过访问 `keys` 或者 `values` 属性,我们也可以遍历字典的键或者值。

### 5. 控制流

```swift
for item in collections
for initialization ; condition ; increment {
  statements
}

while condition { statements }
repeat { statements } while condition

if {} else if {} else {}

switch some value to consider {
  case value 1 : respond to value 1
  case value 2 , value 3 : respond to value 2 or 3
  default: 默认分支必须在 switch 语句的最后面。
}
```

case 分支的模式可以使用 where 语句来判断额外的条件

```swift
   case let (x, y) where x == y:
```

case 分支是默认 break 的，如果想实现 C 语言中的 case 功能，则需要使用 fallthrough 关键字。

### 6. 函数
```swift
    func funcName([paramName: paramType[, ...]]) [-> returnType]{

    }
```

函数返回值可以是一个元组，原来返回多个值。

可选元组返回类型(Optional Tuple Return Types)，`(String, Int)?`, 表示整个元组是可选的，既可能返回nil值。这与 `(String?, Int?)`是不同的，它表示元组中的元素可能是nil值

**函数参数名称** 函数参数有一个 **外部参数名** 和一个 **内部参数名**，外部参 数名用来标记传递给函数调用的参数,本地参数名在实现函数的时候使用。一般情况下，第一个参数省略其外部参数名，第二个以后的参数使用其本地参数名作为自己的外部参数名。所有参数需要有不同的本地参数名,但可以共享相同的外部参数名。

可以在本地参数名前指定外部参数名,中间以空格分隔。如果指定了外部参数名，则调用时必须使用外部参数名。若不想指定外部参数名，可以使用 `_` 代替。

可以指定默认参数值。

**可变参数** 可以接受0个或多个值。使用 `...` 表示。

```swift
    func arithmeticMean(numbers: Double...) -> Double {
      // do something...
    }
```

*如果函数有一个或多个带默认值的参数,而且还有一个可变参数,那么把可变参数放在参数表的最后。*

**函数参数默认是常量。** 通过在参数名前加关键字 `var` 来定义变量参数。

**输入输出参数** 保证参数在函数体内修改后仍然保持修改后的状态。定义一个输入输出参数时,在参数定义前加 `inout` 关键字。传参时需要在实参前面使用 `&` 符号。

*输入输出参数不能有默认值,而且可变参数不能用 inout 标记。如果你用 inout 标记一个参数,这个 参数不能被 var 或者 let 标记。*

**函数类型** 可以定义一个类型为函数的常量或变量,并将函数 赋值给它。可以作为其他函数的参数。也可以作为函数的返回类型。

```swift
    var mathFunction: (Int, Int) -> Int = addTwoInts
```

**嵌套函数** 指定义在函数中的函数。

### 7. 闭包

闭包是自包含的函数代码块,可以在代码中被传递和使用。闭包可以捕获和存储其所在上下文中任意常量和变量的引用。 这就是所谓的闭合并包裹着这些常量和变量,俗称闭包。

闭包的三种形式：

  * 全局函数是一个有名字但不会捕获任何值的闭包
  * 嵌套函数是一个有名字并可以捕获其封闭函数域内值的闭包
  * 闭包表达式是一个利用轻量级语法所写的可以捕获其上下文中变量或常量值的匿名闭包

**闭包表达式** 可以使用常量、变量和 inout 类型作为参数,不提供默认值。 也可以在参数列表的最后使用可变参数。 元组也可以作为参数和返回值。

```swift
    {(parameters) -> returnType in
      statements
    }
```

闭包的函数体部分由关键字 `in` 引入。 该关键字表示闭包的参数和返回值类型定义已经完成,闭包函数体即将开始。

根据上下文推断类型可以省略参数类型和返回值类型

```swift
     { s1, s2 in return s1 > s2 }
```

单行表达式闭包可以通过隐藏 return 关键字来隐式返回单行表达式的结果。

```swift
     { s1, s2 in s1 > s2 }
```

参数名称缩写可以在闭包参数列表中省略对其的定义,并且对应参数名称缩写的 类型会通过函数类型进行推断。 in 关键字也同样可以被省略。

```swift
    {$0 > $1}
```

运算符函数

**尾随闭包** 是一个书写在函数括号之后的闭包表达式,函数支持将其作为最后一个参数调用。

```swift
    func someFunctionThatTakesAClosure(closure: () -> Void) {
      // 函数体部分
    }
    // 以下是不使用尾随闭包进行函数调用
    someFunctionThatTakesAClosure({
      // 闭包主体部分
    })
    // 以下是使用尾随闭包进行函数调用
    someFunctionThatTakesAClosure() {
        // 闭包主体部分
    }
```

**函数和闭包都是引用类型**

### 8. 枚举

```swift
  enum SomeEnumeration {
    // enumeration definition goes here
  }

  例：
  enum CompassPoint {
    case North
    case South
    case East
    case West
  }
```

在判断一个枚举值时，`switch` 必须穷举所有情况，否则无法通过编译。当不需要匹配每个枚举成员的时候，可以使用 `default` 覆盖未明确的值。

可以在枚举成员前加上 `indirect` 来表示这 成员可递归。也可以在枚举类型开头加上 indirect 关键字来表示它的所有成员都是可递归的

### 9. 类和结构体

Swift 中类和结构体共同点：

  * 定义属性用于存储值
  * 定义方法用于提供功能
  * 定义附属脚本用于访问值
  * 定义构造器用于生成初始化值
  * 通过扩展以增加默认实现的功能
  * 实现协议以提供某种标准功能

类提供的更多功能：

  * 继承允许一个类继承另一个类的特征
  * 类型转换允许在运行时检查和解释一个类实例的类型
  * 解构器允许一个类实例释放任何其所被分配的资源
  * 引用计数允许对一个类的多次引用

**定义**：类名或结构体名使用 UpperCamelCase 方式命名，属性和方法用 lowerCamelCase 命名。

```swift
  class SomeClass {
    // class definition goes here
  }
  struct SomeStructure {
    // structure definition goes here
  }
```  

属性访问使用**点语法**

所有结构体都有一个自动生成的成员逐一构造器,用于初始化新结构体实例中成员的属性。但是类没有逐一构造器。

**结构体和枚举是值类型。** 在 Swift 中,所有的基本类型:整数(Integer)、浮点 数(floating-point)、布尔值(Boolean)、字符串(string)、数组(array)和字典(dictionary),都是值类型。

**类是引用类型**

类和结构体的选择，出现以下任意一种或多种情况时建议使用结构体：

  * 结构体的主要目的是用来封装少量相关简单数据值。
  * 有理由预计一个结构体实例在赋值或传递时,封装的数据将会被拷贝而不是被引用。
  * 任何在结构体中储存的值类型属性,也将会被拷贝,而不是被引用。
  * 结构体不需要去继承另一个已存在类型的属性或者行为。

Swift 中 字符串(String)、数组(Array)和字典(Dictionary) 类型均以结构体的形式实现，使用值传递。

Objective-C中 字符串(NSString) , 数组(NSArray) 和 字典(NSDictionary) 类型均以类的形式实现，使用引用传递。

### 10. 属性
存储属性存储常量或变量作为实例的一部分,而计算属性计算(不是存储)一个值。计算属性可以用于类、结构体和枚举,存储属性只能用于类和结构体.

延迟存储属性是指当第一次被调用的时候才会计算其初始值的属性。在属性声明前使用 `lazy` 来标示一个延迟存储属性。必须将延迟存储属性声明成变量(使用 var 关键字),因为属性的初始值可能在实例构造完成之后才会得 到。而常量属性在构造过程完成之前必须要有初始值,因此无法声明成延迟属性。

**计算属性** 不直接存储值,而是提供一个 getter 和一个可选 的 setter,来间接获取和设置其他属性或变量的值。如果计算属性的 setter 没有定义表示新值的参数名,则可以使用默认名称 `newValue` 。

只有 `getter` 没有 `setter` 的计算属性就是 **只读计算属性**。只读计算属性总是返回一个值,可以通过点运算符访 问,但不能设置新的值。

**属性观察器** 属性观察器监控和响应属性值的变化,每次属性被设置值的时候都会调用属性观察器,甚至新的值和现在的值相同的时候也不例外。可以为除了延迟存储属性之外的其他存储属性添加属性观察器,也可以通过重载属性的方式为继承的属性(包括 存储属性和计算属性)添加属性观察器。

  * willSet 在新的值被设置之前调用。willSet 观察器会将新的属性值作为常量参数传入,在 willSet 的实现代码中可以为这个参数指定一个名称,如果 不指定则参数仍然可用,这时使用默认名称 `newValue` 表示。
  * didSet 在新的值被设置之后立即调用。didSet 观察器会将旧的属性值作为参数传入,可以为该参数命名或者使用默认参数名 `oldValue` 。

举例如下：
```swift
class StepCounter {
  var totalSteps: Int = 0 {
    willSet(newTotalSteps) {
      print("About to set totalSteps to \(newTotalSteps)")
    }
    didSet {
      if totalSteps > oldValue {
        print("Added \(totalSteps - oldValue) steps")
        }
    }     
  }
}
```

在全局或局部范围都可以定义计算型变量和为存储型变量定义观察器。

**全局的常量或变量都是延迟计算的，但不需要 lazy 标记。局部范围的常量或变量不会延迟计算。**

**类型属性** 用于定义特定类型所有实例共享的数据，所有实例共用一份，必须指定默认值，使用 `static` 关键字标识。

值类型的存储型类型属性可以是变量或常量,计算型类型属性跟实例的计算属性一样只能定义成变量属性。

### 11.方法
结构体和枚举能够定义方法是 Swift 与 C/Objective-C 的主要区别之一。

**实例方法** 是属于某个特定类、结构体或者枚举类型实例的方法。实例方法提供访问和修改实例属性的方法或提供
与实例目的相关的功能,并以此来支撑实例的功能。

Swift 默认仅给方法的第一个参数名称一个局部参数名称;默认同时给第二个和后续的参数名称局部参 数名称和外部参数名称。

类型的每一个实例都有一个隐含属性叫做 self , self 完全等同于该实例本身。

结构体和枚举是值类型。一般情况下,值类型的属性不能在它的实例方法中被修改。但是可以通过 `变异` 的方式使得方法可以改变属性，并且会保留改变的结果。完成变异只需要在方法开始时加上 `mutating` 关键字。

举例如下：
```swift
struct Point {
  var x = 0.0, y = 0.0
  mutating func moveByX(deltaX: Double, y deltaY: Double) {
    x += deltaX
    y += deltaY
  }
}
```

**变异方法能够赋给隐含属性 self 一个全新的实例。**
