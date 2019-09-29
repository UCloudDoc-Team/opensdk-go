{{indexmenu_n>3}}

# 错误处理

## 基本用法

对于 SDK 抛出的错误，最典型的用法是直接序列化为字符串打印出来：

```go
if err != nil {
    fmt.Printf("got error: %s", err)
}
```

对于 API 返回 RetCode 大于 0 的错误，可以通过以下方法简单地判断出来：

```go
if uerr.IsCodeError(err) {
    fmt.Printf("%v %s", e.Code(), e.Message())
} else {
    fmt.Printf("%s", e)
}
```

## 高阶用法

如果需要对于 SDK 错误有更细粒度的要求，可以是有类型推导来检查错误，例如：

```go
import (
    uerr "github.com/ucloud/ucloud-sdk-go/ucloud/error"
)

if err != nil {
    switch err.(type) {
    case uerr.(ClientError):
        fmt.Printf("client error: %s", err)
    case uerr.(ServerError):
        fmt.Printf("server error: %s", err)
    }
}
```

或者直接枚举错误的名称来分别处理：

```go
import (
    uerr "github.com/ucloud/ucloud-sdk-go/ucloud/error"
)

if e, ok := err.(uerr.Error); err != nil && ok {
    switch e.Name() {
    case uerr.ErrRetCode:
        fmt.Printf("%v %s", e.Code(), e.Message())
    default:
        fmt.Printf("%s", e)
    }
}
```
