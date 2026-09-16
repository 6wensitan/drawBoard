# OA回调数据解析

> 单据状态：
>
> - INITIATE：审批中   
> - PASS：通过   
> - CANCEL：取消
>
> 出差单据这里的是全集团，不单单只有集团总部的，这里：外部人员 出差来总部并且是在总部用餐的人员，他人不在我的人员名单，这个就不需要关联找他的排班。

## 目前这里出差单的情况有两种情况去“停餐”&一种情况去“不停餐”

是否停餐：按出差或外出时间停餐||按填写具体停餐时间停餐||不停餐



## 总部人员

### 情况一：`选择：按出差或外出时间停餐`

（这里需要比对：规定用餐时间区间：12：30-13：00 ，其余时间是不用餐的，这里其实你只需要检查开始的第一天和结束的最后一天的时间是否在此区间即可。

#### 执行‘逻辑处理’前提条件：

​	申请人id `APPLICANT_ID`存在集团总部人员表：OA_HRIS_STAFF 表中  用STAFF_ID关联。

​	然后才去看是否停餐：`按出差或外出时间停餐`

#### 逻辑处理：

是否停餐选择项中：我选择`按出差或外出时间停餐的大前提下`，公司规定`用餐时间区间`是：12：30-13：00。

场景：我选择的出差时间是三号九点到十号十二点   ，
此时我三号的九点开始出差，包含`用餐时间区间`。那我九月三号九点开始的这一天是不在集团食堂用餐的 ；  
此时我十号十二点结束出差，不包含`用餐时间区间`，所以十号那一天是在集团食堂用餐的  ；
三号到十号已经确定了三号是不用餐的十号是用餐的那我们就看看中间的“四五六七八九”六天时间，对比上我们已经获取的排班表数据（通过申请人id `APPLICANT_ID`和排班人ID `STAFF_ID`）去看它里面有多少是工作日和非工作日。这样就可以直接筛选出对应的工作日停餐，而非工作日不用计算停餐。

![image-20260901143856153](https://raw.githubusercontent.com/6wensitan/drawBoard/main/img/image-20260901143856153.png)



按出差或外出时间停餐-需要的json数据

> 组件标记码: 出差目的地：出差开始时间：出差结束时间：申请事由：是否需要停餐：具体停餐时间区间：具体停餐开始时间：具体停餐结束时间：外地就餐公司：
>
> - 出差目的地：A10110813911202919200   
> - 是否需要停餐：A10110815111621800000   
> - 外地就餐公司：A10110815119917900000
> - 申请事由：applyReason
>
> `这里的出差时间其实也是根据排班信息表去做处理（利用STAFF_ID去关联Oa回调单据表和排班表）`
>
> - 出差开始时间：startTime
> - 出差结束时间：endTime
>
> 出差开始结束时间格式：yyyy-MM-dd HH:mm

这个是解析表HRIS_INS_DETAIL字段：FORM_INFO的文本数据

{

​	"A10110813911202919200": {
​    	"dataName": "贵港基地 Guigang Base",
​    	"dataId": "rMGcEtRFFmZC4JHC"
  	},

​	"A10110815111621800000": {
​    	"dataName": "按出差或外出时间停餐",
​    	"dataId": "ctNAMZki3t6w5GGn"
  	},

​	"A10110815119917900000": {
   	 "dataName": "贵港订制餐（包间）Guigang Customized Meal (Private Room)",
   	 "dataId": "BXjKpCnKxj6h635k"
  	},

​	"applyReason": "GPICA客户来访，沟通sysco第二次验厂事宜",

​	"startTime": "2026-08-26 06:30",
 	"endTime": "2026-08-26 18:00",

}



```json
{
  "A10110814218413712800": {
    "dataName": "不需要 not？required",
    "dataId": "m7YS6cnxGMHRmmn6"
  },
  "startTime__endTime__total_unit": "HOUR",
  "A10110815207711906400": {
    "dataId": "Dij3ww85kEFDxzxw",
    "dataName": "自行安排 Self arranged"
  },
  "dateFormat": "yyyy-MM-dd HH:mm",
  "A10110815119917900000": {
    "dataName": "贵港订制餐（包间）Guigang Customized Meal (Private Room)",
    "dataId": "BXjKpCnKxj6h635k"
  },
  "applyReason": "GPICA客户来访，沟通sysco第二次验厂事宜",
  "endTime_format": "yyyy-MM-dd HH:mm",
  "attendanceDetailId": "9ADQOVDJ13QTDEN0MYYT5Q-1787646002984",
  "A10110813911202919200": {
    "dataName": "贵港基地 Guigang Base",
    "dataId": "rMGcEtRFFmZC4JHC"
  },
  "A1024810104417519200": "打车",
  "startTime_format": "yyyy-MM-dd HH:mm",
  "total": "8",
  "A10110815111621800000": {
    "dataName": "按出差或外出时间停餐",
    "dataId": "ctNAMZki3t6w5GGn"
  },
  "originDurationTime": "8",
  "startTime": "2026-08-26 06:30",
  "endTime": "2026-08-26 18:00",
  "format_unit": "HOUR",
  "startTime__endTime__total_originDurationTime": "8"
}
```





### 情况二：`选择：按填写具体停餐时间停餐` 

这里其实你就可以按照具体的停餐时间去处理，这里需要结合排班表去筛除非工作日。（注意 这里拉去的数据只是总部人员发起的出差

#### 执行‘逻辑处理’前提条件：

​	申请人id `APPLICANT_ID`存在集团总部人员表：OA_HRIS_STAFF 表中  用STAFF_ID关联。

​	然后才去看是否停餐：`按填写具体停餐时间停餐`

#### 逻辑处理：

是否停餐选择项中：我选择`按填写具体停餐时间停餐的大前提下`，公司规定`用餐时间区间`是：12：30-13：00。

场景：我选择的出差时间是三号九点到十号十二点   ，但是选择具体的停餐时间是四号到八号，

此时我就只要看：四号到八号的这个中间“四五六七八”这五天时间，对比上我们已经获取的排班表数据（通过申请人id `APPLICANT_ID`和排班人ID `STAFF_ID`）去看它里面有多少是工作日和非工作日。这样就可以直接筛选出对应的工作日停餐，而非工作日不用计算停餐。

![image-20260901143936592](https://raw.githubusercontent.com/6wensitan/drawBoard/main/img/image-20260901143936592.png)



按具体停餐时间停餐-需要的json数据

> 组件标记码: 
>
> - 出差目的地：A10110813911202919200   
>
> - 是否需要停餐：A10110815111621800000   
>
> - 具体停餐时间区间：A10211500713105300000
>
>   `（这里面包含具体停餐的开始结束时间，注意这里是没有去除节假日，因为这个是要根据排班表去做处理（利用STAFF_ID去关联Oa回调单据表和排班表）`
>
> - 外地就餐公司：A10110815119917900000
>
> - 申请事由：applyReason
>
> - 出差开始时间：startTime
> - 出差结束时间：endTime
>
> 出差开始结束时间格式：yyyy-MM-dd HH:mm



这个是解析表HRIS_INS_DETAIL字段：FORM_INFO的文本数据

{

​	"A10110813911202919200": {
​    	"dataName": "其它 other",
   	 "dataId": "7YpZWtwhTMb5fGKp"
 	 },

​	 "applyReason": "测试数据，琴姐不用管，等下就撤回",

​	"A10110815119917900000": {
​    	"dataName": "不在龙派就餐  Not dining at  Lopie",
​    	"dataId": "k36F6bF3DfFb2h5G"
 	 },

​	"A10110815111621800000": {
​    	"dataName": "按填写具体停餐时间停餐",
​    	"dataId": "bYY75e2fwTXsQJyp"
  	},

​	"A10211500713105300000": {
   	 "startTime": "2026-09-04",
​    	"endTime": "2026-09-07",
   	 "durationTime": "4",
​    	"format": "YYYY-MM-DD"
 	 },

}



```json
{
  "A10110814218413712800": {
    "dataName": "不需要 not？required",
    "dataId": "m7YS6cnxGMHRmmn6"
  },
  "startTime__endTime__total_unit": "HOUR",
  "A10110815207711906400": {
    "dataName": "自行安排 Self arranged",
    "dataId": "Dij3ww85kEFDxzxw"
  },
  "dateFormat": "yyyy-MM-dd HH:mm",
  "A10110815119917900000": {
    "dataName": "不在龙派就餐  Not dining at  Lopie",
    "dataId": "k36F6bF3DfFb2h5G"
  },
  "applyReason": "测试数据，琴姐不用管，等下就撤回",
  "A10211500713105300000": {
    "startTime": "2026-09-04",
    "endTime": "2026-09-07",
    "durationTime": "4",
    "format": "YYYY-MM-DD"
  },
  "endTime_format": "yyyy-MM-dd HH:mm",
  "attendanceDetailId": "EFUGRGWOJM4GFYTYMFH7AR-1788269857098",
  "A10110813911202919200": {
    "dataName": "其它 other",
    "dataId": "7YpZWtwhTMb5fGKp"
  },
  "A10110814011907412800": {
    "dataName": "否",
    "dataId": "XTXDwjCHCnaMfCSm"
  },
  "startTime_format": "yyyy-MM-dd HH:mm",
  "total": "42",
  "A10110815111621800000": {
    "dataName": "按填写具体停餐时间停餐",
    "dataId": "bYY75e2fwTXsQJyp"
  },
  "originDurationTime": "42",
  "A10110814007517012800": "测试",
  "A10211500713105300000_format": "yyyy-MM-dd",
  "dateFormat1": "yyyy-MM-dd",
  "startTime": "2026-09-03 11:00",
  "endTime": "2026-09-08 14:00",
  "format_unit": "HOUR",
  "startTime__endTime__total_originDurationTime": "42"
}
```



### 情况三：`选择：不停餐`

#### 执行‘逻辑处理’前提条件：

​	申请人id `APPLICANT_ID`存在集团总部人员表：OA_HRIS_STAFF 表中  用STAFF_ID关联。

​	然后才去看是否停餐：`不停餐`

#### 逻辑处理：

是否停餐选择项中：我选择`不停餐的大前提下`，公司规定`用餐时间区间`是：12：30-13：00。

场景：我选择的出差时间是三号九点到十号十二点  ，

对比上我们已经获取的排班表数据（通过申请人id `APPLICANT_ID`和排班人ID `STAFF_ID`）去看它里面有多少是工作日和非工作日。这样就可以直接筛选出对应的工作日用餐，而非工作日不用计算用餐。

![image-20260901143958841](https://raw.githubusercontent.com/6wensitan/drawBoard/main/img/image-20260901143958841.png)



不停餐-需要的json数据

> 组件标记码: 
>
> - 出差目的地：A10110813911202919200   
> - 是否需要停餐：A10110815111621800000   
> - 申请事由：applyReason
> - 出差开始时间：startTime
> - 出差结束时间：endTime
>
> 出差开始结束时间格式：yyyy-MM-dd HH:mm

{

​	"A10110813911202919200": {
   	 "dataName": "其它 other",
​    	"dataId": "7YpZWtwhTMb5fGKp"
 	 },

​	"A10110815111621800000": {
​    	"dataName": "不停餐",
   	 "dataId": "n6Qa8rxQiXr8z8T4"
  	},

​	"applyReason": "南沙统计局8月新入统以及更换统计员企业培训会议",

​	"startTime": "2026-08-26 14:00",
 	"endTime": "2026-08-26 18:00",

}



```json
{
  "A10110814218413712800": {
    "dataName": "不需要 not？required",
    "dataId": "m7YS6cnxGMHRmmn6"
  },
  "startTime__endTime__total_unit": "HOUR",
  "A10110815207711906400": {
    "dataName": "自行安排 Self arranged",
    "dataId": "Dij3ww85kEFDxzxw"
  },
  "dateFormat": "yyyy-MM-dd HH:mm",
  "applyReason": "南沙统计局8月新入统以及更换统计员企业培训会议",
  "endTime_format": "yyyy-MM-dd HH:mm",
  "attendanceDetailId": "P7QUXOQ872J8AGG772F90L-1787627218424",
  "A10110813911202919200": {
    "dataName": "其它 other",
    "dataId": "7YpZWtwhTMb5fGKp"
  },
  "A10110814011907412800": {
    "dataName": "否",
    "dataId": "XTXDwjCHCnaMfCSm"
  },
  "startTime_format": "yyyy-MM-dd HH:mm",
  "total": "4",
  "A10110815111621800000": {
    "dataName": "不停餐",
    "dataId": "n6Qa8rxQiXr8z8T4"
  },
  "originDurationTime": "4",
  "A10110814007517012800": "南沙区政府",
  "startTime": "2026-08-26 14:00",
  "endTime": "2026-08-26 18:00",
  "format_unit": "HOUR",
  "startTime__endTime__total_originDurationTime": "4"
}
```



## 除总部人员

> 除总部人员提出差单外的所有处理都如下

### 情况一：`选择：按出差或外出时间停餐`  

公司规定`用餐时间区间`是：12：30-13：00。

> 出差几天本质上就等于在集团用餐几天。这里不用去关联排班表   公司规定`用餐时间区间`是：12：30-13：00。

#### **条件**：

​	出差地点为`集团本部`和外出就餐地为：`集团食堂`（同时满足）。

​	然后才去看是否停餐：`按出差或外出时间停餐`

#### 逻辑处理：

场景：

一个工厂端的员工出差到集团本部，现提出出差申请单，出差地点为`集团本部`(模糊查询），出差时间为4号9点至10号中午12点 。 我选择`按出差或外出时间停餐`。选择外出就餐地为：`集团食堂`（模糊查询）。

这名员工4号的九点开始出差，包含`用餐时间区间`，那这名员工九月三号九点开始的这一天是在集团食堂用餐的 ；

这名员工十号十二点结束出差，不包含`用餐时间区间`，所以十号那一天是不在集团食堂用餐的  ；

此时我们就可以知道这名员工从4号一直到9号都是在集团食堂用餐。这里还需要`减去节假日`，即可得到在集团用餐的具体天数。

![image-20260903210606348](https://raw.githubusercontent.com/6wensitan/drawBoard/main/img/image-20260903210606348.png)



### 情况二：`选择：按填写具体停餐时间停餐` 

公司规定`用餐时间区间`是：12：30-13：00。

> 具体停餐几天本质上就等于在集团用餐几天。这里不用去关联排班表

#### **条件**：

​	出差地点为`集团本部`和外出就餐地为：`集团食堂`（同时满足）。

​	然后才去看是否停餐：`按填写具体停餐时间停餐`

#### 逻辑处理：

场景：

一个工厂端的员工出差到集团本部，现提出出差申请单，出差地点为`集团本部`(模糊查询），出差时间为4号9点至10号中午12点 。 我选择`按填写具体停餐时间停餐`。具体停餐时间是：5号到8号，选择外出就餐地为：`集团食堂`（模糊查询）。

此时我们就可以知道这名员工从5号一直到8号都是在集团食堂用餐。这里还需要`减去节假日`，即可得到在集团用餐的具体天数。

![image-20260903211540178](https://raw.githubusercontent.com/6wensitan/drawBoard/main/img/image-20260903211540178.png)





### 情况三：`选择：不停餐`

公司规定`用餐时间区间`是：12：30-13：00。

> 直接就是不在集团用餐。

#### **条件**：

​	出差地点为`集团本部`和外出就餐地为：`集团食堂`（同时满足）。

​	然后才去看是否停餐：`不停餐`

结果：直接就是不在集团用餐。

![image-20260903211729037](https://raw.githubusercontent.com/6wensitan/drawBoard/main/img/image-20260903211729037.png)



## 开发编码

| 单据类型（APPROVAL_TYPE） | 值                 | 申请单据模板值（SETTING_NAME） |      |
| ------------------------- | ------------------ | ------------------------------ | ---- |
| 停餐申请单                | USER_CUSTOMIZATION | 停餐申请单                     |      |
| 用餐申请单                | USER_CUSTOMIZATION | 用餐申请单                     |      |
| 出差申请单                | EVECTION           | 出差或外出申请                 |      |
| 请假申请                  | LEAVE_ADJUST_GROUP | 请假申请（总部）               |      |
|                           |                    |                                |      |

数据源：单据类型值：APPROVAL_TYPE = EVECTION 和 SETTING_NAME = ‘出差或外出申请’

抓取`HRIS_INS_DETAIL表`拿到数据源的数据：流程状态：PROCESS_STATUS = PASS 或者 操作状态：OPERATOR_STATUS = createProcess 的数据，并对 `FORM_INFO` 该字段进行数据解析 拿去到下面的数据字段内容（出差解析后的表字段）



### 开发流程

```txt
HRIS_INS_DETAIL（原始回调）
   ↓ 按类型分流解析
四张单据解析表（各自的字段）
   ↓ 统一转换为"人×日期×停餐/用餐"
就餐明细底表（一张，字段稳定）
   ↓
三张报表
```



### 出差解析后的表字段：

标注`所有数据库建表的时候，id主键，雪花id（主键索引）； 创建人创建时间；更新人更新时间；软删除标识；备注remark；（这些标准字段是必须要有的）`

```txt
申请人id：APPLICANT_ID
申请人名字：APPLICANT_NAME
流程id：PROCESS_ID
流程编码：PROCESS_CODE
流程状态：PROCESS_STATUS
流程类型：APPROVAL_TYPE
流程开始时间：APPLY_TIME
流程结束时间：END_TIME
流程名称：SETTING_NAME
操作状态：OPERATOR_STATUS 

出差目的地：
出差开始时间：
出差结束时间：
申请事由：
是否需要停餐：按出差或外出时间停餐||按填写具体停餐时间停餐||不停餐 三种
具体停餐时间区间：
具体停餐开始时间：
具体停餐结束时间：
外地就餐公司：
是否为总部员工：
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



## 