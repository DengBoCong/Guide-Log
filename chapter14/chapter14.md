#### 数据库信息需要一同协商敲定，在这里我暂时举例说明如何安排数据库表
-------------------------------

## 用户信息表

| 字段        | 类型    |  说明  |
| :----:   | :----:   | :----: |
| id | Long | 用户id |
| userType | Integer | 用户类型，1：普通用户，2：会员用户 |
| webSource | String | 用户注册来源 |
| authenticationState | Integer | 认证状态（0等待认证、1已认证、2信息不全、3未通过），只对会员用户有效 |
| lastAuthDate | Date | 用户最后认证时间 只对会员用户有效 |
| realname | String | 真实姓名 |
| gender | String | 性别， M:男，F:女， N：未知 |
| year | Integer | 生日年 |
| month | Integer | 生日月 |
| day | Integer | 生日日 |
| imgUrl | String | 头像文件名，不包含CDN路径 |
| imgUrlGfsId | String | 头像文件在gfs中的id（业务上不用关心） |