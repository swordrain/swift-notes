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
    } 
}
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
}
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

```
//1 Basic - if gurad for switch
if case (0, 0, 0) = point {
    return "At origin"
}
guard case (0, 0, 0) = point else {
    return "Not at origin"
}
let groupSizes = [1, 5, 4, 6, 2, 1, 3]
for case 1 in groupSizes {
  print("Found an individual") // 2 times
}

//2 Wildcard
if case (_, 0, 0) = coordinate {
    // x can be any value. y and z must be exactly 0.
    print("On the x-axis") // Printed!
}

//3 Value-binding
if case let (x, y, 0) = coordinate {
    print("On the x-y plane at (\(x), \(y))") // Printed: 1, 0
}

//4 Identifier

//5 tuple

//6 Enumeration case
if case .north = heading {
    print("Don't forget your jacket") // Printed!
}

//7 Optional
for case let name? in names {
    print(name) // 4 times
}

//8 “Is” type-casting
for element in array {
    switch element {    
    case is String:
        print("Found a string") // 1 time
    default:
        print("Found something else") // 2 times
    }
}

//9 “As” type-casting
for element in array {
    switch element {
    case let text as String:
        print("Found a string: \(text)") // 1 time
    default:
        print("Found something else") // 2 times
    }
}
//10 Qualifying with where
for level in levels {
    switch level {
        case .inProgress(let percent) where percent > 0.8 :
        print("Almost there!")
    case .inProgress(let percent) where percent > 0.5 :
        print("Halfway there!")
    case .inProgress(let percent) where percent > 0.2 :
        print("Made it through the beginning!")
    default:
    break
    } 
}

//11 Chaining with commas - Switch case 多个条件逗号分隔或者
if case .animal(let legs) = pet, case 2...4 = legs {
    print("potentially cuddly") // Printed!
} else {
    print("no chance for cuddles")
}

let a = 5
let b = 6
let c: Number? = .integerValue(7)
let d: Number? = .integerValue(8)
if a != b,
    let c = c,
    let d = d,
    case .integerValue(let cValue) = c,
    case .integerValue(let dValue) = d,
    dValue > cValue {
        print("a and b are different") // Printed!
        print("d is greater than c") // Printed!
        print("sum: \(a + b + cValue + dValue)") // Printed: 26
}

//12 Custom tuple
if case ("Bob", 23) = (name, age) {
    print("Found the right Bob!")
}

var username: String?
var password: String?
switch (username, password) {
    case let (username?, password?):
        print("Success!")
    case let (username?, nil):
        print("Password is missing")
    case let (nil, password?):
        print("Username is missing")
    case (nil, nil):
        print("Both username and password are missing")  // Printed!
}

//13 Validate that an optional exists
let user: String? = "Bob"
guard let _ = user else {
  print("There is no user.")
  fatalError()
}
print("User exists, but identity not needed.")
guard user != nil else {
  print("There is no user.")
  fatalError()
}

```

**Pattern的背后**

同一类型的数据使用`==`比较，不同类型的数据使用`~=`比较，必要时覆盖它

```
let matched = (1...10 ~= 5) // true
if case 1...10 = 5 {
    print("In the range")
}
```

**丢弃函数执行结果或阻止警告**

```
_ = sum(4,5)
@discardableResult
```

**函数重载时使用某一个**

```
class Dog{
    func bark() {
    }
    func bark(_ loudly:Bool) {
    }
    func bark(_ times:Int) {
    }
    func test() {
        let barkFunction = bark(_:) // compile error
        let barkFunction = bark as (Int) -> Void // "times", not "loudly"
    }
}
```

**类和实例的方法使用**

```
class Dog{
    func bark(){
        print("bark")
    }
}

let dog  = Dog()
dog.bark // () -> ()
dog.bark()
let f1 = Dog.bark //(Dog) -> ()->()
let f2 = f1(dog) //() -> ()
f2()
```

**Swift支持标签语句**

```
var sum = 0
rowLoop: for row in 0..<8 {
  columnLoop: for column in 0..<8 {
    if row == column {
      continue rowLoop
    }
    sum += row * column
  }
}
```
