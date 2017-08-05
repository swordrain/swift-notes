# Swift 拾遗

**运算符左右要么都有空格要么都没有空格**

```
1 + 6
1+6
```

**类型转换，注意`as`的方式**

```
let actuallyDouble = Double(3)
let actuallyDouble: Double = 3
let actuallyDouble = 3 as Double
```

**`++` `--`被干掉了**

**尽量用`Range`来进行确定的循环**

```
for _ in 1..<10 {
    ...
}
```

**for in / switch中的模式过滤**

```
for i in 1...count where i % 2 == 1 {
    sum += i 
}

switch(number){
    case let x where x % 2 == 0:
        print("Even")
    default:
        print("Odd")
}
```

**永远不会有返回的函数**

```
func infiniteLoop() -> Never {
    while true {
} }
```

**用`enumerated()`循环数组的index和value**

```
for (index, player) in players.enumerated() {
    print("\(index + 1). \(player)")
}
```

**struct默认的init不包含let的属性**

```
struct MyTest{
    var a: Int
    let b = 2
}

MyTest(a: 1)
```

**对于执行函数获得结果的property，尽量使用懒加载**

```
struct Circle {
    lazy var pi = { //必须是var
        return ((4.0 * atan(1.0 / 5.0)) - atan(1.0 / 239.0)) * 4.0
    }()
```

**Swift里enum struct class都可以继承protocol**

**重要的protocol**

```
protocol Equatable {
    static func ==(lhs: Self, rhs: Self) -> Bool
}

protocol Comparable: Equatable {
    static func <(lhs: Self, rhs: Self) -> Bool
    static func <=(lhs: Self, rhs: Self) -> Bool
    static func >=(lhs: Self, rhs: Self) -> Bool
    static func >(lhs: Self, rhs: Self) -> Bool
}

protocol Hashable : Equatable {
    var hashValue: Int { get }
}

protocol CustomStringConvertible { //打印对象的时候用，跟Java类似
    var description: String { get }
}
```

**Swift里的pattern match**

