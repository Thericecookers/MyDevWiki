## switch关键字  
 go 不写break，如果有fallthrough则会执行后续case
```go
switch cnt{  
    case 1:
        fmt.Println("1")
        fallthrough
    case 2:
        fmt.Println("2")
 }
```

## 短声明
可以在for循环中使用
```go
var count =0
for count=10;count>0;count--{
}
```
```go
for count:=10;count>0;count--{
}
```
* var不能放在for/if/switch
* **短声明不能够用于包级别变量**

## 浮点类型
* 格式化浮点数
```go
fmt.Printf("%4.2f\n",dec)
```
* 先做乘法再做除法比较有利于精度
* 比较浮点型
```go
math.Abs(dec-0.3)<0.0001
```

## 整型
* 十六进制的表示：
```go
var red=0x00
fmt.Printf("%x",red)
```
* 如何打印比特
```go
fmt.Printf("%b",red)
```
* 时间戳记得要用int64,否则到2038年会溢出

## 大数
* 超过18e18的数可以用：big.Int
```go
distance:=new(big.Int)
distance.SetString("400000000000000000000000",10)//param2代表进制
```
* 分数：big.rat
* 用了big.Int那么公式中其它数也需要是big.Int
* 常量大数可以直接使用于编译阶段（运行期不行），因为存在untype类型常量，原因是**GO编译器使用了big包处理untype Int**

## 字符串
* 如果用`符号，那么转义符无效
* rune类型是Int32类型的别名(UNICODE)
* byte是uint8类型别名（ASC2）
* code point打印值用%v,打印字符用%c
* len()函数忽视了字符可能是8，16，32位的情况，应该用utf8.RuneCountInString()计算长度

## 类型转换
* 浮点转整型是直接发生截断而不是近似
* 数值转化为string:
```go
strconv.Itoa()
str:=fmt.Sprintf()//Sprintf会返回一个字符串
```
* string转化为数值
```go
countdown,err:=strconv.Atoi("10")
if err!=nil{
    //oh no something went wrong
}
fmt.Println(countdown)
```
* go是静态类型语言
* go里面不能将1和0当作true和false

## 函数
* go中大写字母开头函数，变量或其它标识符类似于public，可以被其他包导出，所以调用别的包的函数基本都是大写开头
* 形参 parameter
* 实参 argument
* 可变参数函数
```go
//例如Println
func Println(a ...interface{})(int error)
//...代表参数数量可变
//interface{}代表空接口，可以由任意参数实现
```
* 函数是一等的，可以赋给变量，作为实参
* 匿名函数是可行的：
```go
func() {
    fmt.Println("立即执行")
}()//如果加了括号则立即执行
```
*闭包（closure）就是由于匿名函数封闭并包围作用域中的变量而得名:
```go
func calibrate(offset kelvin) sensor {
    return func() kelvin{
        return s()+offset
    }
}

func main(){
    offset := 5
    sensor :=calibrate(offset)
    //虽然函数返回了，但是sensor仍然能访问offset参数
    //闭包相当于利用形参访问实参，由链式作用域，一层层找到实际调用的参数
}
```

## 类型/方法
* go里面提供了方法但是没有提供类与对象（但是存在类型type）
* 方法可以与同包中任意类型关联
```go
type kelvin float64
type celsius float64
func (k kelvin) celsius() celsius{
    return celsius(k-273.15)
}//k是类型参数的接收者
```
* 方法只能有唯一的接收者
![avatar](./picture/go/method_for_type.jpg)

## 数组
* 没有被赋值的元素为对应类型的零值
* 复合字面值初始化
```go
dwarfs := [2]string{"Cad","der"}
dwarfs := [...]string{"Cad","der"}//自动推断长度
```
* 数组赋值给新的变量是deep copy
* 数组长度也是数组类型的一部分，因此函数一般用**切片**而不使用**数组**作为参数

## 切片
* （反常规）切分字符串时，索引代表的是字节数而不是rune数（因此在存在非英文时需要注意）
* 切片可以由两种方式声明
```go
dwarfSlice:=dwarfArray[:]//由已知数组切片
dwarfSlice:=[]string{"dsaf","fdf"}//新建数组同时切片所有元素
```
* []是切片，[...]或者[num]是数组
* 在切片上进行append操作很有可能会改变底层数组（有一些例外情况，例如切片的容量已经满了）因此可以限制切片容量slice[0:3:3]
* make函数可以预分配容量给slice，提升效率
```go
dwarfs:=make([]string,0,10)//string切片，长度0，容量10
dwarfs:=make([]string,10)//string切片，长度10，容量10
```
* 针对可变长度参数，切片需要展开
```go
func terraform(prefix string,words ...string)//words其实就是string切片
planets:=[]string{"sdf","dff"}
terraform("New",planets...)//三个点代表展开切片
```
* 传可变参数是浅拷贝过程

## map
*判断键是否存在
```go
if moon,ok:=dict["Moon"];ok{
    fmt.Printf("key exist")
}else{
    fmt.Printf(key does not exist)
}
```
* map在赋值的时候是浅拷贝（与数组，整型等不同）
* map可以用复合字面值与make函数初始化

# 方法转发
* struct可以嵌入，省略字段名
```go
type report struct{
    sol int//组合
    temperature//嵌入
    location//嵌入
}
```
* 嵌入字段类型会自动生成与类型名一样的字段
* 嵌入后可以直接使用temperature,location的方法
* 嵌入有可能造成重复命名问题，需要明确调用哪个嵌入字段的方法

# 继承还是组合
* 优先使用组合而不是对象
* 传统继承均可用组合实现