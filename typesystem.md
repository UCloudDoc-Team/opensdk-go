# 类型系统

由于 Golang 不区分 零值 和 空值，所以 Go SDK 的简单类型采用包装类型的方式来设置参数：

```go
req := client.NewCreateUHostInstanceRequest()

req.Password = ucloud.String("xxx")
req.LoginMode = ucloud.String("Password")
req.Region = ucloud.String("cn-bj2")
req.Zone = ucloud.String("cn-bj2-04")
req.ImageId = ucloud.String("uimage-xxxx")
req.CPU = ucloud.Int(1)
req.Memory = ucloud.Int(1024)
```

数组和结构体等类型与 Go 原生类型系统保持一致。

