# 第八章 反射
反射，即再运行时更新变量和检查自身的值、调用方法等，编译时并不知道这些变量的类型。
反射特性给编程语言增加了表达力，也可以让我们把类型作为第一类的值类型处理。
相关且重要的两个包：fmt包的字符串格式化功能；encoding/json、encoding/xml等特定协议的编解码功能。
text/template、html/template也是依赖反射试下。
反射是内省技术，不应该随意使用。上文的包是用反射技术实现，但是他们自己的api都没有公开反射相关的接口。

## 1. 为何需要反射
为解决处理一类{不满足普通公共接口的类型的值}(面向扩展编程，当前的未来的各种情况)的函数编写需求。
如，fmt.Fprintf字符串格式化处理。