# 接口文档



# 开发前必读/认证授权



> i人事的OPENAPI认证授权采用标准oauth2客户端授权方式。认证授权的过程就是获取`access_token`的过程。
> `access_token`是第三方系统调用i人事接口的调用凭据，第三方应用调用各接口时都需使用`access_token`。开发者需要进行妥善保存`access_token`， `access_token`为36位字符。

### 接口描述：

> 获取访问OPENAPI的认证凭证(获取`access_token`)。第三方应用可以使用AppID和AppSecret调用本接口来获取`access_token`。

接口定义：获取接口请求`access_token`

### 接口地址:

```http
https://openapi.ihr360.com/openapi/oauth/token?grant_type=client_credentials&scope=client
```

### 请求方式: POST

### 数据格式:

```
Content-Type:application/json;charset=UTF-8;Authorization: Basic 认证串
```

**认证串**的生成方式

```java
Base64Encode(AppID:AppSecret)
```

| 键              | 值                         |
| --------------- | -------------------------- |
| AppID           | aBcDeF                     |
| AppSecret       | A1B2C3                     |
| AppID:AppSecret | aBcDeF:A1B2C3              |
| Base64编码结果  | YUJjRGVGOkExQjJDMw==       |
| Authorization   | Basic YUJjRGVGOkExQjJDMw== |

完整请求头信息为

```
Content-Type:application/json;charset=UTF-8;Authorization: Basic YUJjRGVGOkExQjJDMw==
```

### 请求参数说明:

| 参数         | 位置  | 类型   | 必填 | 限制 | 备注                                                         |                                                             |
| ------------ | ----- | ------ | ---- | ---- | ------------------------------------------------------------ | ----------------------------------------------------------- |
| `grant_type` | query | String |      | 是   | 固定值                                                       | 标志授权方式，目前支持`client_credentials`、`refresh_token` |
| `scope`      | query | String |      | 是   | `client`：全部范围 `administration`：行政 `attendance`：考勤 `file`：文件 `kpi`：绩效 `message`：消息 `operationlogs`：操作日志 `organization`：组织 `staff`：人员 `payroll`：薪资 `todos`：代办 `workflow`：审批相关 `smartapp`：smartapp | 如没有特殊需求，`scope=client`即可                          |

### 请求示例:

> 请求地址为固定地址
>
> ```http
> https://openapi.ihr360.com/openapi/oauth/token?grant_type=client_credentials&scope=client
> ```
>
> ------

响应数据说明

**正常返回结果说明**

| 参数            | 类型   | 父节点 | 备注                  |
| --------------- | ------ | ------ | --------------------- |
| `access_token`  | String | -      | 接口访问凭证          |
| `token_type`    | String | -      | 授权类型,固定位bearer |
| `refresh_token` | String | -      | 刷新凭证              |
| `expires_in`    | long   | -      | 凭证有效期,单位:秒    |
| `scope`         | String | -      | 授权范围              |

### 响应数据示例

```json
{
    "access_token": "d257c5b1-a7e1-4bdc-899c-a8f8378b9e8f",
    "token_type": "bearer",
    "refresh_token": "58463bbb-0167-4b0e-b378-c5c01600543f",
    "expires_in": 4411,
    "scope": "client"
}
```

**异常返回结果说明**

| 参数                | 类型   | 父节点 | 备注           |
| ------------------- | ------ | ------ | -------------- |
| `error`             | String | -      | 认证授权错误码 |
| `error_description` | String | -      | 错误信息       |

响应数据示例

```json
{
    "error": "invalid_token",
    "error_description": "Access token expired:8f87b9b1-9f66-4b32-9269-0d41a0face00"
}
```

*凭证过期后，用户可以重新调用以下接口获取新的`access_token`并延长使用时间*

接口描述：

> 用于重新获取访问OPENAPI的认证凭证(获取access*token、refresh*token)。

接口定义：重新获取接口请求`access_token`，并延长使用时间

接口地址:

```http
https://openapi.ihr360.com/openapi/oauth/token?grant_type=refresh_token&refresh_token
```

请求方式: POST

数据格式:

```
Content-Type:application/json;charset=UTF-8;Authorization: Basic 认证串
```

**认证串**的生成方式

```java
Base64Encode(AppID:AppSecret)
```

| 键              | 值                         |
| --------------- | -------------------------- |
| AppID           | aBcDeF                     |
| AppSecret       | A1B2C3                     |
| AppID:AppSecret | aBcDeF:A1B2C3              |
| Base64编码结果  | YUJjRGVGOkExQjJDMw==       |
| Authorization   | Basic YUJjRGVGOkExQjJDMw== |

完整请求头信息为

```
Content-Type:application/json;charset=UTF-8;Authorization: Basic YUJjRGVGOkExQjJDMw==
```

### 请求参数说明:

| 参数            | 位置  | 类型   | 必填 | 限制 | 备注   |                                                             |
| --------------- | ----- | ------ | ---- | ---- | ------ | ----------------------------------------------------------- |
| `grant_type`    | query | String |      | 是   | 固定值 | 标志授权方式，目前支持`client_credentials`、`refresh_token` |
| `refresh_token` | query | String |      | 是   | 无     |                                                             |

请求示例:

> 请求地址为固定地址
>
> ```http
> https://openapi.ihr360.com/openapi/oauth/token?grant_type=refresh_token&refresh_token=58463bbb-0167-4b0e-b378-c5c01600543f
> ```
>
> ------

响应数据说明

**正常返回结果说明**

| 参数            | 类型   | 父节点 | 备注                  |
| --------------- | ------ | ------ | --------------------- |
| `access_token`  | String | -      | 接口访问凭证          |
| `token_type`    | String | -      | 授权类型,固定位bearer |
| `refresh_token` | String | -      | 刷新凭证              |
| `expires_in`    | long   | -      | 凭证有效期,单位:秒    |
| `scope`         | String | -      | 授权范围              |

响应数据示例

```json
{
    "access_token": "17efe4c1-078d-426a-bda4-178e3f312438",
    "token_type": "bearer",
    "refresh_token": "46675425-07d5-4775-bf3e-551a842cbd0f",
    "expires_in": 7199,
    "scope": "client"
}
```

*凭证过期后，用户可以重新通过此方式获取凭证*

------

### 其它特殊注意点说明

> 如果获取`access_token`响应结果中没有`refresh_token`信息，请联系管理员重新开通openAPI功能。

**Access Token的有效期**

> 调试期间，`access_token`的有效期目前为2分钟；
>
> 正式运行期间，`access_token`的有效期目前为2个小时；
>
> `access_token` 过期后，需要获取新的`access_token`，`access_token`有效期的范围内容，重复获取`access_token`会返回原有`access_token`。

"i人事"的API调用所需的`access_token`的使用及生成方式说明：

1. 建议第三方应用使用http请求拦截器（过滤器）来获取 `access_token`，在接口请求拦截器，如果发现`access_token`已经失效，那么先更新`access_token`，再进行接口请求。
2. 目前`access_token`的有效期通过返回的`expire_in`来传达，目前是7200秒之内的值。
3. `access_token`的有效时间可能会在未来有调整，所有接口请求如果报`invalid_token`接口错误，也需要执行更新`access_token`，再重新进行接口调用。

**关于请求头信息**

> 调用任何接口前，请保证请求头信息（HttpRequest Header）中具有`Content-Type:application/json;charset=UTF-8`信息，否则无法保证结果的准确性！！！





# 企业应用API/组织架构/部门/根据部门id获取所有子孙部门



### 接口描述：

> 根据部门id获取所有子孙部门(包含本部门).

------

变更历史：

| 变更内容                           | 变更时间   | 变更人 | 备注 |
| ---------------------------------- | ---------- | ------ | ---- |
| 创建根据部门id获取所有子孙部门接口 | 2024-09-19 | Alex   | -    |

------

### 接口地址：

```
https://openapi.ihr360.com/openapi/thirdparty/api/org/v1/organization/children/all
```



### 请求方式：POST

数据格式：

```
Content-Type:application/json;charset=UTF-8;Authorization:Bearer access_token
```



------

### 请求示例：

URL： https://openapi.ihr360.com/openapi/thirdparty/api/org/v1/organization/children/all?companyId=system-default-company-id&departmentStatus=ALL

```http-request

[
  {
    "departmentId": 999,
    "includeAll": true
  },
  {
    "departmentId": 255,
    "includeAll": true
  }
]
```

### 请求数据：

| 参数             | 位置         | 类型    | 必填 | 限制 | 备注                                             |
| ---------------- | ------------ | ------- | ---- | ---- | ------------------------------------------------ |
| companyId        | RequestParam | String  | 是   | -    | 公司id                                           |
| departmentStatus | RequestParam | String  | 否   | -    | ALL：全部（包含停用的部门） ENABLE：启用（默认） |
| includeAll       | body         | Boolean | 否   | -    | 是否查询所有子孙部门，默认为false                |
| departmentId     | body         | Long    | 是   | -    | 部门id                                           |

------

### 响应数据：

| 参数             | 类型      | 父节点        | 备注                                    |
| ---------------- | --------- | ------------- | --------------------------------------- |
| code             | int       | Response Body | -                                       |
| message          | String    | Response Body | -                                       |
| data             | JSONArray | Response Body | -                                       |
| companyId        | String    | data          | 公司id                                  |
| id               | String    | data          | id                                      |
| departmentCode   | String    | data          | 部门编码                                |
| departmentId     | Long      | data          | 部门ID                                  |
| departmentName   | String    | data          | 部门名称                                |
| parentId         | Long      | data          | 父部门id                                |
| staffId          | String    | data          | 部门负责人Id                            |
| staffName        | String    | data          | 部门负责人名称                          |
| departmentStatus | String    | data          | 部门状态 `ENABLE`：启用 `DISABLE`：停用 |
| errorResult      | Boolean   | Response Body | -                                       |

### 响应示例：

```json
{
  "data": {
    "255": [
      {
        "id": "bf4d269a-ee31-4477-aaf3-3af16517068e",
        "companyId": "75f058a05fbb4ed6b9cde6a8ffd898a6",
        "parentId": "0",
        "staffId": null,
        "staffName": null,
        "departmentId": "255",
        "departmentName": "11222255",
        "departmentStatus": "ENABLE",
        "departmentCode": null
      },
      {
        "id": "2d948b43-935d-42ee-96a5-8ddeeea4c719",
        "companyId": "75f058a05fbb4ed6b9cde6a8ffd898a6",
        "parentId": "255",
        "staffId": null,
        "staffName": null,
        "departmentId": "258",
        "departmentName": "11",
        "departmentStatus": "DISABLE",
        "departmentCode": "00007"
      }
    ],
    "999": []
  },
  "code": 0,
  "message": "OK",
  "errorResult": false
}
```





# 企业应用API/员工管理/员工/根据部门ID获取员工基本信息



### 接口描述：

> 此接口用于根据部门ID获取当前部门下所有员工基本信息

定义：获取员工基本信息

------

变更历史：

| 变更内容                           | 变更时间   | 变更人     | 备注 |
| ---------------------------------- | ---------- | ---------- | ---- |
| 错误文档修正                       | 2024-01-05 | Beck Wang  | -    |
| 新增根据部门id获取员工基本信息接口 | 2022-01-12 | Nick Zhang | -    |

------

### 接口地址：

```
https://openapi.ihr360.com/openapi/thirdparty/api/staff/v1/department/staff/basic
```



### 请求方式：POST

数据格式：

```
Content-Type:application/json;charset=UTF-8;Authorization:Bearer access_token
```



------

### 请求数据

| 参数         | 位置 | 类型 | 必填 | 备注   |
| ------------ | ---- | ---- | ---- | ------ |
| departmentId | body | List | 是   | 部门ID |

### 请求示例

Body

```json
[
    0,1,2
]
```

------

### 响应数据

| 参数              | 类型       | 父节点        | 备注                                                         |
| ----------------- | ---------- | ------------- | ------------------------------------------------------------ |
| code              | int        | Response Body | -                                                            |
| message           | String     | Response Body | -                                                            |
| data              | JSONObject | Response Body | -                                                            |
| staffId           | String     | data          | 员工ID                                                       |
| staffNo           | String     | data          | 工号                                                         |
| staffName         | String     | data          | 姓名                                                         |
| englishName       | String     | data          | 英文姓名                                                     |
| nickName          | String     | data          | 花名                                                         |
| mobileNo          | String     | data          | 手机号                                                       |
| email             | String     | data          | 电子地址                                                     |
| workEmail         | String     | data          | 工作邮箱                                                     |
| workPhone         | String     | data          | 工作电话                                                     |
| staffStatus       | String     | data          | 员工状态 `IN_SERVICE`：在职 `QUIT`：离职                     |
| idCardType        | String     | data          | 证件类型 `IDENTITY_CARD`：居民身份证 `CHINA_PASSPORT`：中国护照 `FOREIGN_PASSPORT`：外国护照 `MTP`：台湾居民来往大陆通行证 `MTPFHKAMR`：港澳居民来往内地通行证 `OTHER`：其他 |
| idCardNo          | String     | data          | 证件号码                                                     |
| departmentId      | Long       | data          | 部门id                                                       |
| departmentName    | String     | data          | 部门                                                         |
| positionId        | String     | data          | 职位ID                                                       |
| positionName      | String     | data          | 职位                                                         |
| sex               | String     | data          | 性别 `MALE`：男 `FEMALE`：女                                 |
| staffImageId      | String     | data          | 头像Id                                                       |
| staffType         | String     | data          | 员工类型 `FULLTIME`：全职 `PARTTIME`：兼职 `INTERSHIP：`实习 `EXPATRIATE`：外派 `TEMPORARY`：临时工 `REHIRE_RETIREMENT`：退休返聘 |
| marryStatus       | String     | data          | 婚姻状态 `UNMARRIED`：未婚 `MARRIED`：已婚 `M_CHILDREN`：已婚已育 |
| highestEducation  | String     | data          | 最高学历 `DOCTOR`：博士 `MASTER`：硕士/MBA/EMBA `UNDER_GRADUATE`：本科 `JUNIOR`：大专/高职 `SENIOR_MIDDLE`：高中/中专/中技 `JUNIOR_MIDDLE`：初中 `PRIMARY_SCHOOL`：小学 `OTHER`：其他 |
| workPlace         | String     | data          | 联系地址                                                     |
| birthday          | String     | data          | 出生日期，格式：yyyy-MM-dd                                   |
| contractBeginDate | String     | data          | 合同开始日期，格式：yyyy-MM-dd                               |
| contractEndDate   | String     | data          | 合同结束日期，格式：yyyy-MM-dd                               |
| enrollInDate      | String     | data          | 入职日期，格式：yyyy-MM-dd                                   |
| leaveDate         | String     | data          | 离职日期，格式：yyyy-MM-dd                                   |
| probationEndDate  | String     | data          | 试用期结束日期，格式：yyyy-MM-dd                             |
| createdDate       | String     | data          | 创建日期，格式：yyyy-MM-dd HH:mm:ss                          |
| lastUpdateDate    | String     | data          | 最后修改日期，格式：yyyy-MM-dd HH:mm:ss                      |
| errorResult       | Boolean    | Response Body | -                                                            |

### 响应数据JSON示例

```json
{
  "code": 0,
  "message": "SUCCESS",
  "data": [
    {
      "staffId": "02178796-cd8f-40d1-ae73-7cb4edac6018",
      "staffNo": "FN001",
      "staffName": "刘云",
      "nickName": null,
      "englishName": null,
      "mobileNo": "13355556666",
      "email": "cloudliu@xxxtech.cn",
      "workEmail": "liu.shishi@ihr360.com",
      "workPhone": null,
      "staffStatus": "IN_SERVICE",
      "idCardType": "OTHER",
      "idCardNo": "35044266668888",
      "departmentId": 0,
      "departmentName": "财务部门",
      "positionId": null,
      "positionName": "财务总监",
      "sex": "MALE",
      "staffImageId": "e9093f9c-890e-48a7-912e-2e61641d36e3",
      "staffType": "FULLTIME",
      "marryStatus": "UNMARRIED",
      "highestEducation": "UNDER_GRADUATE",
      "workPlace": "上海",
      "birthday": "1990-07-01",
      "contractBeginDate": "2015-07-01",
      "contractEndDate": "2018-06-30",
      "enrollInDate": "2015-07-01",
      "leaveDate": null,
      "probationEndDate": "2015-09-30",
      "createdDate": "2016-12-20 19:22:00",
      "lastUpdateDate": null
    },
    {
      "staffId": "02178796-cd8f-40d1-ae73-7cb4edac6018",
      "staffNo": "FN001",
      "staffName": "徐兵",
      "nickName": null,
      "englishName": null,
      "mobileNo": "13355556666",
      "email": "cloudliu@xxxtech.cn",
      "workEmail": "liu.shishi@ihr360.com",
      "workPhone": null,
      "staffStatus": "IN_SERVICE",
      "idCardType": "OTHER",
      "idCardNo": "35044266668888",
      "departmentId": 0,
      "departmentName": "财务部门",
      "positionId": null,
      "positionName": "财务总监",
      "sex": "MALE",
      "staffImageId": "e9093f9c-890e-48a7-912e-2e61641d36e3",
      "staffType": "FULLTIME",
      "marryStatus": "UNMARRIED",
      "highestEducation": "UNDER_GRADUATE",
      "workPlace": "上海",
      "birthday": "1990-07-01",
      "contractBeginDate": "2015-07-01",
      "contractEndDate": "2018-06-30",
      "enrollInDate": "2015-07-01",
      "leaveDate": null,
      "probationEndDate": "2015-09-30",
      "createdDate": "2016-12-20 19:22:00",
      "lastUpdateDate": null
    }
  ],
  "errorResult": false
}
```





# 企业应用API/考勤管理/排班相关/获取排班V4



### 接口描述：

> 此接口用于批量获取某些人某些天所有的排班班次列表

接口定义：批量获取所有排班班次列表

------

变更历史：

| 变更内容           | 变更时间   | 变更人 | 备注 |
| ------------------ | ---------- | ------ | ---- |
| 创建获取排班V4接口 | 2022-03-14 | Jerry  | -    |

------

### 接口地址：

```
https://openapi.ihr360.com/openapi/thirdparty/api/tm/v4/schedules/actual/all
```



### 请求方式：POST

数据格式：

```
Content-Type:application/json;charset=UTF-8;Authorization:Bearer access_token
```



------

### 请求示例：

URL

```http
https://openapi.ihr360.com/openapi/thirdparty/api/tm/v4/schedules/actual/all
```

Body

```json
{
  "startTime": "2021-01-01",
  "endTime": "2021-01-31",
  "staffIdList": ["d4843836-8c4a-46eb-875b-aabef3ecb0cc","db20ae02-df53-4386-b8d8-db279eb5e9f4"]
}
```

### 请求数据：

| 参数        | 位置 | 类型 | 必填 | 限制             | 备注                                            |
| ----------- | ---- | ---- | ---- | ---------------- | ----------------------------------------------- |
| startTime   | body | Date | 是   | 格式：yyyy-MM-dd | 排班日期开始时间                                |
| endTime     | body | Date | 是   | 格式：yyyy-MM-dd | 排班日期结束时间 开始和结束时间跨度不能超过31天 |
| staffIdList | body | List | 否   | -                | 员工Id集合，单次最多传入100人                   |

### 响应数据：

| 参数         | 类型      | 父节点        | 备注         |
| ------------ | --------- | ------------- | ------------ |
| code         | int       | Response Body | -            |
| message      | String    | Response Body | -            |
| data         | JSONArray | Response Body | -            |
| errorResult  | Boolean   | Response Body | -            |
| staffId      | String    | data          | 员工ID       |
| shiftId      | String    | data          | 班次ID       |
| shiftAbbr    | String    | data          | 班次简称     |
| planDate     | Date      | data          | 排班日期     |
| isWorkingDay | Boolean   | data          | 是否是工作日 |

### 响应数据JSON示例

```json
{
  "code": 0,
  "message":"OK",
  "data": [
      {
        "staffId": "d4843836-8c4a-46eb-875b-aabef3ecb0cc",
        "shiftId": "0",
        "shiftAbbr": "休",
        "planDate": "2021-09-01",
        "isWorkingDay": false
      },
      {
        "staffId": "db20ae02-df53-4386-b8d8-db279eb5e9f4",
        "shiftId": "0",
        "shiftAbbr": "休",
        "planDate": "2021-09-02",
        "isWorkingDay": false
      }
    ],
  "errorResult": false
}
```



## 文字描述：

认证授权前置参数条件：

```
获取token：
https://openapi.ihr360.com/openapi/oauth/token

headers：
Authorization：Basic MmI3YTE1YmQtNDA2Yi00ODRlLWE3NmMtY2FlMGY4ZmYwYmQzOjE5ZWQ4Njg2LWUzNDQtNDNhNi1iNzY0LTgxMzg0ODAxN2QxMA==

Content-Type：application/json

```

> "MmI3YTE1YmQtNDA2Yi00ODRlLWE3NmMtY2FlMGY4ZmYwYmQzOjE5ZWQ4Njg2LWUzNDQtNDNhNi1iNzY0LTgxMzg0ODAxN2QxMA==" ，这个内容 是我提供的数据参数，但注意Basic后面是有一个空格



接口返回体

```json
{
    "access_token": "f97213ba-bea1-4963-a48a-f8ba0c12c293",（这个就是我们的目标token值
    "token_type": "bearer",
    "refresh_token": "734630b0-e356-45cb-846e-bd520d287e43",
    "expires_in": 6955,
    "scope": "dingtalk smartapp interface"
}
```

>  "access_token": "f97213ba-bea1-4963-a48a-f8ba0c12c293",（这个就是我们的目标token值，



### 获取排班表业务逻辑：

# **补充公司人员排班是在当月最后一天排下一个月的数据，而不是全年一次性排完）** 未完成！！！！

：根据“集团总部”的id：`3`根据这个id去调用接口`根据部门id获取所有子孙部门(包含本部门)`,获取所有子部门的子部门id数据`"departmentId"`, 然后根据部门ids去获取所有的员工信息`接口为：根据部门ID获取当前部门下所有员工基本信息`。然后根据员工id去获取所有对应人的排班信息`接口为：批量获取某些人某些天所有的排班班次列表（排班日期结束时间、开始和结束时间跨度不能超过31天）`



### 三个接口：

标注`所有数据库建表的时候，id主键，雪花id（主键索引）； 创建人创建时间；更新人更新时间；软删除标识；备注remark；（这些标准字段是必须要有的）`

条件：龙派集团总部：id=‘3’ （这是父节点）  departmentCode=‘LPH’

#### 接口一：`根据部门id获取所有子孙部门(包含本部门)`

```
接口调用地址：
https://openapi.ihr360.com/openapi/thirdparty/api/org/v1/organization/children/all

headers
Content-Type：application/json
Authorization：Bearer f97213ba-bea1-4963-a48a-f8ba0c12c293

请求体：
[
  {
    "departmentId": 27,
    "includeAll": true
  }
]
```

查询返回的一个json结果：

```
{
    "data": {
        "27": [
            {
                "id": "f50e1f48-5424-4914-a5e4-5c9495336715",
                "companyId": "01-0202d678717cae4b036b1397bd2cf",
                "parentId": "3",
                "staffId": "ece81ac1-0cd1-49cd-b001-c7889b06bdba",
                "staffName": "刘璋",
                "departmentId": "27",
                "departmentName": "产品中心",
                "inServiceNumber": null,
                "inServiceTotalNumber": null,
                "departmentStatus": "ENABLE",
                "departmentCode": "PDC"
            },
            {
                "id": "2fef0af9-0193-45a4-98f6-4001c6c0a4bc",
                "companyId": "01-0202d678717cae4b036b1397bd2cf",
                "parentId": "27",
                "staffId": "cea00112-c049-4258-8cf3-1d42bfda21ce",
                "staffName": "刘永洪",
                "departmentId": "28",
                "departmentName": "TNT商用成品部",
                "inServiceNumber": null,
                "inServiceTotalNumber": null,
                "departmentStatus": "ENABLE",
                "departmentCode": "PDC01"
            },
            {
                "id": "ca81a04a-d569-4c35-a177-9b10a75bfdca",
                "companyId": "01-0202d678717cae4b036b1397bd2cf",
                "parentId": "27",
                "staffId": "08926a50-2dd4-41b5-bb55-16590d469783",
                "staffName": "张艳军",
                "departmentId": "29",
                "departmentName": "TNT分配器部",
                "inServiceNumber": null,
                "inServiceTotalNumber": null,
                "departmentStatus": "ENABLE",
                "departmentCode": "PDC02"
            },
            {
                "id": "ec61a1d8-e6e1-483c-8645-b2a695c6df00",
                "companyId": "01-0202d678717cae4b036b1397bd2cf",
                "parentId": "27",
                "staffId": "adb9ab0d-3d13-4eec-9a2a-40115ece8df5",
                "staffName": "赵来斌",
                "departmentId": "30",
                "departmentName": "纸塑容器部",
                "inServiceNumber": null,
                "inServiceTotalNumber": null,
                "departmentStatus": "ENABLE",
                "departmentCode": "PDC03"
            },
            {
                "id": "a68605ae-7169-45af-9305-8bda71ea023a",
                "companyId": "01-0202d678717cae4b036b1397bd2cf",
                "parentId": "27",
                "staffId": "adb9ab0d-3d13-4eec-9a2a-40115ece8df5",
                "staffName": "赵来斌",
                "departmentId": "31",
                "departmentName": "卡纸容器杯碗桶部",
                "inServiceNumber": null,
                "inServiceTotalNumber": null,
                "departmentStatus": "ENABLE",
                "departmentCode": "PDC04"
            },
            {
                "id": "47e217cd-7cea-4d7a-a1d7-6ecb19d2c999",
                "companyId": "01-0202d678717cae4b036b1397bd2cf",
                "parentId": "27",
                "staffId": "adb9ab0d-3d13-4eec-9a2a-40115ece8df5",
                "staffName": "赵来斌",
                "departmentId": "32",
                "departmentName": "卡纸容器碟盒部",
                "inServiceNumber": null,
                "inServiceTotalNumber": null,
                "departmentStatus": "ENABLE",
                "departmentCode": "PDC05"
            },
            {
                "id": "b232b2db-bfd9-4623-9f34-fb4b2c681642",
                "companyId": "01-0202d678717cae4b036b1397bd2cf",
                "parentId": "27",
                "staffId": "adb9ab0d-3d13-4eec-9a2a-40115ece8df5",
                "staffName": "赵来斌",
                "departmentId": "238",
                "departmentName": "瓦纸容器部",
                "inServiceNumber": null,
                "inServiceTotalNumber": null,
                "departmentStatus": "ENABLE",
                "departmentCode": "PDC06"
            },
            {
                "id": "807b0239-bea5-4a0e-b3f7-508f75177535",
                "companyId": "01-0202d678717cae4b036b1397bd2cf",
                "parentId": "27",
                "staffId": "26586ef3-0662-466b-984c-c2309dc7a48e",
                "staffName": "朱燕妮",
                "departmentId": "241",
                "departmentName": "购物袋部",
                "inServiceNumber": null,
                "inServiceTotalNumber": null,
                "departmentStatus": "ENABLE",
                "departmentCode": "PDC07"
            },
            {
                "id": "47f4bb7f-4397-478e-8f11-a18d0d1f12b7",
                "companyId": "01-0202d678717cae4b036b1397bd2cf",
                "parentId": "27",
                "staffId": "26586ef3-0662-466b-984c-c2309dc7a48e",
                "staffName": "朱燕妮",
                "departmentId": "244",
                "departmentName": "食品包装纸袋部",
                "inServiceNumber": null,
                "inServiceTotalNumber": null,
                "departmentStatus": "ENABLE",
                "departmentCode": "PDC08"
            },
            {
                "id": "3a08f1c4-30fb-4847-bc41-2838f2837700",
                "companyId": "01-0202d678717cae4b036b1397bd2cf",
                "parentId": "27",
                "staffId": "08926a50-2dd4-41b5-bb55-16590d469783",
                "staffName": "张艳军",
                "departmentId": "247",
                "departmentName": "餐具部",
                "inServiceNumber": null,
                "inServiceTotalNumber": null,
                "departmentStatus": "ENABLE",
                "departmentCode": "PDC09"
            }
        ]
    },
    "code": 0,
    "message": "OK",
    "errorResult": false
}
```

数据库表：部门表

```json
存表数据字段：
		  
           "companyId": "01-0202d678717cae4b036b1397bd2cf",   公司id
           "parentId": "27",   父部门id
           "staffId": "08926a50-2dd4-41b5-bb55-16590d469783",  部门负责人Id
           "staffName": "张艳军
           "departmentId": "247 部门ID 
           "departmentName": "餐具部   
           "departmentStatus": "ENABLE",  部门状态 ENABLE：启用 DISABLE：停用
           "departmentCode": "PDC09 部门编码（注意这是二级部门是"PDC01"、"PDC02"等，一级部门就是PDC
			
这里我希望就是知道当前部门，我需要拉出他所有上下级部门出来，上级的顶点是：龙派集团总部：id=‘3’ （这是父节点）
下级部门就拿到是什么就存什么，没有就是null。所以departmentName2、departmentName3、departmentName4、departmentName5就好了。（注意departmentCode，departmentId   这里面的部门1是最顶层的部门，然后依次往下推就好； 。
```



#### 接口二：`根据部门ID获取当前部门下所有员工基本信息`  （权限已开通）

```json
接口调用地址：
https://openapi.ihr360.com/openapi/thirdparty/api/staff/v1/department/staff/basic

headers
Content-Type：application/json
Authorization：Bearer f97213ba-bea1-4963-a48a-f8ba0c12c293

请求体：
[
    294
]
```

> f97213ba-bea1-4963-a48a-f8ba0c12c293    这个就是通过认证接口获取的 "access_token"，然后拼接到`Authorization：Bearer `注意bearer后面是有一个空格



查询返回的一个json结果：

```json
{
    "data": [
        {
            "staffId": "0b310c2b-f0c6-47a5-9e73-8601343d194f",
            "staffNo": "CH2012HQ00001",
            "staffName": "邵宇",
            "englishName": null,
            "nickName": null,
            "mobileNo": "15013013597",
            "email": "",
            "workEmail": "steve-shao@lopiepaper.com",
            "workPhone": null,
            "staffStatus": "IN_SERVICE",
            "idCardType": "IDENTITY_CARD",
            "idCardNo": "220622198801075511",
            "departmentId": 21,
            "departmentName": "加拿大销售部",
            "positionId": "952306cb-7316-4c73-9acf-51bd4924b4b1",
            "positionName": "部门负责人",
            "sex": "MALE",
            "staffImageId": "664c1ab3-6792-47a6-9a7c-66644cd53e6d",
            "staffType": "FULLTIME",
            "marryStatus": "MARRIED",
            "highestEducation": null,
            "workPlace": "广州市白云区同德围",
            "companySiteId": "f89a6fc4-c291-4379-8583-4075c043fe80",
            "companySiteName": "龙派集团总部",
            "birthday": "1988-01-07",
            "contractBeginDate": "2024-09-01",
            "contractEndDate": null,
            "enrollInDate": "2012-03-15",
            "leaveDate": null,
            "probationEndDate": "2012-06-14",
            "createdDate": "2025-02-12 11:11:38",
            "lastUpdateDate": "2026-08-13 16:24:15"
        }
    ],
    "code": 0,
    "message": "SUCCESS",
    "errorResult": false
}
```

```
数据库表：人员表（这个是普通索引

需要存表字段：
            "staffId": "0b310c2b-f0c6-47a5-9e73-8601343d194f",
            "staffNo": "CH2012HQ00001",
            "staffName": "邵宇",
            "staffStatus": "IN_SERVICE",
            "departmentId": 21,
            "departmentName": "加拿大销售部",
		   "companySiteName": "龙派集团总部",
		   "leaveDate": null,  
		   prDepartmentId:"",  二级部门id
		   prDepartmentName:""  二级部门名称
		   
		 注意：如果当前部门不是二级部门，那就带出他的二级部门存入数据库中，(我只需要拿到二级部门的信息，别的信息我是不需要的)
```





#### 接口三：`批量获取某些人某些天所有的排班班次列表`

```
接口调用地址：
https://openapi.ihr360.com/openapi/thirdparty/api/tm/v4/schedules/actual/all

headers
Content-Type：application/json
Authorization：Bearer f97213ba-bea1-4963-a48a-f8ba0c12c293

请求体：
{
  "startTime": "2026-08-15",
  "endTime": "2026-08-19",
  "staffIdList": ["d505d53c-07ac-461c-a28e-b4826b08c625","a559abd5-e8db-4fea-b324-3033359a5398"],
  "page": 0,
  "size": 100
}
```

```
返回数据体：
{
    "data": [
        {
            "staffId": "a559abd5-e8db-4fea-b324-3033359a5398",  员工ID
            "shiftId": "7d5afda9-c553-4302-822b-457d0c4ce015",  班次ID
            "shiftAbbr": "总部行政（休2）",  班次简称
            "planDate": "2026-08-15",   排班日期
            "isWorkingDay": false   是否是工作日
        },
        {
            "staffId": "a559abd5-e8db-4fea-b324-3033359a5398",
            "shiftId": "7d5afda9-c553-4302-822b-457d0c4ce015",
            "shiftAbbr": "总部行政（休2）",
            "planDate": "2026-08-16",
            "isWorkingDay": false
        },
        {
            "staffId": "a559abd5-e8db-4fea-b324-3033359a5398",
            "shiftId": "d3e4d7a7-04fd-4c88-be12-2c46439d44cb",
            "shiftAbbr": "总部行政",
            "planDate": "2026-08-17",
            "isWorkingDay": true
        },
        {
            "staffId": "a559abd5-e8db-4fea-b324-3033359a5398",
            "shiftId": "d3e4d7a7-04fd-4c88-be12-2c46439d44cb",
            "shiftAbbr": "总部行政",
            "planDate": "2026-08-18",
            "isWorkingDay": true
        },
        {
            "staffId": "a559abd5-e8db-4fea-b324-3033359a5398",
            "shiftId": "d3e4d7a7-04fd-4c88-be12-2c46439d44cb",
            "shiftAbbr": "总部行政",
            "planDate": "2026-08-19",
            "isWorkingDay": true
        },
        {
            "staffId": "d505d53c-07ac-461c-a28e-b4826b08c625",
            "shiftId": "7d5afda9-c553-4302-822b-457d0c4ce015",
            "shiftAbbr": "总部行政（休2）",
            "planDate": "2026-08-15",
            "isWorkingDay": false
        },
        {
            "staffId": "d505d53c-07ac-461c-a28e-b4826b08c625",
            "shiftId": "7d5afda9-c553-4302-822b-457d0c4ce015",
            "shiftAbbr": "总部行政（休2）",
            "planDate": "2026-08-16",
            "isWorkingDay": false
        },
        {
            "staffId": "d505d53c-07ac-461c-a28e-b4826b08c625",
            "shiftId": "d3e4d7a7-04fd-4c88-be12-2c46439d44cb",
            "shiftAbbr": "总部行政",
            "planDate": "2026-08-17",
            "isWorkingDay": true
        },
        {
            "staffId": "d505d53c-07ac-461c-a28e-b4826b08c625",
            "shiftId": "d3e4d7a7-04fd-4c88-be12-2c46439d44cb",
            "shiftAbbr": "总部行政",
            "planDate": "2026-08-18",
            "isWorkingDay": true
        },
        {
            "staffId": "d505d53c-07ac-461c-a28e-b4826b08c625",
            "shiftId": "d3e4d7a7-04fd-4c88-be12-2c46439d44cb",
            "shiftAbbr": "总部行政",
            "planDate": "2026-08-19",
            "isWorkingDay": true
        }
    ],
    "code": 0,
    "message": "OK",
    "errorResult": false
}
```



```
数据库表字段：
		   "staffId": "a559abd5-e8db-4fea-b324-3033359a5398",  员工ID （这个是普通索引
            "shiftId": "7d5afda9-c553-4302-822b-457d0c4ce015",  班次ID
            "shiftAbbr": "总部行政（休2）",  班次简称
            "planDate": "2026-08-15",   排班日期
            "isWorkingDay": false   是否是工作日
            拼接人员表的数据，通过staffId关联
            "staffName": "邵宇",
            "staffStatus": "IN_SERVICE",
            "departmentId": 21,
            "departmentName": "加拿大销售部",
		   "companySiteName": "龙派集团总部",
		    prDepartmentId:"",  二级部门id
		   prDepartmentName:""  二级部门名称
```

接口三注意点：（获取一年的，一个月为子任务去拆解，

（注意存储两张表：一个是所有员工的信息表，一个是所有员工的排班表

（员工Id集合，单次最多传入100人的staffIds



## 编码

> mapper.xml我就没有补充了。

### 建表sql

数据库创建表的sql语句Oracle

```sql
-- --------------------------------------------------------
-- 部门表 LPDCM.OA_HRIS_DEPARTMENT
-- 来源接口：/api/org/v1/organization/children/all 
-- 层级说明：LEVEL1 固定为顶点「龙派集团总部」(departmentId=3, departmentCode=LPH)，
-- 依次往下 LEVEL2~LEVEL5；取不到的层级存 NULL。
-------------------------------------------------------------

CREATE TABLE "LPDCM"."OA_HRIS_DEPARTMENT" (
  "ID"                  NUMBER(19)     NOT NULL,
  -- ===== 接口原始字段 =====
  "COMPANY_ID"          VARCHAR2(64)   DEFAULT '',
  "DEPARTMENT_ID"       NUMBER(19),
  "DEPARTMENT_NAME"     VARCHAR2(200)  DEFAULT '',
  "DEPARTMENT_CODE"     VARCHAR2(64)   DEFAULT '',
  "PARENT_ID"           NUMBER(19),
  "LEADER_STAFF_ID"     VARCHAR2(64)   DEFAULT '',
  "LEADER_STAFF_NAME"   VARCHAR2(100)  DEFAULT '',
  "DEPARTMENT_STATUS"   VARCHAR2(16)   DEFAULT '',
  -- ===== 层级展开（LEVEL1=顶层总部，依次往下）=====
  "DEPARTMENT_ID1"      NUMBER(19),
  "DEPARTMENT_NAME1"    VARCHAR2(200)  DEFAULT '',
  "DEPARTMENT_CODE1"    VARCHAR2(64)   DEFAULT '',
  "DEPARTMENT_ID2"      NUMBER(19),
  "DEPARTMENT_NAME2"    VARCHAR2(200)  DEFAULT '',
  "DEPARTMENT_CODE2"    VARCHAR2(64)   DEFAULT '',
  "DEPARTMENT_ID3"      NUMBER(19),
  "DEPARTMENT_NAME3"    VARCHAR2(200)  DEFAULT '',
  "DEPARTMENT_CODE3"    VARCHAR2(64)   DEFAULT '',
  "DEPARTMENT_ID4"      NUMBER(19),
  "DEPARTMENT_NAME4"    VARCHAR2(200)  DEFAULT '',
  "DEPARTMENT_CODE4"    VARCHAR2(64)   DEFAULT '',
  "DEPARTMENT_ID5"      NUMBER(19),
  "DEPARTMENT_NAME5"    VARCHAR2(200)  DEFAULT '',
  "DEPARTMENT_CODE5"    VARCHAR2(64)   DEFAULT '',
  "DEPARTMENT_LEVEL"    NUMBER(2),
  -- ===== 标准字段 =====
  "CREATE_BY"           VARCHAR2(64)   DEFAULT '',
  "CREATE_TIME"         DATE           DEFAULT SYSDATE,
  "UPDATE_BY"           VARCHAR2(64)   DEFAULT '',
  "UPDATE_TIME"         DATE,
  "DEL"                 CHAR(1)        DEFAULT '0',
  "REMARK"              VARCHAR2(500)  DEFAULT '',
  CONSTRAINT "PK_OA_HRIS_DEPARTMENT" PRIMARY KEY ("ID")
);

CREATE UNIQUE INDEX "LPDCM"."UK_OHD_DEPT_ID"  ON "LPDCM"."OA_HRIS_DEPARTMENT" ("DEPARTMENT_ID");
CREATE INDEX        "LPDCM"."IDX_OHD_PARENT"  ON "LPDCM"."OA_HRIS_DEPARTMENT" ("PARENT_ID");
CREATE INDEX        "LPDCM"."IDX_OHD_LEVEL2"  ON "LPDCM"."OA_HRIS_DEPARTMENT" ("DEPARTMENT_ID2");

COMMENT ON TABLE  "LPDCM"."OA_HRIS_DEPARTMENT"                    IS '就餐项目-HRIS部门表（集团总部及所有子孙部门）';
COMMENT ON COLUMN "LPDCM"."OA_HRIS_DEPARTMENT"."ID"               IS '主键（雪花ID）';
COMMENT ON COLUMN "LPDCM"."OA_HRIS_DEPARTMENT"."COMPANY_ID"       IS '公司id';
COMMENT ON COLUMN "LPDCM"."OA_HRIS_DEPARTMENT"."DEPARTMENT_ID"    IS '部门ID（业务唯一）';
COMMENT ON COLUMN "LPDCM"."OA_HRIS_DEPARTMENT"."DEPARTMENT_NAME"  IS '部门名称';
COMMENT ON COLUMN "LPDCM"."OA_HRIS_DEPARTMENT"."DEPARTMENT_CODE"  IS '部门编码（如一级PDC、二级PDC01）';
COMMENT ON COLUMN "LPDCM"."OA_HRIS_DEPARTMENT"."PARENT_ID"        IS '父部门id（顶点：龙派集团总部 id=3）';
COMMENT ON COLUMN "LPDCM"."OA_HRIS_DEPARTMENT"."LEADER_STAFF_ID"  IS '部门负责人Id（接口staffId）';
COMMENT ON COLUMN "LPDCM"."OA_HRIS_DEPARTMENT"."LEADER_STAFF_NAME" IS '部门负责人名称（接口staffName）';
COMMENT ON COLUMN "LPDCM"."OA_HRIS_DEPARTMENT"."DEPARTMENT_STATUS" IS '部门状态：ENABLE启用/DISABLE停用';
COMMENT ON COLUMN "LPDCM"."OA_HRIS_DEPARTMENT"."DEPARTMENT_ID1"   IS '一级部门id（顶层，龙派集团总部 id=3）';
COMMENT ON COLUMN "LPDCM"."OA_HRIS_DEPARTMENT"."DEPARTMENT_NAME1" IS '一级部门名称';
COMMENT ON COLUMN "LPDCM"."OA_HRIS_DEPARTMENT"."DEPARTMENT_CODE1" IS '一级部门编码（如LPH）';
COMMENT ON COLUMN "LPDCM"."OA_HRIS_DEPARTMENT"."DEPARTMENT_ID2"   IS '二级部门id';
COMMENT ON COLUMN "LPDCM"."OA_HRIS_DEPARTMENT"."DEPARTMENT_NAME2" IS '二级部门名称';
COMMENT ON COLUMN "LPDCM"."OA_HRIS_DEPARTMENT"."DEPARTMENT_CODE2" IS '二级部门编码';
COMMENT ON COLUMN "LPDCM"."OA_HRIS_DEPARTMENT"."DEPARTMENT_ID3"   IS '三级部门id，无则为空';
COMMENT ON COLUMN "LPDCM"."OA_HRIS_DEPARTMENT"."DEPARTMENT_NAME3" IS '三级部门名称，无则为空';
COMMENT ON COLUMN "LPDCM"."OA_HRIS_DEPARTMENT"."DEPARTMENT_CODE3" IS '三级部门编码，无则为空';
COMMENT ON COLUMN "LPDCM"."OA_HRIS_DEPARTMENT"."DEPARTMENT_ID4"   IS '四级部门id，无则为空';
COMMENT ON COLUMN "LPDCM"."OA_HRIS_DEPARTMENT"."DEPARTMENT_NAME4" IS '四级部门名称，无则为空';
COMMENT ON COLUMN "LPDCM"."OA_HRIS_DEPARTMENT"."DEPARTMENT_CODE4" IS '四级部门编码，无则为空';
COMMENT ON COLUMN "LPDCM"."OA_HRIS_DEPARTMENT"."DEPARTMENT_ID5"   IS '五级部门id，无则为空';
COMMENT ON COLUMN "LPDCM"."OA_HRIS_DEPARTMENT"."DEPARTMENT_NAME5" IS '五级部门名称，无则为空';
COMMENT ON COLUMN "LPDCM"."OA_HRIS_DEPARTMENT"."DEPARTMENT_CODE5" IS '五级部门编码，无则为空';
COMMENT ON COLUMN "LPDCM"."OA_HRIS_DEPARTMENT"."DEPARTMENT_LEVEL" IS '当前部门所处层级：1~5';
COMMENT ON COLUMN "LPDCM"."OA_HRIS_DEPARTMENT"."CREATE_BY"        IS '创建人';
COMMENT ON COLUMN "LPDCM"."OA_HRIS_DEPARTMENT"."CREATE_TIME"      IS '创建时间';
COMMENT ON COLUMN "LPDCM"."OA_HRIS_DEPARTMENT"."UPDATE_BY"        IS '更新人';
COMMENT ON COLUMN "LPDCM"."OA_HRIS_DEPARTMENT"."UPDATE_TIME"      IS '更新时间';
COMMENT ON COLUMN "LPDCM"."OA_HRIS_DEPARTMENT"."DEL"              IS '删除标识：0未删除/1已删除';
COMMENT ON COLUMN "LPDCM"."OA_HRIS_DEPARTMENT"."REMARK"           IS '备注';


-- --------------------------------------------------------
-- 人员表 LPDCM.OA_HRIS_STAFF
-- 来源接口：/api/staff/v1/department/staff/basic
-- 只保留就餐统计所需字段；PR_DEPARTMENT_* 为二级部门（当前部门若不是二级部门，则回溯带出其所属二级部门）。
-------------------------------------------------------------

CREATE TABLE "LPDCM"."OA_HRIS_STAFF" (
  "ID"                  NUMBER(19)     NOT NULL,
  "STAFF_ID"            VARCHAR2(64)   DEFAULT '',
  "STAFF_NO"            VARCHAR2(64)   DEFAULT '',
  "STAFF_NAME"          VARCHAR2(100)  DEFAULT '',
  "STAFF_STATUS"        VARCHAR2(16)   DEFAULT '',
  "DEPARTMENT_ID"       NUMBER(19),
  "DEPARTMENT_NAME"     VARCHAR2(200)  DEFAULT '',
  "PR_DEPARTMENT_ID"    NUMBER(19),
  "PR_DEPARTMENT_NAME"  VARCHAR2(200)  DEFAULT '',
  "COMPANY_SITE_ID"     VARCHAR2(64)   DEFAULT '',
  "COMPANY_SITE_NAME"   VARCHAR2(200)  DEFAULT '',
  "LEAVE_DATE"          DATE,
  -- ===== 标准字段 =====
  "CREATE_BY"           VARCHAR2(64)   DEFAULT '',
  "CREATE_TIME"         DATE           DEFAULT SYSDATE,
  "UPDATE_BY"           VARCHAR2(64)   DEFAULT '',
  "UPDATE_TIME"         DATE,
  "DEL"                 CHAR(1)        DEFAULT '0',
  "REMARK"              VARCHAR2(500)  DEFAULT '',
  CONSTRAINT "PK_OA_HRIS_STAFF" PRIMARY KEY ("ID")
);

CREATE INDEX "LPDCM"."IDX_OHS_STAFF_ID"   ON "LPDCM"."OA_HRIS_STAFF" ("STAFF_ID");
CREATE INDEX "LPDCM"."IDX_OHS_STAFF_NO"   ON "LPDCM"."OA_HRIS_STAFF" ("STAFF_NO");
CREATE INDEX "LPDCM"."IDX_OHS_DEPT"       ON "LPDCM"."OA_HRIS_STAFF" ("DEPARTMENT_ID");

COMMENT ON TABLE  "LPDCM"."OA_HRIS_STAFF"                       IS '就餐项目-HRIS人员表（集团总部在职/离职人员）';
COMMENT ON COLUMN "LPDCM"."OA_HRIS_STAFF"."ID"                  IS '主键（雪花ID）';
COMMENT ON COLUMN "LPDCM"."OA_HRIS_STAFF"."STAFF_ID"            IS '员工ID（普通索引，关联排班表）';
COMMENT ON COLUMN "LPDCM"."OA_HRIS_STAFF"."STAFF_NO"            IS '工号';
COMMENT ON COLUMN "LPDCM"."OA_HRIS_STAFF"."STAFF_NAME"          IS '姓名';
COMMENT ON COLUMN "LPDCM"."OA_HRIS_STAFF"."STAFF_STATUS"        IS '员工状态：IN_SERVICE在职/QUIT离职';
COMMENT ON COLUMN "LPDCM"."OA_HRIS_STAFF"."DEPARTMENT_ID"       IS '所在部门id';
COMMENT ON COLUMN "LPDCM"."OA_HRIS_STAFF"."DEPARTMENT_NAME"     IS '所在部门名称';
COMMENT ON COLUMN "LPDCM"."OA_HRIS_STAFF"."PR_DEPARTMENT_ID"    IS '二级部门id：当前部门非二级时回溯取其所属二级部门';
COMMENT ON COLUMN "LPDCM"."OA_HRIS_STAFF"."PR_DEPARTMENT_NAME"  IS '二级部门名称：报表按此分组';
COMMENT ON COLUMN "LPDCM"."OA_HRIS_STAFF"."COMPANY_SITE_ID"     IS '办公地点id';
COMMENT ON COLUMN "LPDCM"."OA_HRIS_STAFF"."COMPANY_SITE_NAME"   IS '办公地点名称（如龙派集团总部）';
COMMENT ON COLUMN "LPDCM"."OA_HRIS_STAFF"."LEAVE_DATE"          IS '离职日期，在职为空；名单按此日期剔除';
COMMENT ON COLUMN "LPDCM"."OA_HRIS_STAFF"."CREATE_BY"           IS '创建人';
COMMENT ON COLUMN "LPDCM"."OA_HRIS_STAFF"."CREATE_TIME"         IS '创建时间';
COMMENT ON COLUMN "LPDCM"."OA_HRIS_STAFF"."UPDATE_BY"           IS '更新人';
COMMENT ON COLUMN "LPDCM"."OA_HRIS_STAFF"."UPDATE_TIME"         IS '更新时间';
COMMENT ON COLUMN "LPDCM"."OA_HRIS_STAFF"."DEL"                 IS '删除标识：0未删除/1已删除';
COMMENT ON COLUMN "LPDCM"."OA_HRIS_STAFF"."REMARK"              IS '备注';







-- --------------------------------------------------------
-- 排班表 LPDCM.OA_HRIS_SCHEDULE
-- 来源接口：/api/tm/v4/schedules/actual/all 
-- 冗余人员信息（通过 STAFF_ID 关联人员表带出），便于报表直接取数不做多表关联。
-- IS_WORKING_DAY 即"应就餐日"判定基准。
-------------------------------------------------------------

CREATE TABLE "LPDCM"."OA_HRIS_SCHEDULE" (
  "ID"                  NUMBER(19)     NOT NULL,
  -- ===== 接口原始字段 =====
  "STAFF_ID"            VARCHAR2(64)   DEFAULT '',
  "SHIFT_ID"            VARCHAR2(64)   DEFAULT '',
  "SHIFT_ABBR"          VARCHAR2(100)  DEFAULT '',
  "PLAN_DATE"           DATE,
  "IS_WORKING_DAY"      CHAR(1)        DEFAULT 'N',
  -- ===== 冗余人员信息（关联人员表带出）=====
  "STAFF_NO"            VARCHAR2(64)   DEFAULT '',
  "STAFF_NAME"          VARCHAR2(100)  DEFAULT '',
  "STAFF_STATUS"        VARCHAR2(16)   DEFAULT '',
  "DEPARTMENT_ID"       NUMBER(19),
  "DEPARTMENT_NAME"     VARCHAR2(200)  DEFAULT '',
  "PR_DEPARTMENT_ID"    NUMBER(19),
  "PR_DEPARTMENT_NAME"  VARCHAR2(200)  DEFAULT '',
  "COMPANY_SITE_ID"     VARCHAR2(64)   DEFAULT '',
  "COMPANY_SITE_NAME"   VARCHAR2(200)  DEFAULT '',
  -- ===== 标准字段 =====
  "CREATE_BY"           VARCHAR2(64)   DEFAULT '',
  "CREATE_TIME"         DATE           DEFAULT SYSDATE,
  "UPDATE_BY"           VARCHAR2(64)   DEFAULT '',
  "UPDATE_TIME"         DATE,
  "DEL"                 CHAR(1)        DEFAULT '0',
  "REMARK"              VARCHAR2(500)  DEFAULT '',
  CONSTRAINT "PK_OA_HRIS_SCHEDULE" PRIMARY KEY ("ID")
)
-- 按排班日期自动年分区（Oracle 11g+ INTERVAL 分区，新年度数据到达时自动建分区，无需人工维护）
PARTITION BY RANGE ("PLAN_DATE")
INTERVAL (NUMTOYMINTERVAL(1,'YEAR'))
(
  PARTITION "P_SCHEDULE_INIT" VALUES LESS THAN (TO_DATE('2026-01-01','YYYY-MM-DD'))
);

-- 分区键 PLAN_DATE 已包含在索引列中，故可建 LOCAL 索引，随分区一起维护
CREATE UNIQUE INDEX "LPDCM"."UK_OHSC_STAFF_DATE" ON "LPDCM"."OA_HRIS_SCHEDULE" ("STAFF_ID","PLAN_DATE") LOCAL;
CREATE INDEX        "LPDCM"."IDX_OHSC_PLAN_DATE" ON "LPDCM"."OA_HRIS_SCHEDULE" ("PLAN_DATE") LOCAL;
-- STAFF_ID 不含分区键，建全局索引以支持按人跨年度查询
-- ⚠ 注意：该全局索引在 DROP/TRUNCATE PARTITION 时必须带 UPDATE GLOBAL INDEXES，否则索引会失效（详见 4.3）
CREATE INDEX        "LPDCM"."IDX_OHSC_STAFF_ID"  ON "LPDCM"."OA_HRIS_SCHEDULE" ("STAFF_ID");

COMMENT ON TABLE  "LPDCM"."OA_HRIS_SCHEDULE"                       IS '就餐项目-HRIS排班表（员工每日排班，用于确定应就餐日）';
COMMENT ON COLUMN "LPDCM"."OA_HRIS_SCHEDULE"."ID"                  IS '主键（雪花ID）';
COMMENT ON COLUMN "LPDCM"."OA_HRIS_SCHEDULE"."STAFF_ID"            IS '员工ID（普通索引，关联人员表）';
COMMENT ON COLUMN "LPDCM"."OA_HRIS_SCHEDULE"."SHIFT_ID"            IS '班次ID';
COMMENT ON COLUMN "LPDCM"."OA_HRIS_SCHEDULE"."SHIFT_ABBR"          IS '班次简称（如总部行政、总部行政（休2））';
COMMENT ON COLUMN "LPDCM"."OA_HRIS_SCHEDULE"."PLAN_DATE"           IS '排班日期';
COMMENT ON COLUMN "LPDCM"."OA_HRIS_SCHEDULE"."IS_WORKING_DAY"      IS '是否工作日：Y是/N否；Y即应就餐日，N不产生就餐数据';
COMMENT ON COLUMN "LPDCM"."OA_HRIS_SCHEDULE"."STAFF_NO"            IS '工号（冗余）';
COMMENT ON COLUMN "LPDCM"."OA_HRIS_SCHEDULE"."STAFF_NAME"          IS '姓名（冗余）';
COMMENT ON COLUMN "LPDCM"."OA_HRIS_SCHEDULE"."STAFF_STATUS"        IS '员工状态：IN_SERVICE在职/QUIT离职（冗余）';
COMMENT ON COLUMN "LPDCM"."OA_HRIS_SCHEDULE"."DEPARTMENT_ID"       IS '所在部门id（冗余）';
COMMENT ON COLUMN "LPDCM"."OA_HRIS_SCHEDULE"."DEPARTMENT_NAME"     IS '所在部门名称（冗余）';
COMMENT ON COLUMN "LPDCM"."OA_HRIS_SCHEDULE"."PR_DEPARTMENT_ID"    IS '二级部门id（冗余）';
COMMENT ON COLUMN "LPDCM"."OA_HRIS_SCHEDULE"."PR_DEPARTMENT_NAME"  IS '二级部门名称（冗余，报表分组用）';
COMMENT ON COLUMN "LPDCM"."OA_HRIS_SCHEDULE"."COMPANY_SITE_ID"     IS '办公地点id（冗余）';
COMMENT ON COLUMN "LPDCM"."OA_HRIS_SCHEDULE"."COMPANY_SITE_NAME"   IS '办公地点名称（冗余）';
COMMENT ON COLUMN "LPDCM"."OA_HRIS_SCHEDULE"."CREATE_BY"           IS '创建人';
COMMENT ON COLUMN "LPDCM"."OA_HRIS_SCHEDULE"."CREATE_TIME"         IS '创建时间';
COMMENT ON COLUMN "LPDCM"."OA_HRIS_SCHEDULE"."UPDATE_BY"           IS '更新人';
COMMENT ON COLUMN "LPDCM"."OA_HRIS_SCHEDULE"."UPDATE_TIME"         IS '更新时间';
COMMENT ON COLUMN "LPDCM"."OA_HRIS_SCHEDULE"."DEL"                 IS '删除标识：0未删除/1已删除';
COMMENT ON COLUMN "LPDCM"."OA_HRIS_SCHEDULE"."REMARK"              IS '备注';




CREATE SEQUENCE LPDCM.SEQ_OA_HRIS_DEPARTMENT START WITH 1 INCREMENT BY 1 NOCACHE;
CREATE SEQUENCE LPDCM.SEQ_OA_HRIS_STAFF      START WITH 1 INCREMENT BY 1 NOCACHE;
CREATE SEQUENCE LPDCM.SEQ_OA_HRIS_SCHEDULE   START WITH 1 INCREMENT BY 1 NOCACHE;
```



### 同步部门：

controller层：HrisController

```java
@Autowired
    private IHrisDepartmentService departmentService;
    @Autowired
    private IHrisStaffService staffService;
    @Autowired
    private IHrisScheduleService scheduleService;
    @Autowired
    private IhrDiningApiUtil ihrDiningApiUtil;

    /** 初始化：部门+人员 */
    @PostMapping("/calendar/syncBase")
    public AjaxResult syncBase() {
        int dept = departmentService.syncDepartment(ihrDiningApiUtil.getCompanyId());
        int staff = staffService.syncStaff();
        return AjaxResult.success("部门:" + dept + ", 人员:" + staff);
    }

    /** 按日期区间补排班，如 2026-01-01 ~ 2026-12-31 */
    @PostMapping("/calendar/syncSchedule")
    public AjaxResult syncSchedule(
            @RequestParam @DateTimeFormat(pattern = "yyyy-MM-dd") Date startDate,
            @RequestParam @DateTimeFormat(pattern = "yyyy-MM-dd") Date endDate) {
        return AjaxResult.success("排班:" + scheduleService.syncSchedule(startDate, endDate));
    }
```

domain层

```java
@Data
public class HrisDepartment implements Serializable {
    private static final long serialVersionUID = 1L;

    private Long   id;
    private String companyId;
    private Long   departmentId;
    private String departmentName;
    private String departmentCode;
    private Long   parentId;
    private String leaderStaffId;
    private String leaderStaffName;
    private String departmentStatus;

    /** 层级展开：1=顶层（龙派集团总部），依次往下 */
    private Long   departmentId1;
    private String departmentName1;
    private String departmentCode1;
    private Long   departmentId2;
    private String departmentName2;
    private String departmentCode2;
    private Long   departmentId3;
    private String departmentName3;
    private String departmentCode3;
    private Long   departmentId4;
    private String departmentName4;
    private String departmentCode4;
    private Long   departmentId5;
    private String departmentName5;
    private String departmentCode5;
    private Integer departmentLevel;

    private String createBy;
    private Date   createTime;
    private String updateBy;
    private Date   updateTime;
    private String del;
    private String remark;
}
```

service层

```java
/**
 * @author 彭春衡
 */
public interface IHrisDepartmentService {

    /**
     * 同步集团总部及其所有子孙部门
     *
     * @param companyId 公司id
     * @return 同步条数
     */
    int syncDepartment(String companyId);

    /** 查询已同步的所有部门id（供人员同步作为入参） */
    List<Long> queryAllDepartmentIds();
}

```

impl实现类层

```java
/**
 * @author 彭春衡
 */
@Service
public class HrisDepartmentServiceImpl implements IHrisDepartmentService {
    private static final Logger log = LoggerFactory.getLogger(HrisDepartmentServiceImpl.class);

    @Autowired
    private HrisDepartmentMapper departmentMapper;

    /** 就餐项目专用接口工具类（@Component，需注入使用） */
    @Autowired
    private IhrDiningApiUtil ihrDiningApiUtil;

    /**
     * 同步集团总部及其所有子孙部门
     */
    @Override
    @Transactional(rollbackFor = Exception.class)
    public int syncDepartment(String companyId) {
        // 1. 查询总部(id=3)下所有子孙部门（工具类内部已按 ALL 处理，含停用部门）
        JSONArray data = ihrDiningApiUtil.getChildDeptAll(IhrDiningConstant.HQ_DEPARTMENT_ID);
        if (data == null || data.isEmpty()) {
            log.warn("部门同步: 接口返回为空");
            return 0;
        }

        // 2. 转换
        List<HrisDepartment> list = new ArrayList<HrisDepartment>();
        for (int i = 0; i < data.size(); i++) {
            JSONObject o = data.getJSONObject(i);
            HrisDepartment d = new HrisDepartment();
            d.setCompanyId(o.getString("companyId"));
            d.setDepartmentId(o.getLong("departmentId"));
            d.setDepartmentName(o.getString("departmentName"));
            d.setDepartmentCode(o.getString("departmentCode"));
            d.setParentId(o.getLong("parentId"));
            d.setLeaderStaffId(o.getString("staffId"));
            d.setLeaderStaffName(o.getString("staffName"));
            d.setDepartmentStatus(o.getString("departmentStatus"));
            d.setCreateTime(new Date());
            d.setUpdateTime(new Date());
            d.setDel("0");
            list.add(d);
        }

        // 3. 回填层级（LEVEL1=顶层总部）
        DepartmentTreeUtil.fillLevel(list);

        // 4. 按 departmentId upsert，支持重跑
        int cnt = 0;
        for (HrisDepartment d : list) {
            cnt += departmentMapper.upsertDepartment(d);
        }
        log.info("部门同步完成: 共{}条", cnt);
        return cnt;
    }

    @Override
    public List<Long> queryAllDepartmentIds() {
        return departmentMapper.queryAllDepartmentIds();
    }
}
```

mapper层

```java
/**
 * @author 彭春衡
 */
@Repository
public interface HrisDepartmentMapper {

    /** 按 DEPARTMENT_ID upsert，支持重跑 */
    int upsertDepartment(HrisDepartment department);

    /** 查询所有部门id */
    List<Long> queryAllDepartmentIds();

    /** 查询所有部门（含层级字段，供人员回溯二级部门） */
    List<HrisDepartment> queryAll();

    /** 按部门id查询单个部门 */
    HrisDepartment queryByDepartmentId(@Param("departmentId") Long departmentId);
}
```

mapper.xml层

```xml

```





### 同步人员：

controller层

```java

```

domain层

```java
@Data
public class HrisStaff implements Serializable {
    private static final long serialVersionUID = 1L;

    private Long   id;
    private String staffId;
    private String staffNo;
    private String staffName;
    /** IN_SERVICE在职 / QUIT离职 */
    private String staffStatus;
    private Long   departmentId;
    private String departmentName;
    /** 二级部门（报表分组用） */
    private Long   prDepartmentId;
    private String prDepartmentName;
    private String companySiteId;
    private String companySiteName;
    /** 离职日期，在职为空 */
    private Date   leaveDate;

    private String createBy;
    private Date   createTime;
    private String updateBy;
    private Date   updateTime;
    private String del;
    private String remark;
}
```

service层

```java
/**
 * @author 彭春衡
 */
public interface IHrisStaffService {

    /**
     * 同步集团总部人员（含离职人员）
     *
     * @return 同步条数
     */
    int syncStaff();
}
```

impl实现类层

```java
@Service
public class HrisStaffServiceImpl implements IHrisStaffService {
    private static final Logger log = LoggerFactory.getLogger(HrisStaffServiceImpl.class);

    @Autowired private HrisStaffMapper staffMapper;
    @Autowired private HrisDepartmentMapper departmentMapper;
    @Autowired private IhrDiningApiUtil ihrDiningApiUtil;

    /**
     * 同步集团总部人员（含离职，用于历史报表还原当时名单）
     */
    @Override
    @Transactional(rollbackFor = Exception.class)
    public int syncStaff() {
        // 1. 取部门ids作为查询条件
        List<Long> deptIds = departmentMapper.queryAllDepartmentIds();
        if (deptIds == null || deptIds.isEmpty()) {
            log.warn("人员同步: 部门数据为空，请先执行部门同步");
            return 0;
        }

        JSONArray data = ihrDiningApiUtil.getStaffBasicByDeptIds(deptIds);
        if (data == null || data.isEmpty()) {
            log.warn("人员同步: 接口返回为空");
            return 0;
        }

        // 2. 部门层级Map，用于回溯二级部门
        Map<Long, HrisDepartment> deptMap = new HashMap<Long, HrisDepartment>();
        for (HrisDepartment d : departmentMapper.queryAll()) {
            deptMap.put(d.getDepartmentId(), d);
        }

        int cnt = 0;
        for (int i = 0; i < data.size(); i++) {
            JSONObject o = data.getJSONObject(i);
            HrisStaff s = new HrisStaff();
            s.setStaffId(o.getString("staffId"));
            s.setStaffNo(o.getString("staffNo"));
            s.setStaffName(o.getString("staffName"));
            s.setStaffStatus(o.getString("staffStatus"));
            s.setDepartmentId(o.getLong("departmentId"));
            s.setDepartmentName(o.getString("departmentName"));
            s.setCompanySiteId(o.getString("companySiteId"));
            s.setCompanySiteName(o.getString("companySiteName"));
            // leaveDate 格式 yyyy-MM-dd，在职为 null
            s.setLeaveDate(DiningDateUtil.parse(o.getString("leaveDate")));

            // 3. 二级部门：接口有值优先取，无值则按部门层级回溯
            Long prId = o.getLong("prDepartmentId");
            String prName = o.getString("prDepartmentName");
            if (prId == null || prId == 0L) {
                HrisDepartment dept = deptMap.get(s.getDepartmentId());
                if (dept != null) {
                    prId = dept.getDepartmentId2();
                    prName = dept.getDepartmentName2();
                }
            }
            s.setPrDepartmentId(prId);
            s.setPrDepartmentName(prName);

            s.setDel("0");
            s.setCreateTime(new Date());
            s.setUpdateTime(new Date());
            cnt += staffMapper.upsertStaff(s);
        }
        log.info("人员同步完成: 共{}条", cnt);
        return cnt;
    }
}
```

mapper层

```java
@Repository
public interface HrisStaffMapper {

    /** 按 STAFF_ID upsert */
    int upsertStaff(HrisStaff staff);

    /** 查询所有人员（供排班同步取staffId与冗余信息） */
    List<HrisStaff> queryAll();

    /** 查询在职人员 */
    List<HrisStaff> queryInService();

    /** 按员工id查询 */
    HrisStaff queryByStaffId(@Param("staffId") String staffId);
}
```

mapper.xml层

```xml

```





### 同步排班表：

controller层

```java

```

domain层

```java
@Data
public class HrisSchedule implements Serializable {
    private static final long serialVersionUID = 1L;

    private Long   id;
    private String staffId;
    private String shiftId;
    private String shiftAbbr;
    private Date   planDate;
    /** Y是工作日（即应就餐日） / N否 */
    private String isWorkingDay;

    /** ===== 冗余人员信息，报表取数免join ===== */
    private String staffNo;
    private String staffName;
    private String staffStatus;
    private Long   departmentId;
    private String departmentName;
    private Long   prDepartmentId;
    private String prDepartmentName;
    private String companySiteId;
    private String companySiteName;

    private String createBy;
    private Date   createTime;
    private String updateBy;
    private Date   updateTime;
    private String del;
    private String remark;
}
```

service层

```java
public interface IHrisScheduleService {

    /**
     * 同步指定日期区间的排班
     * 内部按"自然月 + 每批100人"双重分批，规避接口限制
     *
     * @return 同步条数
     */
    int syncSchedule(Date startDate, Date endDate);

    /** 某人某日是否应就餐（工作日） */
    boolean isWorkingDay(String staffId, Date planDate);

    /** 某人某日期区间的应就餐日列表（停餐按天展开的基准） */
    List<Date> queryWorkingDays(String staffId, Date startDate, Date endDate);

    /** 某日应就餐人数（在职且为工作日，每日报餐人数的分母） */
    int countWorkingStaff(Date planDate);
}
```

impl实现类层

```java
@Service
public class HrisScheduleServiceImpl implements IHrisScheduleService {
    private static final Logger log = LoggerFactory.getLogger(HrisScheduleServiceImpl.class);

    @Autowired private HrisScheduleMapper scheduleMapper;
    @Autowired private HrisStaffMapper staffMapper;
    @Autowired private IhrDiningApiUtil ihrDiningApiUtil;

    /**
     * 同步指定日期区间的排班
     * 注意：接口限制①日期跨度≤31天 ②单次员工≤100人，故按月 + 按人双重分批
     */
    @Override
    public int syncSchedule(Date startDate, Date endDate) {
        List<HrisStaff> staffList = staffMapper.queryAll();
        if (staffList == null || staffList.isEmpty()) {
            log.warn("排班同步: 人员数据为空，请先执行人员同步");
            return 0;
        }

        // 人员信息Map，用于冗余写入排班表
        Map<String, HrisStaff> staffMap = new HashMap<String, HrisStaff>();
        List<String> staffIds = new ArrayList<String>();
        for (HrisStaff s : staffList) {
            staffMap.put(s.getStaffId(), s);
            staffIds.add(s.getStaffId());
        }

        int total = 0;
        // ① 按自然月拆分（规避31天限制）
        for (DateRange month : splitByMonth(startDate, endDate)) {
            // ② 按100人分批
            for (int i = 0; i < staffIds.size(); i += IhrDiningConstant.SCHEDULE_STAFF_BATCH_SIZE) {
                int end = Math.min(i + IhrDiningConstant.SCHEDULE_STAFF_BATCH_SIZE, staffIds.size());
                List<String> batch = staffIds.subList(i, end);

                // 若依 DateUtils 格式化：格式在前、日期在后
                String startTime = DiningDateUtil.format(month.getStart());
                String endTime   = DiningDateUtil.format(month.getEnd());

                try {
                    JSONArray data = ihrDiningApiUtil.getScheduleActualAll(startTime, endTime, batch);
                    if (data == null) {
                        log.error("排班同步失败: month={}, batchStart={}", month, i);
                        continue;   // 单批失败不影响其他批次，后续可重跑
                    }
                    total += saveBatch(data, staffMap);
                } catch (Exception e) {
                    log.error("排班同步异常: month={}, batchStart={}", month, i, e);
                }
            }
        }
        log.info("排班同步完成: 共{}条", total);
        return total;
    }

    private int saveBatch(JSONArray data, Map<String, HrisStaff> staffMap) {
        if (data == null || data.isEmpty()) {
            return 0;
        }
        int cnt = 0;
        for (int i = 0; i < data.size(); i++) {
            JSONObject o = data.getJSONObject(i);
            HrisSchedule sc = new HrisSchedule();
            sc.setStaffId(o.getString("staffId"));
            sc.setShiftId(o.getString("shiftId"));
            sc.setShiftAbbr(o.getString("shiftAbbr"));
            sc.setPlanDate(DiningDateUtil.parse(o.getString("planDate")));
            // Boolean -> Y/N，Y 即应就餐日
            sc.setIsWorkingDay(o.getBooleanValue("isWorkingDay") ? "Y" : "N");

            // 冗余人员信息，报表取数免join
            HrisStaff s = staffMap.get(sc.getStaffId());
            if (s != null) {
                sc.setStaffNo(s.getStaffNo());
                sc.setStaffName(s.getStaffName());
                sc.setStaffStatus(s.getStaffStatus());
                sc.setDepartmentId(s.getDepartmentId());
                sc.setDepartmentName(s.getDepartmentName());
                sc.setPrDepartmentId(s.getPrDepartmentId());
                sc.setPrDepartmentName(s.getPrDepartmentName());
                sc.setCompanySiteId(s.getCompanySiteId());
                sc.setCompanySiteName(s.getCompanySiteName());
            }
            sc.setDel("0");
            sc.setCreateTime(new Date());
            sc.setUpdateTime(new Date());
            // 按 STAFF_ID+PLAN_DATE 唯一键
            cnt += scheduleMapper.upsertSchedule(sc);
        }
        return cnt;
    }

    /**
     * 按自然月拆分日期区间，规避接口"跨度不超过31天"限制
     */
    private List<DateRange> splitByMonth(Date start, Date end) {
        List<DateRange> ranges = new ArrayList<DateRange>();
        Calendar cur = Calendar.getInstance();
        cur.setTime(start);
        while (!cur.getTime().after(end)) {
            Calendar monthEnd = (Calendar) cur.clone();
            monthEnd.set(Calendar.DAY_OF_MONTH, monthEnd.getActualMaximum(Calendar.DAY_OF_MONTH));
            Date rangeEnd = monthEnd.getTime().after(end) ? end : monthEnd.getTime();
            ranges.add(new DateRange(cur.getTime(), rangeEnd));

            monthEnd.add(Calendar.DAY_OF_MONTH, 1);
            monthEnd.set(Calendar.DAY_OF_MONTH, 1);
            cur = monthEnd;
        }
        return ranges;
    }

    /**
     * 日期分片（内部类）
     * 仅服务于本类的按月分批逻辑，故声明为私有静态内部类，不单独建文件
     */
    private static class DateRange {
        private final Date start;
        private final Date end;

        DateRange(Date start, Date end) {
            this.start = start;
            this.end = end;
        }

        public Date getStart() {
            return start;
        }

        public Date getEnd() {
            return end;
        }

        @Override
        public String toString() {
            return DiningDateUtil.format(start) + "~" + DiningDateUtil.format(end);
        }
    }


    /** ===== 以下为供数据换算调用的查询方法 ===== */

    @Override
    public boolean isWorkingDay(String staffId, Date planDate) {
        return "Y".equals(scheduleMapper.isWorkingDay(staffId, planDate));
    }

    @Override
    public List<Date> queryWorkingDays(String staffId, Date startDate, Date endDate) {
        return scheduleMapper.queryWorkingDays(staffId, startDate, endDate);
    }

    @Override
    public int countWorkingStaff(Date planDate) {
        return scheduleMapper.countWorkingStaff(planDate);
    }
}
```

mapper层

```java
@Repository
public interface HrisScheduleMapper {

    /** 按 STAFF_ID + PLAN_DATE upsert */
    int upsertSchedule(HrisSchedule schedule);

    /** 某人某日是否工作日，返回 Y/N */
    String isWorkingDay(@Param("staffId") String staffId,
                        @Param("planDate") Date planDate);

    /** 某人某区间的应就餐日列表 */
    List<Date> queryWorkingDays(@Param("staffId") String staffId,
                                @Param("startDate") Date startDate,
                                @Param("endDate") Date endDate);

    /** 某日应就餐人数（在职且工作日） */
    int countWorkingStaff(@Param("planDate") Date planDate);

    /** 按区间查询排班明细 */
    List<HrisSchedule> queryByDateRange(@Param("startDate") Date startDate,
                                        @Param("endDate") Date endDate);
}
```

mapper.xml层

```xml

```



### 工具类：

#### 部门回填层级

```java
package com.ruoyi.project.ihr.util;

import com.ruoyi.project.constans.IhrDiningConstant;
import com.ruoyi.project.ihr.domain.HrisDepartment;

import java.util.*;

/**
 * @author 彭春衡
 */

public class DepartmentTreeUtil {

    /**
     * 回填部门层级字段（LEVEL1=顶层总部，依次往下）
     *
     * @param list 接口返回的扁平部门列表
     */
    public static void fillLevel(List<HrisDepartment> list) {
        if (list == null || list.isEmpty()) {
            return;
        }
        Map<Long, HrisDepartment> map = new HashMap<Long, HrisDepartment>();
        for (HrisDepartment d : list) {
            if (d.getDepartmentId() != null) {
                map.put(d.getDepartmentId(), d);
            }
        }

        for (HrisDepartment d : list) {
            // 自当前部门向上回溯，得到 [当前, 父, 祖父 ... 顶点]
            LinkedList<HrisDepartment> path = new LinkedList<HrisDepartment>();
            HrisDepartment cur = d;
            Set<Long> guard = new HashSet<Long>();   // 防脏数据成环导致死循环
            while (cur != null && guard.add(cur.getDepartmentId())) {
                path.addFirst(cur);
                // 已到顶层总部（id=3），不再向上回溯
                if (IhrDiningConstant.HQ_DEPARTMENT_ID.equals(cur.getDepartmentId())) {
                    break;
                }
                Long pid = cur.getParentId();
                if (pid == null) {
                    break;
                }
                cur = map.get(pid);   // 父不在结果集中也会自然终止
            }

            d.setDepartmentLevel(path.size());
            for (int i = 0; i < path.size() && i < IhrDiningConstant.MAX_DEPT_LEVEL; i++) {
                setLevel(d, i + 1, path.get(i));
            }
        }
    }

    private static void setLevel(HrisDepartment target, int level, HrisDepartment src) {
        switch (level) {
            case 1:
                target.setDepartmentId1(src.getDepartmentId());
                target.setDepartmentName1(src.getDepartmentName());
                target.setDepartmentCode1(src.getDepartmentCode());
                break;
            case 2:
                target.setDepartmentId2(src.getDepartmentId());
                target.setDepartmentName2(src.getDepartmentName());
                target.setDepartmentCode2(src.getDepartmentCode());
                break;
            case 3:
                target.setDepartmentId3(src.getDepartmentId());
                target.setDepartmentName3(src.getDepartmentName());
                target.setDepartmentCode3(src.getDepartmentCode());
                break;
            case 4:
                target.setDepartmentId4(src.getDepartmentId());
                target.setDepartmentName4(src.getDepartmentName());
                target.setDepartmentCode4(src.getDepartmentCode());
                break;
            case 5:
                target.setDepartmentId5(src.getDepartmentId());
                target.setDepartmentName5(src.getDepartmentName());
                target.setDepartmentCode5(src.getDepartmentCode());
                break;
            default:
                break;
        }
    }
}
```

#### 日期处理工具：

```java
package com.ruoyi.project.ihr.util;

import com.ruoyi.common.utils.DateUtils;
import com.ruoyi.common.utils.StringUtils;
import com.ruoyi.project.constans.IhrDiningConstant;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Date;

/**
 * 就餐项目 - 日期处理工具
 */
public class DiningDateUtil {
    private static final Logger log = LoggerFactory.getLogger(DiningDateUtil.class);

    private DiningDateUtil() {
    }

    /**
     * Date -> yyyy-MM-dd
     * 说明：若依 DateUtils.parseDateToStr 参数顺序为(格式, 日期)
     */
    public static String format(Date date) {
        if (date == null) {
            return null;
        }
        return DateUtils.parseDateToStr(IhrDiningConstant.DATE_PATTERN, date);
    }

    /**
     * yyyy-MM-dd 字符串 -> Date；解析失败返回 null，不抛异常中断同步
     */
    public static Date parse(String str) {
        if (StringUtils.isEmpty(str)) {
            return null;
        }
        try {
            // SimpleDateFormat 非线程安全，方法内创建，勿声明为 static 共享字段
            return new SimpleDateFormat(IhrDiningConstant.DATE_PATTERN).parse(str);
        } catch (ParseException e) {
            log.warn("日期解析失败: {}", str);
            return null;
        }
    }
}
```

#### i人事接口工具类

```Java
package com.ruoyi.project.ihr.util;

import com.alibaba.fastjson.JSON;
import com.alibaba.fastjson.JSONArray;
import com.alibaba.fastjson.JSONObject;
import com.ruoyi.common.constant.Constants;
import com.ruoyi.framework.redis.RedisCache;
import com.ruoyi.project.constans.IhrDiningConstant;
import com.ruoyi.project.jdy.util.HttpUtil;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

import java.util.HashMap;
import java.util.List;

/**
 * 就餐项目 - i人事接口工具类
 * 负责部门 / 人员 / 排班三个接口的调用；token 复用 IhrServiceUtil 统一管理
 *
 * @date 2026-08
 */
@Component
public class IhrDiningApiUtil {
    private static final Logger log = LoggerFactory.getLogger(IhrDiningApiUtil.class);

    /** 获取所有子孙部门 */
    private static final String URI_DEPT_CHILDREN_ALL =
            "/openapi/thirdparty/api/org/v1/organization/children/all";
    /** 按部门获取员工基本信息 */
    private static final String URI_DEPT_STAFF_BASIC =
            "/openapi/thirdparty/api/staff/v1/department/staff/basic";
    /** 获取实际排班 */
    private static final String URI_SCHEDULE_ACTUAL_ALL =
            "/openapi/thirdparty/api/tm/v4/schedules/actual/all";

    @Value("${ihr.base_url}")
    private String baseUrl;

    @Value("${ihr.company_id}")
    private String companyId;

    /** 复用现有工具类的 token 获取与自动刷新逻辑 */
    @Autowired
    private IhrServiceUtil ihrServiceUtil;

    @Autowired
    private RedisCache redisCache;

    public String getCompanyId() {
        return companyId;
    }

    /**
     * 组装带凭证的请求头
     */
    private HashMap<String, Object> getHeaderParam() {
        HashMap<String, Object> headerParam = new HashMap<String, Object>();
        headerParam.put("Authorization", "Bearer " + ihrServiceUtil.getAccessToken());
        return headerParam;
    }

    /**
     * 统一 POST 调用并取 data 数组
     *
     * @param uri      接口路径
     * @param urlParam 地址参数，可为 null
     * @param bodyJson body 字符串
     * @param apiName  接口名称（日志用）
     * @return data 数组；失败返回 null（调用方需判空）
     */
    private JSONArray postForArray(String uri, HashMap<String, Object> urlParam,
                                   String bodyJson, String apiName) {
        HashMap<String, Object> headerParam = getHeaderParam();
        try {
            log.info("请求i人事{}: {} 地址参数: {} body参数: {}", apiName, uri, urlParam, bodyJson);
            JSONObject res = HttpUtil.doPostForString(baseUrl + uri, urlParam, headerParam, bodyJson);

            if (res == null) {
                throw new Exception("请求目标接口失败");
            }
            if (res.containsKey("error") || !res.containsKey("data")) {
                // 凭证失效：清除缓存，下次调用自动重新获取
                if ("invalid_token".equals(res.getString("error"))) {
                    redisCache.deleteObject(Constants.HRIS_TOKEN_KEY);
                }
                throw new Exception(res.toString());
            }
            JSONArray data = res.getJSONArray("data");
            log.info("请求i人事{}: 成功返回 {} 条", apiName, data == null ? 0 : data.size());
            return data;
        } catch (Exception e) {
            log.error("请求i人事{}！异常信息:{}", apiName, e.getMessage());
            return null;
        }
    }


    /** 统一POST，返回 data 对象 */
    private JSONObject postForObject(String uri, HashMap<String, Object> urlParam,
                                     String bodyJson, String apiName) {
        HashMap<String, Object> headerParam = getHeaderParam();
        try {
            log.info("请求i人事{}: {} 地址参数: {} body参数: {}", apiName, uri, urlParam, bodyJson);
            JSONObject res = HttpUtil.doPostForString(baseUrl + uri, urlParam, headerParam, bodyJson);
            if (res == null) {
                throw new Exception("请求目标接口失败");
            }
            if (res.containsKey("error") || !res.containsKey("data")) {
                if ("invalid_token".equals(res.getString("error"))) {
                    redisCache.deleteObject(Constants.HRIS_TOKEN_KEY);
                }
                throw new Exception(res.toString());
            }
            return res.getJSONObject("data");
        } catch (Exception e) {
            log.error("请求i人事{}！异常信息:{}", apiName, e.getMessage());
            return null;
        }
    }

    /**
     * 根据部门id获取所有子孙部门（含本部门）
     *
     * @param departmentId 部门id（就餐项目传集团总部 id=3）
     * @return 部门列表，失败返回 null
     */
    public JSONArray getChildDeptAll(Long departmentId) {
        HashMap<String, Object> urlParam = new HashMap<String, Object>();
        urlParam.put("companyId", companyId);
        urlParam.put("departmentStatus", "ALL");

        JSONObject item = new JSONObject();
        item.put("departmentId", departmentId);
        item.put("includeAll", true);
        JSONArray bodyData = new JSONArray();
        bodyData.add(item);

        // 注意：该接口 data 为对象，key 是请求的部门id，value 才是部门列表
        JSONObject data = postForObject(URI_DEPT_CHILDREN_ALL, urlParam,
                bodyData.toJSONString(), "获取所有子孙部门");
        if (data == null) {
            return null;
        }
        JSONArray list = data.getJSONArray(String.valueOf(departmentId));
        log.info("获取所有子孙部门: departmentId={}, 返回 {} 条",
                departmentId, list == null ? 0 : list.size());
        return list;
    }

    /**
     * 根据部门ids获取员工基本信息（body 形如 [3,27,28]）
     *
     * @return 员工列表，失败返回 null
     */
    public JSONArray getStaffBasicByDeptIds(List<Long> departmentIds) {
        if (departmentIds == null || departmentIds.isEmpty()) {
            log.warn("获取员工基本信息: 部门ids为空");
            return null;
        }
        return postForArray(URI_DEPT_STAFF_BASIC, null,
                JSON.toJSONString(departmentIds), "获取员工基本信息");
    }

    /**
     * 获取指定员工、指定日期区间的实际排班
     * 接口限制：①日期跨度不超过31天 ②staffIdList单次最多100人
     *
     * @param startTime   开始日期 yyyy-MM-dd
     * @param endTime     结束日期 yyyy-MM-dd
     * @param staffIdList 员工id集合，最多100人
     * @return 排班列表，失败返回 null
     */
    public JSONArray getScheduleActualAll(String startTime, String endTime, List<String> staffIdList) {
        if (staffIdList == null || staffIdList.isEmpty()) {
            log.warn("获取实际排班: 员工ids为空");
            return null;
        }
        if (staffIdList.size() > IhrDiningConstant.SCHEDULE_STAFF_BATCH_SIZE) {
            log.error("获取实际排班: 员工数超过限制{}人，当前{}人",
                    IhrDiningConstant.SCHEDULE_STAFF_BATCH_SIZE, staffIdList.size());
            return null;
        }
        JSONObject bodyData = new JSONObject();
        bodyData.put("startTime", startTime);
        bodyData.put("endTime", endTime);
        bodyData.put("staffIdList", staffIdList);

        return postForArray(URI_SCHEDULE_ACTUAL_ALL, null,
                bodyData.toJSONString(), "获取实际排班");
    }
}
```

