# 请求中间件

了解如何拦截 SDK 发起的请求，并统一添加额外的逻辑。

UCloud SDK 为 Client 或 Transport 级别的请求提供了请求中间件的特性。

该特性允许在 请求/响应 的生命周期中添加自定义的逻辑。

## 自定义行为中间件

通过定义 Client 级别的中间件，可以拦截 Action 的请求和响应:

```go
// 定义一个 Handler，打印客户端信息 MetaData
func printClientInfo(c *ucloud.Client, req request.Common, resp response.Common, err error) (response.Common, error) {
	fmt.Println("[INFO] Product: ", c.GetMeta().Product)

	return resp, err
}

uhostClient := uhost.NewClient(cfg, cred)
_ = uhostClient.AddResponseHandler(printClientInfo)
```

在这个例子中，每次请求的生命周期都会经过自定义 Handler，打印出产品名称。

## 自定义网络中间件

通过定义 Transport 级别的中间件，可以拦截 HTTP 的网络请求和响应:

```go
// 定义一个 Handler，打印 HTTP 状态码
func logHTTPStatusCode(c *Client, req *http.HttpRequest, resp *http.HttpResponse, err error) (*http.HttpResponse, error) {
	if resp != nil {
		fmt.Println("[INFO] Product: ", resp.GetStatusCode())
	}
	return resp, err
}

uhostClient := uhost.NewClient(cfg, cred)
_ = uhostClient.AddHttpResponseHandler(logHTTPStatusCode)
```

在这个例子中，每次请求的生命周期都会经过自定义 Handler，打印出 HTTP 状态码。
