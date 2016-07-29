# 光速达开放平台API
光速达智能云开放平台API

## 概述


## 接口版本
| 版本    | 更新内容| 发布人 |  发布时间 |
|--------|-------|----|-------|
| V0.1 | 初稿待确认|  光速达 | 2016-05-10  |



## 我的叮咚接口列表  

### 获取设备列表


+ Parameters
    + timestamp : `1463535207898` (number) - 请求提交的当前时间时间戳
    + appId : `123456` (string) - 第三方用户唯一凭证
    + pageIndex : `0` (number) - 分页页数,从0开始
    + pageSize : `20` (number) - 每页条数


+ Request (application/json)

        {
            "sign": "0a20d32eda37038e2287ccf74b49e0f4",
            "timestamp": 1463535207898,
            "lastSyncDate": "2016-04-19T02:06:57.931Z",
            "appId": "123456",
            "nonstr": "aaabbb",
            "pageSize": 20,
            "deviceId": "GTS0001",
            "pageIndex": 0
        }
        
+ Response 200 (application/json)

        {
            "total": 6,
            "count": 2,
            "data": [{
                "startTime": "2016-05-10T16:00:00.000Z",
                "endTime": "2016-12-31T16:00:00.000Z",
                "mId": "12354abdd",
                "mobile": "18905029911",
                "deviceId": "GTS0001",
                "createdAt": "2016-05-26T11:44:08.509Z",
                "updatedAt": "2016-05-26T11:45:04.861Z",
                "gtsNo": "1212",
                "communityCode": "3501240001",
                "communityName": "光速达体验社区"
            }, {
                "startTime": "2016-05-26T11:43:40.368Z",
                "endTime": "2016-06-06T18:40:53.000Z",
                "mId": "/5l7xT8jOkrp2WfmiiIujDk5ODQ0MjMxMDd4Y3ZZT0Q=ecosAmJsIYcE2GWOFwpXxjBFSlhadmlHNWl6RHJlSU8=",
                "mobile": "18950333962",
                "deviceId": "GTS0001",
                "createdAt": "2016-05-26T11:43:42.421Z",
                "updatedAt": "2016-05-26T11:45:33.639Z",
                "gtsNo": "12313123",
                "communityCode": "3501240001",
                "communityName": ""
            }]
        }

 

### 获取道闸通行记录（建议使用） [POST]
