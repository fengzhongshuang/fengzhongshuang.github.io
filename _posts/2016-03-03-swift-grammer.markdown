---
title:  "Swift语法"
summary:  Swift语法简介。
categories: [iOS]
tags: [Swift]
---

### Swift语法

---

1. 基础部分

  用 `let` 声明常量，用 `var` 声明变量。可以不指定数据类型，Swift 通过**类型推断**获取变量或常量的类型。也可以指定类型。

  声明指定类型的常量或变量：
  ```Swift
  var welcomeMessage: String
  let pi: Double = 3.14
  ```

  常量与变量名不能包含数学符号,箭头,保留的(或者非法的)Unicode 码位,连线与制表符。也不能以数字开 头,但是可以在常量与变量名的其他地方包含数字。

  可以使用 `typealias` 关键字来定义类型别名。定义别名后就可以在任何使用该类型的地方换成类型别名。
  ```Swift
  typealias AudioSample = UInt16
  ```

  布尔值使用 `true` 和 `false`，而在OC中，使用 `YES` 和 `NO`。

  **元组(tuples)** 把多个值组合成一个复合值。元组内的值可以是任意类型,并不要求是相同类型。可以把元组进行分解，对于要忽略的部分可以使用 `_` 来代替。也可以通过下标的方式访问其中的元素，下标从 0 开始。还可以对元组的元素进行命名，访问时就可以使用名称访问。

  **可选类型**，用类型后加问号的形式表示。如 `Int?`.

  Swift 的 nil 和 Objective-C 中的 nil 并不一样。在 Objective-C 中, nil 是一个指向不存在对象的指针。在 Swift 中, nil 不是指针——它是一个确定的值,用来表示值缺失。任何类型的可选状态都可以被设置为 nil ,不只是对象类型。

  **可选绑定**(optional binding)来判断可选类型是否包含值,如果包含就把值赋给一个临时常量或者变量
  ```Swift
  if let constantName = someOptional {
    statements
  }
  ```

  **隐式解析可选类型** 把想要用作可选的类型的 后面的问号( String? )改成感叹号( String! )

2. 基本运算符

  **空合运算符**( a ?? b )将对可选类型 a 进行空判断,如果 a 包含一个值就进行解封,否则就返回一个默认值 b .这 个运算符有两个条件：
  * 表达式 a 必须是Optional类型
  * 默认值 b 的类型必须要和 a 存储值的类型保持一致

  **区间运算符**
  * 闭区间运算符( a...b )定义一个包含从 a 到 b (包括 a 和 b )的所有值的区间, b 必须大于等于 a
  * 半开区间( a..<b )定义一个从 a 到 b 但不包括 b 的区间

3. 字符串和字符

  Swift 的 String 类型是值类型。

  **字符串插值** 是一种构建新字符串的方式,可以在其中包含常量、变量、字面量和表达式。 您插入的字符串字面量 的每一项都在以反斜线为前缀的圆括号中:
  ```
  let multiplier = 3
  let message = "\(multiplier) times 2.5 is \(Double(multiplier) * 2.5)" // message is "3 times 2.5 is 7.5"
  ```

4. 集合类型
  Swift 语言提供 `Arrays` 、`Sets`  和 `Dictionaries` 三种基本的集合类型用来存储集合数据。数组是有序数据的 集。集合是无序无重复数据的集。字典是无序的键值对的集。

  **数组** 使用[]进行创建。通过`count`获取元素数量，`append`添加元素，使用下标获取元素，下标从 0 开始， `insert`插入元素，`removeAtIndex` 删除元素， `removeLast`移除最后一个元素，使用 `for-in`遍历，如果我们同时需要每个数据项的值和索引值,可以使用 `enumerate()` 方法来进行数组遍历。
  ```
  for (index, value) in shoppingList.enumerate() {
    print("Item \(String(index + 1)): \(value)")
  }
  ```

  **集合** 一个类型为了存储在集合中,该类型必须是 **可哈希化** 的。Swift 的所有基本类型(比如 String、Int 和 Double 等)默认都是可哈希化的,可以作为集合的值的类型或者字典的键的类型。没有关联值的枚举成员值(在枚举有讲述)默认也是可哈希化的。

  Set类型被写成 Set<类型>。可以使用数组字面量来创建集合。
  ```
  var favoriteGenres: Set<String> = ["Rock", "Classical", "Hip hop"]
  // favoriteGenres 被构造成含有三个初始值的集合
  ```
  通过`count`获取元素数量，`append`添加元素，使用下标获取元素，下标从 0 开始， `insert`插入元素，`removeAtIndex` 删除元素，`contains`判断是否包含某个元素。`intersect`获取交集，`union`获取并集，`subtract`获取差集，`exclusiveOr`获取异或集合。`isSubsetOf`是否子集，`isSupersetOf`是否超集，`isDisjointWith`是否完全不包含相同元素。

  **字典** 使用 `Dictionary<Key, Value>` 定义,也可以用 `[Key: Value]` 这样快捷的形式去创建一个字典类型。字典的key必须是可哈希的。

  `updateValue`更新或设置某个key对应的值。`removeValueForKey`移除key对应的值。通过访问 `keys` 或者 `values` 属性,我们也可以遍历字典的键或者值。

5. 控制流

   ```
   for item in collections
   for initialization ; condition ; increment { statements }

   while condition { statements }
   repeat { statements } while condition

   if {} else if {} else {}

   switch some value to consider {
     case value 1 : respond to value 1
     case value 2 , value 3 : respond to value 2 or 3 default: 默认分支必须在 switch 语句的最后面。
   }
   ```
   case 分支的模式可以使用 where 语句来判断额外的条件
   ```
   case let (x, y) where x == y:
   ```
   case 分支是默认 break 的，如果想实现 C 语言中的 case 功能，则需要使用 fallthrough 关键字。
