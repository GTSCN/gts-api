FORMAT: 1A
HOST: http://gtsc.leanapp.cn/

# gts-api
光速达智能云


# 光速达智能云API接口使用说明

## 概述
本文档用于说明私有云调用光速达智能云接口方法，主要分三部分进行相关内容定义和介绍。
- 第一部分概述，主要介绍本文档结构和主要内容。
- 第二部分主要介绍光速达智能云HTTP连接请求和安全策略，对HTTP请求格式、应答格式、签名算法等做了详细介绍，并列举实例进行相关说明。
- 第三部分主要介绍光速达智能云平台开放接口规范细则，从接口功能、定义和范例三方面对接口做了详细规定并给出使用范例。最后对有关专业用语做了名词解释，便于理解文档内容。

### 开发者规范
开发者进行开发时，除了需要满足每个接口的规范限制、调用频率限制外，还需特别注意设备控制消息、用户数据等敏感信息的使用规范。

涉及用户数据时：

- 您的服务需要收集用户任何数据的，必须事先获得用户的明确同意，且仅应当收集为运营及功能实现目的而必要的用户数据， 同时应当告知用户相关数据收集的目的、范围及使用方式等，保障用户知情权。
- 您收集用户的数据后，必须采取必要的保护措施，防止用户数据被盗、泄漏等。
- 您在特收集的用户数据仅可以在该特定应用中使用，不得将其使用在该特定应用之外或为其他任何目的进行使用，也不得以任何方式将其提供给他人。
- 如果光速达认为您收集、使用用户数据的方式，可能损害用户体验，光速达有权要求您删除相关数据并不得再以该方式收集、使用用户数据。
- 一旦您停止使用本服务，或光速达基于任何原因终止您使用本服务，您必须立即删除全部因使用本服务而获得的数据（包括各种备份）， 且不得再以任何方式进行使用。

其他规范：

- 请勿为任何用户自动登录到光速达云平台提供代理身份验证凭据。
- 请勿提供跟踪功能，包括但不限于识别其他用户在个人主页上查看、点击等操作行为。
- 请勿自动将浏览器窗口定向到其他网页。
- 请勿设置或发布任何违反相关法规、公序良俗、社会公德等的玩法、内容等。
- 请勿公开表达或暗示，您与光速达之间存在合作关系，包括但不限于相互持股、商业往来或合作关系等，或声称光速达对您的认可。
- 完整的开发者规范和接口限制，请详见开发者接口文档，以及光速达微信公众平台开发者协议。

### 接口权限说明

### 接口调用频率限制说明

### 接口返回码说明


## 光速达智能云HTTP连接请求和安全策略
### HTTP连接请求
用户通过HTTP请求方式，从私有云调用光速达智能云开放接口，实现获取设备列表、查询报警信息、获取视频广场栏目和展示视频列表等功能。

#### HTTP请求数据格式规范

#### HTTP应答数据格式规范

#### 服务端返回值定义

### HTTP连接安全策略
#### 签名算法

为了防止API调用过程中被黑客恶意篡改，调用任何一个API都需要携带签名，光速达智慧云服务端会根据请求参数，对签名进行验证，签名不合法的请求将会被拒绝。光速达智慧云目前支持的签名算法只支持MD5(sign_method=md5) ，签名大体过程如下：

将待签名参数（包括API的公共参数和业务参数，除去sign参数和byte[]类型的参数）使用英文冒号拼接参数的key和value，根据拼接后的字符串的ASCII码表的顺序排序。如：foo:1, bar:2, foo_bar:3, foobar:4排序后的顺序是bar:2, foo:1, foo_bar:3, foobar:4。
将排序好的字符串用英文逗号拼装在一起，在最后加上secret组成待签名字符串，根据上面的示例得到的结果为：bar:2,foo:1,foo_bar:3,foobar:4,secret:123456。
把待签名字符串使用MD5算法进行摘要，如：md5(bar:2,foo:1,foo_bar:3,foobar:4,secret=123456),将摘要得到的字节流结果输出为十六进制字符串。
说明：MD5是128位长度的摘要算法，用16进制表示，一个十六进制的字符能表示4个位，所以签名后的字符串长度固定为32个十六进制字符。

#### 签名范例

以人闸接口调用为例，具体步骤如下：

1. 设置参数值

 公共参数：
```
appId=123456 //第三方用户唯一凭证
timestamp=1463535207898  //当前时间戳
nonstr=aaabbb  //随机字符串
```

 业务参数：
```
deviceId=GTS0001
pageSize=20
pageIndex=1
```

2. 用冒号拼接并按ASCII顺序排序
```
appId:123456
deviceId:GTS0001
nonstr:aaabbb
pageIndex:1
pageSize:20
timestamp:1463535207898
```
3. 拼接参数名与参数值
toSignStr=appId:123456,deviceId:GTS0001,nonstr:aaabbb,pageIndex:1,pageSize:20,timestamp:1463535207898

4. 生成签名
假设app的secret为123456，则签名结果为：hex(md5(toSignStr + ",secret:123456"))=bb4b98e26c9b916f74ada1137b7fc5aa

5. 组装HTTP请求
将所有参数名和参数值采用utf-8进行URL编码（参数顺序可随意，但必须要包括签名参数），然后通过GET或POST（含byte[]类型参数）发起请求，POST请求也可以使用application/json。

__URL示例：__
```
http://gtsc.leanapp.cn/gate/?appId=123456&deviceId=GTS0001&nonstr=aaabbb&pageIndex=1&pageSize=20&timestamp=1463535207898&sign=bb4b98e26c9b916f74ada1137b7fc5aa
```

- 注意事项

所有的请求和响应数据编码皆为utf-8格式，URL里的所有参数名和参数值请做URL编码。如果请求的Content-Type是application/x-www-form-urlencoded，则HTTP Body体里的所有参数值也做URL编码；如果是multipart/form-data格式，每个表单字段的参数值无需编码,但每个表单字段的charset部分需要指定为utf-8。
参数名与参数值拼装起来的URL长度小于1024个字符时，可以用GET发起请求；参数类型含byte[]类型或拼装好的请求URL过长时，必须用POST发起请求。所有API都可以用POST发起请求。
生成签名（sign）仅对未使用TOP官方SDK进行API调用时需要操作，如使用了TOP官方SDK，该步骤SDK会自动完成。


## 光速达智能云平台开放接口列表

### 光速号API

### 商城平台API

### 智慧社区API

### 智慧酒店API

### 交易中心API


## 参考资料

http://open.taobao.com/doc2/detail.htm?articleId=101617&docType=1&treeId=1

## 示例 [/gate/?appId={appId}&deviceId={deviceId}&timestamp={timestamp}&nonstr={nonstr}&pageSize={pageSize}&pageIndex={pageIndex}&sign={sign}]

### 获取人闸记录 [GET]

获取人闸记录。

+ Parameters
    + timestamp (number) - 请求提交的当前时间时间戳
    + nonstr (string) - 当前加密签名使用的随机字符串
    + deviceId (string) - 设备对应的ID号
    + sign (string) - 签名算法为MD5（signStr + secret）
    + appId (string) - 第三方用户唯一凭证
    + pageIndex (number) - 分页页数
    + pageSize (number) - 每页条数

+ Response 200 (application/json)

        {
            "total": 1502,
            "count": 2,
            "data": [{
                "startTime": {
                    "__type": "Date",
                    "iso": "2016-04-30T16:00:00.000Z"
                },
                "deviceInfo": {
                    "__type": "Pointer",
                    "className": "GateInfo",
                    "objectId": "5731cef41ea4930064fb8121"
                },
                "deviceId": "GTS0001",
                "endTime": {
                    "__type": "Date",
                    "iso": "2017-01-31T16:00:00.000Z"
                },
                "mobileIdentification": "EO9EBF0MSHC1NJJDQFLFP9ANIX9HGAUB",
                "mobile": "08081009592",
                "objectId": "573acce90a1aa41f53bf1944",
                "createdAt": "2016-05-17T07:48:57.375Z",
                "updatedAt": "2016-05-17T07:48:57.375Z"
            }, {
                "startTime": {
                    "__type": "Date",
                    "iso": "2016-04-30T16:00:00.000Z"
                },
                "deviceInfo": {
                    "__type": "Pointer",
                    "className": "GateInfo",
                    "objectId": "5731cef41ea4930064fb8121"
                },
                "deviceId": "GTS0001",
                "endTime": {
                    "__type": "Date",
                    "iso": "2017-01-31T16:00:00.000Z"
                },
                "mobileIdentification": "80V XP1AA1UHHB3P TVMORLQ8BMD 0OX",
                "mobile": "45316706237",
                "objectId": "573acce90a1aa41f53bf1945",
                "createdAt": "2016-05-17T07:48:57.384Z",
                "updatedAt": "2016-05-17T07:48:57.384Z"
            }]
        }

+ Response 503 (application/json)

        {
            "errCode": 503,
            "errMsg": "非法请求，签名内容未通过验证"
        }

