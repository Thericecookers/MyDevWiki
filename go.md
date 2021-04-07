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