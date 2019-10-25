

# 工具箱

## 等待资源状态

**什么时候应该使用它?**

等待函数可以等待远程状态达到某一特定的状态，比如：

- 创建并等待资源创建完成
- 调用主机开关机并等待开机或关机完成
- 自定义等待的时间策略

例如:

```go
waiter.StateWaiter{
    Pending: []string{"pending"},
    Target:  []string{"available"},
    Refresh: CustomRefreshFn,
    Timeout: 5 * time.Minute,
}
```

你可以定义自己的状态刷新函数，函数签名如下:

```go
type RefreshFunc func() (result interface{}, state string, err error)
```

例如:

```go
func CustomRefreshFn() (interface{}, string, error) {
    inst, err := describeUHostByID(uhostID)
    if err != nil {
        return nil, "", err
    }

    if inst == nil || inst.State != "Running" {
        return nil, "pending", nil
    }

    return inst, "available", nil
}
```

## 高阶选项

### 超时

Timeout option will wait and refresh until target state or timeout is reached. State waiter use exponential back-off interval.

超时选项用于以指数退避间隔不断刷新状态，直至达到超时时间。 

注意，超时时间是必选参数。默认情况下，状态的最大刷新间隔是 10s，如果没有配置其它选项，刷新的间隔类似于：

```
0 100ms 200ms 400ms 800ms 1.6s 3.2s 6.4s 10s 10s
```

如果最小刷新间隔 ``MinTimeout`` 被设置，刷新间隔将以一个最小刷新时间开始：

```
0 3s 6s 10s 10s ... (100ms < MinTimeout <= 10s, eg. 3s)

0 11s 10s 10s 10s ... (10s < MinTimeout, eg. 11s)
```

如果轮询间隔 ``PollInterval`` 被设置，指数退避的间隔将不适用，等待函数会简单地使用固定间隔来刷新：

```
0 3s 3s 3s 3s ... (PollInterval, eg. 3s)
```

### 延迟

延迟选项用于在第一次请求前，等待特定的时间.

```go
waiter.StateWaiter{
    Pending: []string{"pending"},
    Target:  []string{"available"},
    Refresh: CustomRefreshFn,
    Timeout: 5 * time.Minute,
    Delay: 30 * time.Second,
}
```

该选项通常用于在资源创建后等待一小段时间，再开始刷新状态.

## Example

可以参考 [官方示例](https://github.com/ucloud/ucloud-sdk-go/tree/master/examples/wait) 获取完整代码。
