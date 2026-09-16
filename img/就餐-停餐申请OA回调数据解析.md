# OA回调数据解析

> 单据状态：
>
> - INITIATE：审批中   
> - DENIED：驳回
> - PASS：通过   
> - CANCEL：取消
>
> 停餐单据这里的是集团总部，具体看下面举例。

## 逻辑处理

停餐处理：首先是从数据源HRIS_INS_DETAIL表中：APPROVAL_TYPE = USER_CUSTOMIZATION 和 SETTING_NAME = ‘停餐申请单’ ，获取到该单据后，将通过申请人id `APPLICANT_ID`和人员ID `STAFF_ID`比对，看看是否是集团总部的员工，并且停餐地点是`集团总部`。这样我们才会将此单据去做处理。停餐时间是有多组：

### 场景：

停餐时间段一：2026-09-17 到 2026-09-21；停餐时间段二：2026-09-22 到 2026-09-23；

那我们这里就要看具体的停餐时间是多少，首先停餐时间截取出来，比对排班表实际工作日有几天，那就是停几天。如果针对同一天工作日，出现出差、请假、停餐申请单在这一天，那就要进行冲突收敛了，对吗？（冲突收敛方法：refreshEffective）

```
停餐说明：A10325224202722119200
申请人工号：apply_staffNo
申请人部门：apply_departmentName
停餐地点：A10325225415413719200
停餐时间：A1040824422815312800    这个也是一个数组：会有多组停餐日期
	[
    {
      "A1040824423915712800_format": "yyyy-MM-dd",
      "dateFormat": "yyyy-MM-dd",
      "A1040824423915712800": {
        "startTime": "2026-09-17",   停餐开始时间
        "endTime": "2026-09-21",     停餐结束时间
        "durationTime": "5",
        "format": "YYYY-MM-DD"
      }
    },
    {
      "A1040824423915712800_format": "yyyy-MM-dd",
      "dateFormat": "yyyy-MM-dd",
      "A1040824423915712800": {
        "startTime": "2026-09-22",    停餐开始时间
        "endTime": "2026-09-23",      停餐结束时间
        "durationTime": "2",
        "format": "YYYY-MM-DD"
      }
    }
  ]
  
  

```



### 回显json数据

```json
{
  "A10325224202722119200": "测试数据",
  "apply_staffNo": "CH2026HQ00051",
  "A10325225415413719200": {
    "dataName": "集团总部",
    "dataId": "JNJGFN4z3d42kR8X"
  },
  "A1040824422815312800": [
    {
      "A1040824423915712800_format": "yyyy-MM-dd",
      "dateFormat": "yyyy-MM-dd",
      "A1040824423915712800": {
        "startTime": "2026-09-17",
        "endTime": "2026-09-21",
        "durationTime": "5",
        "format": "YYYY-MM-DD"
      }
    },
    {
      "A1040824423915712800_format": "yyyy-MM-dd",
      "dateFormat": "yyyy-MM-dd",
      "A1040824423915712800": {
        "startTime": "2026-09-22",
        "endTime": "2026-09-23",
        "durationTime": "2",
        "format": "YYYY-MM-DD"
      }
    }
  ],
  "apply_departmentName": "产品服务部"
}
```



### 优先级对照（就近覆盖）

| 级别  | 来源                         | 说明                           |
| ----- | ---------------------------- | ------------------------------ |
| **1** | **停餐申请单**               | 专门针对具体日期提交，最可信   |
| 2     | 请假/出差·按填写具体停餐日期 | 单据内明确指定的停餐日         |
| 3     | 请假/出差·按整段时间推导     | 由单据起止时间推导             |
| 4     | 系统默认                     | 普通人默认用餐、董事会默认停餐 |

> 冲突示例：某人 9/20 既提交了停餐申请（级别1），又有一张请假单按整段时间判定停餐（级别3）， 收敛后**停餐申请单生效**，请假单那条置为 `IS_EFFECTIVE='N'` 保留追溯。







## 开发编码

| 单据类型（APPROVAL_TYPE） | 值                 | 申请单据模板值（SETTING_NAME） |      |
| ------------------------- | ------------------ | ------------------------------ | ---- |
| 停餐申请单                | USER_CUSTOMIZATION | 停餐申请单                     |      |
| 用餐申请单                | USER_CUSTOMIZATION | 用餐申请单                     |      |
| 出差申请单                | EVECTION           | 出差或外出申请                 |      |
| 请假申请                  | LEAVE_ADJUST_GROUP | 请假申请（总部）               |      |



数据源：单据类型值：APPROVAL_TYPE = USER_CUSTOMIZATION 和 SETTING_NAME = ‘停餐申请单’

抓取`HRIS_INS_DETAIL表`拿到数据源的数据：对 `FORM_INFO` 该字段进行数据解析 拿去到下面的数据字段内容（停餐解析后的表字段）



```
HRIS_INS_DETAIL（原始回调）
     ↓ 按单据类型分流解析
OA_DINING_STOP（主表·一单一行） + OA_DINING_STOP_ITEM（明细表·多段停餐区间）
     ↓ 统一转换为"人 × 日期 × 停餐/用餐"
OA_DINING_DETAIL（就餐明细底表，四类单据共用，不新增）
     ↓
三张报表
```



### 停餐解析后的表字段：

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