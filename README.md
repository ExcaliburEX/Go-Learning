# <font face="Computer Modern" color=#FF0000>Go Learning </font>
🙆🙆‍♀️🙆‍♂️🙅🙅‍♀️🙅‍♂️🙋🙋‍♀️🙋‍♂️<br>😅😅😅

<font face="Noto Serif SC">

# 最新学习情况的思维导图
![图](https://github.com/ExcaliburEX/Go-Learning/blob/master/Go.png)


### 🕐 20.3.17 

## 0️⃣ 基本语法 
- 变量声明
    - 声明变量的一般形式是使用 var 关键字
        ```go
        var name type
        ```
    - Go语言的基本类型
        - bool
        - string
        - int、int8、int16、int32、int64
        - uint、uint8、uint16、uint32、uint64、uintptr
        - byte // uint8 的别名
        - rune // int32 的别名 代表一个 Unicode 码
        - float32、float64
        - complex64、complex128
        
    - 自动赋初值，int 为 0，float 为 0.0，bool 为 false，string 为空字符串，指针为 nil 
    - 批量赋值
        ```go
            var (
            a int
            b string
            c []float32
            d func() bool
            e struct {
                x int
            }
        )
        ```
    - 简短格式```i, j := 0, 1```
        - 定义变量，同时显式初始化。
        - 不能提供数据类型。
        - 只能用在函数内部。


- 变量的初始化
    - 如`var hp int = 100`,其中`int`可以省略。
    - 极简的写法:`hp := 100`即推导声明的写法。
        - 如果声明过，再使用这种方法会报错`no new variables on left side of :=`
        - 在多个短变量声明和赋值中，至少有一个新声明的变量出现在左值中，即便其他变量名可能是重复声明的，编译器也不会报错。如
            ```go
            conn, err := net.Dial("tcp", "127.0.0.1:8080")
            conn2, err := net.Dial("tcp", "127.0.0.1:8080")
            ```

- 多变量同时赋值
    - 交换变量（“多重赋值”特性）
        ```go
        var a int = 100
        var b int = 200
        b, a = a, b
        fmt.Println(a, b)
        ```
- 匿名变量：下划线“_”
    - 如
        ```go
        func GetData() (int, int) {
            return 100, 200
        }
        func main(){
            a, _ := GetData()
            _, b := GetData()
            fmt.Println(a, b)
        }
        ```
- 变量作用域(没什么好讲的，跟`python`类似)
    - 局部变量：`Go`语言程序中全局变量与局部变量名称可以相同，但是函数体内的局部变量会被优先考虑。
    - 全局变量：全局变量声明必须以 var 关键字开头
    - 形式参数


- 浮点类型
    - 常量 `math.MaxFloat32` 表示 `float32` 能取到的最大数值，大约是 3.4e38。
    - 常量 `math.MaxFloat64` 表示 `float64` 能取到的最大数值，大约是 1.8e308。
    - `float32` 和 `float64` 能表示的最小值分别为 1.4e-45 和 4.9e-324。
    - `float32`精度不够，如：
        ```go
        var f float32 = 16777216 // 1 << 24
        fmt.Println(f == f+1)    // "true"!    
        ```
    - && 对应逻辑乘法，|| 对应逻辑加法，乘法比加法优先级要高
    
    
- 字符串（一个不可改变的字节序列）
    - 字符串的内容通过标准索引法来获取
        - 字符串 str 的第 1 个字节：`str[0]`
        - 第 i 个字节：`str[i - 1]`
        - 最后 1 个字节：`str[len(str)-1]`
    - 定义多行字符串用 \`
        ```go
        const str = `第一行
        第二行
        第三行
        \r\n
        `    
        ```


- 字符
    - 一种是 `uint8` 类型，或者叫 `byte` 型，代表了 `ASCII` 码的一个字符
    - 另一种是 `rune` 类型，代表一个 `UTF-8` 字符，当需要处理中文、日文或者其他复合字符时，则需要用到 `rune` 类型。`rune` 类型等价于 `int32` 类型。
        ```go
        var ch byte = 65 或 var ch byte = '\x41'      //（\x 总是紧跟着长度为 2 的 16 进制数）    
        ```
    - UTF-8 和 Unicode 有何区别？
        - 狭义的Unicode是一种字符集。
        - UTF-8 是编码规则，将 Unicode 中字符的 ID 以某种方式进行编码，UTF-8 的是一种变长编码规则，从 1 到 4 个字节不等。
        - 广义的 Unicode 指的是一个标准，它定义了字符集及编码规则，即 Unicode 字符集和 UTF-8、UTF-16 编码等。
  
        
- 指针
    - **类型指针**：允许对这个指针类型的数据进行修改，传递数据可以直接使用指针，而无须拷贝数据，类型指针不能进行偏移和运算。
    - **切片**：由指向起始元素的原始指针、元素数量和容量组成。
    - `ptr := &v    // v 的类型为 T`
    - 从指针获取指针指向的值，对指针使用`*`操作符。
    - **使用指针修改值**
        ```go
        // 交换函数
        func swap(a, b *int) {
            // 取a指针的值, 赋给临时变量t
            t := *a
            // 取b指针的值, 赋给a指针指向的变量
            *a = *b
            // 将a指针的值赋给b指针指向的变量
            *b = t
        }
        func main() {
        // 准备两个变量, 赋值1和2
            x, y := 1, 2
            // 交换变量值
            swap(&x, &y)
            // 输出变量值
            fmt.Println(x, y)
        }    
        ```
    - `*`操作符作为右值时，意义是取指针的值，作为左值时，也就是放在赋值操作符的左边时，表示 `a` 指针指向的变量。
    
    
- 常量（`const`）
    - 必须在声明时就确定数值，即不可获取函数返回的值之类。
    - `iota`常量生成器，类似枚举
        ```go
        type Weekday int
        const (
            Sunday Weekday = iota
            Monday
            Tuesday
            Wednesday
            Thursday
            Friday
            Saturday
        )
        // 分别是周日为0，周一为1
        ```
    
    

- `type`关键字（类型别名）
    - `Go 1.9` 版本之前的内建类型
        - `type byte uint8`
        - `type rune int32`
    - `Go 1.9` 版本之后
        - `type byte = uint8`
        - `type rune = int32`
    - 类型别名与类型定义，如
        ```go
        // 将NewInt定义为int类型
        type NewInt int
        // 将int取一个别名叫IntAlias
        type IntAlias = int
        func main() {
        // 将a声明为NewInt类型
        var a NewInt
        // 查看a的类型名
        fmt.Printf("a type: %T\n", a)
        // 将a2声明为IntAlias类型
        var a2 IntAlias
        // 查看a2的类型名
        fmt.Printf("a2 type: %T\n", a2)
        }    
        ```
        - 结果显示 `a` 的类型是 `main.NewInt`，表示 `main` 包下定义的 `NewInt` 类型，`a2` 类型是 `int`，`IntAlias` 类型只会在代码中存在，编译完成时，不会有 `IntAlias` 类型。
        
        
- 关键字

    <table align="center">
        <tbody align="center">
            <tr>
                <td>
                    break</td>
                <td>
                    default&nbsp;</td>
                <td>
                    func</td>
                <td>
                    interface</td>
                <td>
                    select</td>
            </tr>
            <tr>
                <td>
                    case</td>
                <td>
                    defer</td>
                <td>
                    go</td>
                <td>
                    map</td>
                <td>
                    struct</td>
            </tr>
            <tr>
                <td>
                    chan</td>
                <td>
                    else</td>
                <td>
                    goto</td>
                <td>
                    package</td>
                <td>
                    switch</td>
            </tr>
            <tr>
                <td>
                    const</td>
                <td>
                    fallthrough</td>
                <td>
                    if</td>
                <td>
                    range</td>
                <td>
                    type</td>
            </tr>
            <tr>
                <td>
                    continue</td>
                <td>
                    for</td>
                <td>
                    import</td>
                <td>
                    return</td>
                <td>
                    var</td>
            </tr>
        </tbody>
    </table>      
    
    - 为变量、函数、常量命名时采用驼峰命名法，例如 stuName、getVal；





- 运算优先级

    <table>
    <tr>
        <th>
            优先级</th>
        <th>
            分类</th>
        <th>
            运算符</th>
        <th>
            结合性</th>
    </tr>
    <tr>
        <td>
            1</td>
        <td>
            逗号运算符</td>
        <td>
            ,</td>
        <td>
            从左到右</td>
    </tr>
    <tr>
        <td>
            2</td>
        <td>
            赋值运算符</td>
        <td>
            =、+=、-=、*=、/=、 %=、 &gt;=、 &lt;&lt;=、&amp;=、^=、|=</td>
        <td>
            <span style="color:#b22222;">从右到左</span></td>
    </tr>
    <tr>
        <td>
            3</td>
        <td>
            逻辑或</td>
        <td>
            ||</td>
        <td>
            从左到右</td>
    </tr>
    <tr>
        <td>
            4</td>
        <td>
            逻辑与</td>
        <td>
            &amp;&amp;</td>
        <td>
            从左到右</td>
    </tr>
    <tr>
        <td>
            5</td>
        <td>
            按位或</td>
        <td>
            |</td>
        <td>
            从左到右</td>
    </tr>
    <tr>
        <td>
            6</td>
        <td>
            按位异或</td>
        <td>
            ^</td>
        <td>
            从左到右</td>
    </tr>
    <tr>
        <td>
            7</td>
        <td>
            按位与</td>
        <td>
            &amp;</td>
        <td>
            从左到右</td>
    </tr>
    <tr>
        <td>
            8</td>
        <td>
            相等/不等</td>
        <td>
            ==、!=</td>
        <td>
            从左到右</td>
    </tr>
    <tr>
        <td>
            9</td>
        <td>
            关系运算符</td>
        <td>
            &lt;、&lt;=、&gt;、&gt;=</td>
        <td>
            从左到右</td>
    </tr>
    <tr>
        <td>
            10</td>
        <td>
            位移运算符</td>
        <td>
            &lt;&lt;、&gt;&gt;</td>
        <td>
            从左到右</td>
    </tr>
    <tr>
        <td>
            11</td>
        <td>
            加法/减法</td>
        <td>
            +、-</td>
        <td>
            从左到右</td>
    </tr>
    <tr>
        <td>
            12</td>
        <td>
            乘法/除法/取余</td>
        <td>
            *（乘号）、/、%</td>
        <td>
            从左到右</td>
    </tr>
    <tr>
        <td>
            13</td>
        <td>
            单目运算符</td>
        <td>
            !、*（指针）、&amp; 、++、--、+（正号）、-（负号）</td>
        <td>
            <span style="color:#b22222;">从右到左</span></td>
    </tr>
    <tr>
        <td>
            14</td>
        <td>
            后缀运算符</td>
        <td>
            ( )、[ ]、-&gt;</td>
        <td>
            从左到右</td>
    </tr>
    </table>

## 1️⃣语言容器(container）
> 一般情况下具有各种形式的存储和处理数据的功能

</font>
