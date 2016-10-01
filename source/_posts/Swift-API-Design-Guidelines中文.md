title: Swift API Design Guidelines中文
date: 2016-10-01 22:30:29
tags: Swift
---

##**原则**
在使用时的语义清楚为第一要素
语义清楚比简洁更重要

## 命名

### 增强使用时的语义
#### 加上所有能消除歧义并增加可读性的词汇
> 例如：
public mutating func remove(at position: Index) -> Element
employees.remove(at: x)
`at` 提高了可读性和增强意图

#### 去除累赘、并不能增强可读性的词，尤其那些只是重复描述了参数类型的词
> 例如：
removeElement(_ member: Element) -> Element?
应该去掉Element：remove(_ member: Element) -> Element?

#### 变量、参数、associated types的命名，描述它们的角色，而不是类型
> 例如：
restock(from widgetFactory: WidgetFactory)
应该是：restock(from supplier: WidgetFactory)

#### 对于类型信息不能明确表达意图的类型，例如NSObject, Any, AnyObject, Int, String，加上弱的类型描述信息来增强可读性。
> 例如：
func add(_ observer: NSObject, for keyPath: String)
grid.add(self, for: graphics)   // 语义不清
应该是：
func addObserver(_ observer: NSObject, forKeyPath path: String)
grid.addObserver(self, forKeyPath: graphics)  // 清楚

### 让用起来语义更流畅
#### 让方法和函数的命名在使用时读起来符合英文的语法规则
> 例如：
x.insert(y, at: z)                         “x, insert y at z"
x.subViews(havingColor: y)      “x’s subviews having color y"
x.capitalizingNouns()                “x, capitalizing nouns"

#### 工厂类方法名以make开头
> 例如：x.makeIterator()

#### 初始化和工厂方法名不应该包含第一个参数标签名，保持初始化方法参数结构独立性。
> 例如：
x.makeWidget(count: 47)
Color(red: 32, green: 64, blue: 128)
factory.makeWidget(gears: 42, spindles: 14)

#### 方法或函数的命名应该能描述它的副作用性
- 没有副作用的，应该读起来为名词，例如：x.distance(to: y), i.successor()
- 有副作用的，应该读起来为动词，例如：x.sort(), x.append(y)

协议，用来描述是什么类型，应该读起来为名词，例如：Collection
协议，用户描述能做什么，应该使用后缀`able`、`ible`、`ing`，例如：Equatable，ProgressReporting
 类型、属性、变量和常量应该为名词

###  正确的使用专业术语
尽量避免使用令人费解的专业术语，仅非常必要时使用。
如果你一定要使用专业术语，用的时候要符合它的本意，不要用它来表达一个与术语本身含义有偏差的事物。
避免使用缩写，除非它是广为人知并能轻松在网上搜索到的。
使用已有的、被业界广泛使用和约定俗成的术语。
> 例如：
使用术语Array来描述一组连续数据结构，而不是普通的List。
数学计算中，使用sin(x)，而不是自己为了表达这个意思琢磨出来的一个新词汇。

## 约定

### 常规约定
对于时间复杂度不为O(1)的方法，要在文档中标明。
除非特殊情况，使用方法和属性而非全局函数。
遵循大小写约定，类型和协议大写驼峰命名，其他的全部小写驼峰命名
对于基本意思相同而参数类型不同的，使用多态来保持方法名相同。

### 参数
#### 选择提高函数可读性的参数名
> 例如：
func move(from start: Point, to end: Point)

#### 利用为参数提供默认值的便利
> 例如：
syncData(withCompletion: nil)  // 应该给参数提供nil的默认值
syncDate() // 有默认值

#### 把有默认值的参数放在参数列表的尾部，来保证在省略了尾部参数后的语义连贯性。

### 标签
#### 当参数不能被有效的描述和区分时，省略所有标签
> 例如：
min(number1, number2)
zip(sequence1, sequence2)

#### 当初始化方法是做值的类型转换，省略第一个参数标签
> 例如：
Int64(someUInt32)

#### 当第一个参数属于为语义的介词部分，将整个介词部分作为第一个参数标签
> 例如：
x.removeBoxes(havingLength: 12)

***有一个例外，当前两个参数一起构成一种抽象的类型，第一个参数标签使用介词后面的部分（不包含介词本身），维持抽象类型的清晰性。***
例如：
a.move(toX: b, y: c)      // 破坏了后面抽象类型的清晰性
应该是：
a.moveTo(x: b, y: c)
a.fadeFrom(red: b, green: c, blue: d)

#### 当第一个参数属于语法完整性的一部分，省略第一个标签
> 例如：
x.addSubview(y)     // vt + 宾语，语法完整
***上面的规则意味着，如果第一个参数不属于语法完整性的一部分，但是又需要它增加可读性时，那么应该使用标签。***
例如：
view.dismiss(animated: false)
words.split(maxSplits: 12)
students.sorted(isOrderedBefore: Student.name)
规律：动词都是vi不及物动词，第一个标签都仅仅为描述性词汇，不属于语法部分。

## 特殊说明
#### 给闭包和元组加上参数名字
#### 特别注意参数类型可能为Any，AnyObject等多态函数带来的歧义。
> 例如：
public mutating func append(_ newElement: Element)
public mutating func append(_ ndwEelements: S) where S.Generator.Element = Element
第二个方法本意是append一组数据，但如果Element为Any
values.append([2, 3, 4])到底是调用上面哪个方法呢？要避免这种歧义。


