# 软件的版本号该怎么定？
[主板本号].[子版本号][.修正版本号[.编译版本号]]
1.2.1，2.0，5.0.0 build-13124

[主板本号].[子版本号][.编译版本号[.修正版本号]]

需要考虑的问题：
+ 1.是否向后兼容？  
具有相同名称但不同主板本号的程序集不可互换。产品大量重写，导致无法实现向后兼容。  
程序集和版本号相同，次版本号不同。功能显著增强，但照顾到了向后兼容性。  
内部版本号不同，标识对相同源所作的重新编译。  
+ 2.是否有功能增强.
+ 3.是否有缺陷、漏洞修复  
局部修改或bug修正

参见：[版本号语义化](https://semver.org/lang/zh-CN/spec/v2.0.0.html)
