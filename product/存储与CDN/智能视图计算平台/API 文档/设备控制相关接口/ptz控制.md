## 功能描述

用于设备通道云台控制，包括转动、变倍、变焦、光圈等。

## 请求

#### 请求url

> POST /ivc/cms/control/ptz

#### 请求参数

此接口无请求参数。

#### 请求头

此接口仅使用公共请求头部，详情请参见 [公共请求头部](https://cloud.tencent.com/document/product/1344/50451) 文档。

#### 请求体

该请求操作的实现需要有如下请求体。

```json
{
   "ChannelId": "0022c12a-e220-42e0-975f-800f872fc89e",
   "Type": "right",
   "Speed": 3
}
```

| 字段名    | 类型   | 描述     | 是否必须 | 备注                                                         |
| :-------- | :----- | :------- | :------- | :----------------------------------------------------------- |
| ChannelId | string | 通道id   | 必须     |                                                              |
| Type      | string | 命令类型 | 必须     | 上:up,下:down,左:left,右:right,上左:leftup,上右:rightup,下左:leftdown,下右:rightdown,放大:zoomin,缩小:zoomout,聚焦远:focusfar,聚焦近:focusnear,光圈放大:irisin:,光圈缩小:irisout |
| Speed     | int    | 命令描述 | 必须     | 速度值范围1-8，超出或不填默认为4                             |

## 响应

#### 响应头

此接口仅返回公共响应头部，详情请参见 [公共响应头部](https://cloud.tencent.com/document/product/1344/50452) 文档。

#### 响应体

该响应体返回为 **application/json** 数据，包含完整节点数据的内容展示如下：

```json
{
   "RequestId": "",
   "Code": 0,
   "StatusCode": 200,
   "Message": "ok",
   "Data": null
}
```

| 字段名     | 类型   | 描述                             | 备注 |
| :--------- | :----- | :------------------------------- | :--- |
| RequestId  | string | 请求id                           |      |
| Code       | int    | 状态码，0 成功，500 操作失败     |      |
| StatusCode | int    | 错误码，200 OK，其他详见错误中心 |      |
| Message    | string | 返回消息                         |      |
| Data       | object | 返回结果                         |      |

