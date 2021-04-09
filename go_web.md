## 创建HTTP Server
* 方法一：使用http.ListenAndServe(param1,param2)
```go
http.ListenAndServe("localhost:8080",nil)
//param1代表网络地址，如果是""，那么就是所有网络接口的8080端口
//第二个参数是handler,如果是nil，那么就是DefaultServeMux(路由器)
* 方法二：建立server对象
```
```go
server:=http.Server{
    Addr:"localhost:8080",
    Handler:nil,
}
server.ListenAndServe()
```
* Handler是一个Interface,handle定义了一个方法ServeHttp(p1 ResponseWriter,p2 *Request)
> p1是HTTPResponseWriter:响应
> p2是指向Request的指针

## 创建HTTPS服务
* 上述方法无法建立HTTPS（无法添加SSL(TLS)）

