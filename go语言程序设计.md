## function
- 当调用一个函数的时候，每个传入的参数都会创建一个副本， 然后赋值给对应的函数变
量，所以函数接受的是一个副本，而不是原始的参数。使用这种方式传递大的数组会变得
很低效，并且在函数内部对数组的任何修改都仅影响副本，而不是原始数组

- 对于任何类型，如果它们的值可以是nil ，那么这个类型的nil 值可以使用一种转换
表达式，例如 []int(nil）
- 你可能偶尔会看到有些函数的声明没有函数体，那说明这个函数使用除了Go 以外的语
言实现。这样的声明定义了该函数的签名。
```
package math
func Sin(x float64) float64 ／／ 使用汇编语言实现
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
- 命名结构体类型s 不可以定义一个拥有相同结构体类型s 的成员变量，也就是一个聚合
类型不可以包含它自己（同样的限制对数组也适用） 。但是5 中可以定义一个s 的指针类型，
即吨，这样我们就可以创建一些递归数据结构，比如链表和树
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
- 没有任何成员变量的结构体称为空结构体，写作struct{} 。它没有长度，也不携带任何
信息，但是有的时候会很有用。有一些Go 程序员用它来替代被当作集合使用的map 中的布
尔值，来强调只有键是有用的，但由于这种方式节约的内存很少并且语法复杂，所以一般尽
量避免这样用
```
seen := make(map[string]struct{}) // 字符串集合
if _, ok : = seen [ s]; ! ok {
  seen[s] = struct{}{}
  //．．． 首次出现s ...
}
```
