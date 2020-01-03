## function
- 当调用一个函数的时候，每个传入的参数都会创建一个副本， 然后赋值给对应的函数变量，所以函数接受的是一个副本，而不是原始的参数。使用这种方式传递大的数组会变得很低效，并且在函数内部对数组的任何修改都仅影响副本，而不是原始数组

- 对于任何类型，如果它们的值可以是nil ，那么这个类型的nil 值可以使用一种转换表达式，例如 []int(nil）
- 你可能偶尔会看到有些函数的声明没有函数体，那说明这个函数使用除了Go 以外的语言实现。这样的声明定义了该函数的签名。
```
package math
func Sin(x float64) float64 // 使用汇编语言实现
```

## slice
- slice移除中间元素
```
func remove(slice []int, i int) []int {
    copy(slice[i:], slice[i+l:])
    return slice[ :len(slice)-1]
}

// 不需要维护顺序
func remove(slice "[]int, i int) []int {
    slice[i] = slice[len(slice)-1)
    return slice[ : len(slice)-1)
}
```
## struct
- 命名结构体类型s 不可以定义一个拥有相同结构体类型s 的成员变量，也就是一个聚合类型不可以包含它自己（同样的限制对数组也适用） 。但是5 中可以定义一个s 的指针类型，这样我们就可以创建一些递归数据结构，比如链表和树
```
type tree struct {
    value int
    left *tree
    right *tree
}

// 就地排序
func Sort(values []int) {
    var root *tree
    for_, v ：range values {
        root = add(root, v)
        appendValues(values[:0], root)
    }
}

// appendValues 将元素按照顺序追加到values 里面，然后返回结果slice
func appendValues(values []int, t *tree) []int {
    if t != nil {
        values = appendValues(va l ues, t.left)
        values = append(values, t.value)
        values = appendValues(va l ues, t.right)
    }
    return values
}

func add(t *t 「ee, value int) *t 「ee {
    if t == nil {
        // 等价于返回＆tree{value : value}
        t = new(tree)
        t.value = value
        return t
    }
    if value < t.value {
        t.left = add(t.left, value)
    } else {
        t.right = add(t.right, value)
    }
    return t
}
```
- 没有任何成员变量的结构体称为空结构体，写作struct{} 。它没有长度，也不携带任何信息，但是有的时候会很有用。有一些Go 程序员用它来替代被当作集合使用的map 中的布尔值，来强调只有键是有用的，但由于这种方式节约的内存很少并且语法复杂，所以一般尽量避免这样用
```
seen := make(map[string]struct{}) // 字符串集合
if _, ok : = seen [ s]; ! ok {
  seen[s] = struct{}{}
  //．．． 首次出现s ...
}
```
## panic
- 如果碰到“不可能发生”的状况，若机是最好的处理方式，比如,语句执行到逻辑上不可能到达的地方时：
```go
// 如果没有标题则返回错误
func soleTitle(doc *html.Node) (title string, err error) {
    type bailout struct{}
    defer func() {
        switch p := recover(); p{
        case nil:
            // 没有56 机
        case bailout{}:
            // ” 预期的” 窘机
            err = fmt.Errorf("multiple title eleme nts")
        default:
            panic(p) //未预期的后机；继续岩机过程
        }
    }()
    if title != "" {
        panic(bailout{}) //多个标题元素
    }
    return title, err
}
```
## 方法接收者
-在真实的程序中，习惯上遵循如果Point 的任何一个方法使用指针接收者，那么所有的Point 方法都应该使用指针接收者，即使有些方法并不一定需要。我们在Point 中打破了这个习惯只为了方便展示方法的不同使用方法。
命名类型（ Point ）与指向它们的指针（ *Point ）是唯一可以出现在接收者声明处的类型。而且，为防止混淆，不允许本身是指针的类型进行方法声明：
```
type P *int
func (P) f() { /* . .. *I } // 编译错误：非法的接收者类型
```
```go
p.ScaleBy(2)
Point{l, 2}.ScaleBy(2) ／／ 编译错误：不能获得Point 类型字面量的地址

比如， 6.5 节提到的IntSet 类型的String 方法，需要一个指针接收者，所以我们无法从
一个无地址的IntSet 值上调用该方法：
type IntSet struct { /* ... *I}
func (*IntSet) String() string
var _ = IntSet{}.String()  // 编译错误： String 方法需要＊ IntSet 接收者
但可以从一个Int Set 变量上调用该方法：
var s IntSet
var_= s.String() //OK: s 是一个变量，＆s 有String 方法
因为只有*IntSet 有String 方法，所以也只有可*IntSet 实现了fmt.Stringer、接口：
var ＿ fmt.Stringer = &s // OK
var ＿ fmt.Stringer ＝ s // 编译错误： IntSet缺少String 方法
```
> 实际上编译器会对变量进行旬的隐式转换。**只有变量才允许这么做**，包括结构体字段，像p.X 和数组或者slice 的元素，比如peim[0] 
**不能够对一个不能取地址的Point 接收者参数调用＊ Point 方法，因为无法获取临时变量的地址**
## go的封装
- 另一个结论就是在Go 语言中封装的单元是包而不是类型。无论是在函数内的代码还是方法内的代码，结构体类型内的字段对于同一个包中的所有代码都是可见的

## 接口即约定
- Go 语言中还有另外一种类型称为接口类型。接口是一种抽象类型，它并没有暴露所含数据的布局或者内部结构，当然也没有那些数据的基本操作，它所提供的仅仅是一些方法而已。如果你拿到一个接口类型的值，你无从知道它是什么，你能知道的仅仅是它能做什么，或者更精确地讲，仅仅是它提供了哪些方法。
- 可以把一种类型替换为满足同一接口的另一种类型的特性称为可取代性（ substitutability ），这也是面向对象语言的典型特征。

- 当处理错误或者调试时，能拿到接口值的动态类型是很有帮助的。可以使用fmt包的%T来实现这个需求：
```go
var w io.Writer
fmt.Printf("%T\n", w) // ”< nil >”
w = os.Stdout
fmt.Printf("%T\n", w) // "OS.File"
w = new(bytes.Buffer)
fmt.Printf("%T\n", w) // "*bytes.Buffer"
```
