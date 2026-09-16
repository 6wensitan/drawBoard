# OA回调数据解析

> 单据状态：
>
> - INITIATE：审批中   
> - DENIED：驳回
> - PASS：通过   
> - CANCEL：取消
>
> 请假单据这里的是只有集团总部，注意的是请假时间是存在多类型对应多段时间，具体看下面举例。



## 目前这里请假的情况有两种情况去“停餐”&一种情况去“不停餐”

是否停餐：按请假时间停餐||按填写具体停餐日期停餐||不停餐

在请假时间停餐的大前提下  停餐时间规则：大于12点-小于13点（不包含整点）

请假这里的用餐时间规定和出差是有差异的：12：01-12：59



## 第一种：`按请假时间停餐`

（这里需要比对：规定用餐时间区间：大于12点-小于13点（不包含整点） ，其余时间是不用餐的，这里其实你只需要检查开始的第一天和结束的最后一天的时间是否在此区间即可。

### 执行‘逻辑处理’前提条件：

​	去看是否停餐：`按请假时间停餐`

### 逻辑处理：

是否停餐选择项中：我选择`按请假时间停餐的大前提下`，公司规定`用餐时间区间`是：大于12点-小于13点（不包含整点）。

场景：我选择的请假时间是2026-09-09 11:00到2026-09-16 11:00   ，
此时我2026-09-09 11:00开始请假，包含`用餐时间区间`。那我2026-09-09 11:00开始的这一天是不在集团食堂用餐的 ；  
此时我2026-09-16 11:00 结束请假，不包含`用餐时间区间`，所以十六号那一天是在集团食堂用餐的  ；
九号到十六号已经确定了九号是不用餐的十六号是用餐的那我们就看看中间的“10号-15号”六天时间，对比上我们已经获取的排班表数据（通过申请人id `APPLICANT_ID`和排班人ID `STAFF_ID`）去看它里面有多少是工作日和非工作日。这样就可以直接筛选出对应的工作日停餐，而非工作日不用计算停餐。



`注意：多个类型假期的处理其实就是遍历去做这个拆解就好啦`



```
这个是解析表HRIS_INS_DETAIL字段：FORM_INFO的文本数据

申请理由：applyReason
是否选择停餐：A10110600910009112800  （按请假时间停餐）
组合请假：groupVacationDetail 里面有一个数组（存多种类型的假期，假期就如下：
	假期类型：vacationId
	假期名称：vacationId_dataName
	假期类型id：leaveSettingId
	假期开始时间：startTime   （yyyy-MM-dd HH:mm）
	假期结束时间：endTime   （yyyy-MM-dd HH:mm）




```



### 回显json数据

```json
{
  "groupVacationDetail": [
    {
      "defaultVacationGroup": {
        "startTime__endTime__total_unit": "HOUR",
        "total": "41.5",
        "originDurationTime": "41.5",
        "applicationDuration": "41.5",
        "dateFormat": "yyyy-MM-dd HH:mm",
        "startTime": "2026-09-09 11:00",
        "endTime_format": "yyyy-MM-dd HH:mm",
        "attendanceDetailId": "FZZG6HFNZOLSOCA92EFPPR-1788841283064",
        "endTime": "2026-09-16 13:00",
        "format_unit": "HOUR",
        "startTime_format": "yyyy-MM-dd HH:mm",
        "startTime__endTime__total_originDurationTime": "41.5"
      },
      "vacationId_dataName": "事假",
      "leaveSettingId": "60fa1348-a22f-4670-bc09-17aaaf942bd6",
      "lactationVacationGroup": {},
      "vacationId": "AFFAIR_LEAVE"
    }
  ],
  "totalDuration": 41.5,
  "A10110600910009112800": {
    "dataName": "按请假时间停餐",
    "dataId": "RTF2jHSbp3MayiKA"
  },
  "applyReason": "测试第一种类型"
}
```





## 第二种：`按填写具体停餐日期停餐`

这里其实你就可以按照具体的停餐时间去处理，这里需要结合排班表去筛除非工作日。

#### 执行‘逻辑处理’前提条件：

​	去看是否停餐：`按填写具体停餐日期停餐`

#### 逻辑处理：

是否停餐选择项中：我选择`按填写具体停餐日期停餐的大前提下`，公司规定`用餐时间区间`是：大于12点-小于13点（不包含整点）。

场景：我选择的出差时间是2026-09-09 11:00到2026-09-16 11:00，但是选择具体的停餐时间是四号到八号，

此时我就只要看：四号到八号的这个中间“四五六七八”这五天时间，对比上我们已经获取的排班表数据（通过申请人id `APPLICANT_ID`和排班人ID `STAFF_ID`）去看它里面有多少是工作日和非工作日。这样就可以直接筛选出对应的工作日停餐，而非工作日不用计算停餐。，

`注意：多个具体停餐日期的处理其实就是遍历去做这个拆解就好啦`



```
申请理由：applyReason
是否选择停餐：A10110600910009112800  （按填写具体停餐日期停餐）
组合请假：groupVacationDetail 里面有一个数组（存多种类型的假期，假期就如下：
	一个数组的结构
	[{
	假期类型：vacationId
	假期名称：vacationId_dataName
	假期类型id：leaveSettingId
	假期开始时间：startTime   （yyyy-MM-dd HH:mm）
	假期结束时间：endTime   （yyyy-MM-dd HH:mm）
	},
	{
	假期类型：vacationId
	假期名称：vacationId_dataName
	假期类型id：leaveSettingId
	假期开始时间：startTime   （yyyy-MM-dd HH:mm）
	假期结束时间：endTime   （yyyy-MM-dd HH:mm）
	}
	
	]
具体停餐日期：A10119605624319606400[     这个也是一个数组：会有多组停餐日期
	"A10119605625307212800": {
        "startTime": "2026-09-07",
        "endTime": "2026-09-09",
        "format": "YYYY-MM-DD"
      },
      "A10119605625307212800": {
        "startTime": "2026-09-11",
        "endTime": "2026-09-15",
        "format": "YYYY-MM-DD"
      },
]
	
	
```





### 回显json数据

```json
{
  "groupVacationDetail": [
    {
      "defaultVacationGroup": {
        "startTime__endTime__total_unit": "HOUR",
        "total": "17",
        "originDurationTime": "17",
        "applicationDuration": "17",
        "dateFormat": "yyyy-MM-dd HH:mm",
        "startTime": "2026-09-07 09:00",
        "endTime_format": "yyyy-MM-dd HH:mm",
        "attendanceDetailId": "N26ES8QTLO7XNN6GRXK2HM-1788743263250",
        "endTime": "2026-09-09 10:00",
        "format_unit": "HOUR",
        "startTime_format": "yyyy-MM-dd HH:mm",
        "startTime__endTime__total_originDurationTime": "17"
      },
      "vacationId_dataName": "工伤假",
      "leaveSettingId": "8476af3b-b66b-4b63-8002-0891514618bc",
      "lactationVacationGroup": {},
      "vacationId": "8476af3b-b66b-4b63-8002-0891514618bc"
    },
    {
      "defaultVacationGroup": {
        "startTime__endTime__total_unit": "HOUR",
        "total": "8",
        "originDurationTime": "8",
        "applicationDuration": "8",
        "dateFormat": "yyyy-MM-dd HH:mm",
        "startTime": "2026-09-11 09:00",
        "endTime_format": "yyyy-MM-dd HH:mm",
        "attendanceDetailId": "0NBVJGCTQLNU677A9Z7XNE-1788743291464",
        "endTime": "2026-09-13 10:00",
        "format_unit": "HOUR",
        "startTime_format": "yyyy-MM-dd HH:mm",
        "startTime__endTime__total_originDurationTime": "8"
      },
      "vacationId_dataName": "事假",
      "leaveSettingId": "60fa1348-a22f-4670-bc09-17aaaf942bd6",
      "lactationVacationGroup": {},
      "vacationId": "AFFAIR_LEAVE"
    },
    {
      "defaultVacationGroup": {
        "startTime__endTime__total_unit": "HOUR",
        "total": "17",
        "originDurationTime": "17",
        "applicationDuration": "17",
        "dateFormat": "yyyy-MM-dd HH:mm",
        "startTime": "2026-09-14 09:00",
        "endTime_format": "yyyy-MM-dd HH:mm",
        "attendanceDetailId": "FUITR2UVYJ9GFM4163IOFE-1788743588584",
        "endTime": "2026-09-16 10:00",
        "format_unit": "HOUR",
        "startTime_format": "yyyy-MM-dd HH:mm",
        "startTime__endTime__total_originDurationTime": "17"
      },
      "vacationId_dataName": "病假",
      "leaveSettingId": "b576da27-6984-48b8-a3fe-fb781af53dd2",
      "lactationVacationGroup": {},
      "vacationId": "SICK_LEAVE"
    }
  ],
  "totalDuration": 42,
  "A10110600910009112800": {
    "dataName": "按填写具体停餐日期停餐",
    "dataId": "AC7x7HSh4aFzkjyk",
    "dataKey": "user_custom_oa_6f51bd5771f7477c9cff69cd467bc64c"
  },
  "A10119605624319606400": [
    {
      "dateFormat": "yyyy-MM-dd",
      "A10119605625307212800": {
        "startTime": "2026-09-07",
        "endTime": "2026-09-09",
        "format": "YYYY-MM-DD"
      },
      "A10119605625307212800_format": "yyyy-MM-dd"
    },
    {
      "dateFormat": "yyyy-MM-dd",
      "A10119605625307212800": {
        "startTime": "2026-09-11",
        "endTime": "2026-09-15",
        "format": "YYYY-MM-DD"
      },
      "A10119605625307212800_format": "yyyy-MM-dd"
    }
  ],
  "applyReason": "测试数据，不需要审批，等一下 我撤回"
}
```



## 第三种：`不停餐`

#### 执行‘逻辑处理’前提条件：

​	去看是否停餐：`不停餐`

#### 逻辑处理：

是否停餐选择项中：我选择`不停餐的大前提下`，公司规定`用餐时间区间`是：大于12点-小于13点（不包含整点）。

场景：我选择的出差时间是三号九点到十号十二点  ，

对比上我们已经获取的排班表数据（通过申请人id `APPLICANT_ID`和排班人ID `STAFF_ID`）去看它里面有多少是工作日和非工作日。这样就可以直接筛选出对应的工作日用餐，而非工作日不用计算用餐。



```
申请理由：applyReason
是否选择停餐：A10110600910009112800  （不停餐）
直接就是用餐。
```



### 回显json数据

```json
{
  "groupVacationDetail": [
    {
      "defaultVacationGroup": {
        "startTime__endTime__total_unit": "HOUR",
        "total": "33",
        "originDurationTime": "33",
        "applicationDuration": "33",
        "dateFormat": "yyyy-MM-dd HH:mm",
        "startTime": "2026-09-09 11:00",
        "endTime_format": "yyyy-MM-dd HH:mm",
        "attendanceDetailId": "G7LSKV0NF0ST4POKW93BMH-1788840021842",
        "endTime": "2026-09-15 12:00",
        "format_unit": "HOUR",
        "startTime_format": "yyyy-MM-dd HH:mm",
        "startTime__endTime__total_originDurationTime": "33"
      },
      "vacationId_dataName": "事假",
      "leaveSettingId": "60fa1348-a22f-4670-bc09-17aaaf942bd6",
      "lactationVacationGroup": {},
      "vacationId": "AFFAIR_LEAVE"
    }
  ],
  "totalDuration": 33,
  "A10110600910009112800": {
    "dataName": "不停餐",
    "dataId": "ErZkTSBTKi8i2ySC"
  },
  "applyReason": "测试数据"
}
```









## 开发编码

| 单据类型（APPROVAL_TYPE） | 值                 | 申请单据模板值（SETTING_NAME） |      |
| ------------------------- | ------------------ | ------------------------------ | ---- |
| 停餐申请单                | USER_CUSTOMIZATION | 停餐申请单                     |      |
| 用餐申请单                | USER_CUSTOMIZATION | 用餐申请单                     |      |
| 出差申请单                | EVECTION           | 出差或外出申请                 |      |
| 请假申请                  | LEAVE_ADJUST_GROUP | 请假申请（总部）               |      |
|                           |                    |                                |      |

数据源：单据类型值：APPROVAL_TYPE = LEAVE_ADJUST_GROUP 和 SETTING_NAME = ‘请假申请（总部）’

抓取`HRIS_INS_DETAIL表`拿到数据源的数据：对 `FORM_INFO` 该字段进行数据解析 拿去到下面的数据字段内容（请假解析后的表字段）



### 开发流程	

```txt
HRIS_INS_DETAIL（原始回调）
     ↓ 按单据类型分流解析
OA_DINING_LEAVE（主表·一单一行） + OA_DINING_LEAVE_ITEM（明细表·多段）
     ↓ 统一转换为"人 × 日期 × 停餐/用餐"
OA_DINING_DETAIL（就餐明细底表，四类单据共用，不新增）
     ↓
三张报表
```

> **请假要拆主表+明细表**：一张请假单的假期(`groupVacationDetail`)与具体停餐日期 (`A10119605624319606400`)**都是数组**，可以有多段，一行装不下；

### 请假解析后的表字段：

标注`所有数据库建表的时候，id主键，雪花id（主键索引）； 创建人创建时间；更新人更新时间；软删除标识；备注remark；（这些标准字段是必须要有的）`

```
这里需要你理解前面三种类型的前提下，选择相关字段，，为后续建表做准备
```



### 数据库建表SQL：

```sql

```



### domain实体类：

```java

```



### service接口层：

```java

```



### impl实现类层：

```java

```



### mapper层：

```java

```



### mapper.xml

```xml

```



### controller控制层

```java

```



### 自行补充
