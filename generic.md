

# 泛化调用

如何调用 SDK 尚未支持的 API ？可以使用泛化调用方式。

**NOTE** 如果没有必须使用的理由，不建议使用泛化方式调用 API，因为无法享受 OpenAPI 提供的兼容性保证。

## 调用方式

示例如下: 使用泛化调用的方式调用 `GetMetric` 接口，获取主机近一个小时的 CPU 使用率; 详细使用示例可以参考 [官方示例](https://github.com/ucloud/ucloud-sdk-go/tree/master/examples/generic)。
```go
package main

import (
	"fmt"

	"github.com/ucloud/ucloud-sdk-go/ucloud"
	"github.com/ucloud/ucloud-sdk-go/ucloud/auth"
)

func main() {
	cfg := ucloud.NewConfig()
	// replace the public/private key by your own
	credential := auth.NewCredential()
	credential.PublicKey = "my_public_key"
	credential.PrivateKey = "my_private_key"

	client := ucloud.NewClient(&cfg, &credential)
	req := client.NewGenericRequest()
	err := req.SetPayload(map[string]interface{}{
		"Action":       "GetMetric",
		"Region":       "cn-bj2",
		"ResourceType": "uhost",
		"ResourceId":   "uhost-xxx",
		"TimeRange":    3600,
		"MetricName":   []string{"CPUUtilization"},
	})
	if err != nil {
		panic(err)
	}
	resp, err := client.GenericInvoke(req)
	if err != nil {
		panic(err)
	}
	fmt.Println(resp.GetPayload())
}
```
