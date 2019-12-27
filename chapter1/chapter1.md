## 如何写好代码
+ 选用好的工具
   - 擅长使用快捷键
   - 个性化配置
   - 熟练掌握工具特性，多思考
+ 养成好的习惯
   - 精研文档：官方文档，Google等。去其糟粕。
   - 研究成熟的代码，优秀的项目
   - 擅长使用语言特性
   - 更新代码后，提交代码前做review
 

+ 变量名：命名明确
   - 反面教材：isAft ?? sth??  tth??  mySprite?? indexArr??
   - 只有局部简单变量可以用a, i, j, s 等

+ 模块：划分清楚，MVC
   - Model：数据描述、定义、封装、基本处理
   - View：界面展示
   - Controller：流程控制，方便针对性测试

+ 封装：兼顾灵活
   - 不要过度封装
   - 考虑将来扩展和重构情况
   - 两次以上的代码块尽量抽取成函数
 

+ 流程：单入单出
   - 确定的入口，确定的出口
   - 异常流程要报错
+ 代码组织：局部性原则
   - 不要做全局污染，避免修改的全局变量
+ 可以通过context等机制传递
   - 控制变量和函数的使用范围
+ 内部变量/函数不要暴露给外面使用
   - 通过集成和封装为其他代码提供服务
+ 方便做测试、安全检查、重构等
+ 明确数据定义和类型
   - true 和 “true”
   - 0 和 “0”
   - 通过定义独立的类或者函数，提供统一的判断


## DAO规范
### MYSQL DAO功能及限制
 + 实现对MySQL DB单表的基本增、删、改、查功能
 + 实现通过配置以及简单改动散表功能
 + 框架中引入DAO操作研发规范
 + 只支持单表的增、删、改、查的操作
 + JDBC DAO只能以接口的方式存在，不能自己写具体实现，所有cache与数据库结合的操作，需要在前面封装一层实现

+ 数据源配置
``` java
{
    "test_dao" : {
        "master" : {
            "jdbcUrl"  : "jdbc:mysql://127.0.0.1:3306/dao_test?userUnicode=true&characterEncoding=UTF8&useDynamicCharsetInfo=false",
            "username" : "root",
            "password" : "123456"
        },
        "slave1" : {
            "jdbcUrl"  : "jdbc:mysql://127.0.0.1:3306/dao_test?userUnicode=true&characterEncoding=UTF8&useDynamicCharsetInfo=false",
            "username" : "root",
            "password" : "123456"
        }
    }
}
```
### 创建MySQL Entity
+ Entity类使用说明：
   - Entity类必须实现java.io.Serializable接口，且需要指定14位以上的serialVersionUID，避免cache支持时无法序列化或反序列化
   - Entity类必须提供public修饰的无参构造方法
   - 只有POJO的成员变量可以参与数据库操作，可自定义get、set方法或使用lombok.Data注释
   - 为减少编码量，所有类成员名称与数据表列名称通过固定规则进行转换
   - 数据表列名称采用全大写或全小写字母、或数字，并以下划线分割，例如：create_time、UPDATE_TIME、uid等
   - Entity类成员名称，采用Java命名规则，首字母小写，后面每个单词首字母大写，例如：createTime, updateTime, uid等
   - Entity类成员名称，将大写字母转为下划线+对应的小写字母，变更为数据表列名称，例如 createTime => create_time
   - 数据表列名称，转小写字母后，再将下划线+后面的小写字母转为对应的大写字母，变更为Entity类成员名称，例如：UPDATE_TIME => updateTime
+ 单表ENTITY
``` java
/**
 * $Id$
 * Copyright(C) 2019 DengBoCong, All Rights Reserved.
 */
package com.voxlearning.utopia.test.storage.jdbc.bean;
 
import java.util.Date;
 
import lombok.Data;
 
import com.voxlearning.utopia.storage.util.StringUtil;
 
/**
 * 测试单表Entity
 * @author DengBoCong
 * @version 1.0.0 2019-12-23 10:58:51
 */
@Data
public class TestSingleBean implements Serializable {
 
    private static final long serialVersionUID = 1000020150108105851L;
    private long id;
    private NameEnum name;
    private Date createTime;
    private Date updateTime;
     
    @Override
    public String toString() {
        return StringUtil.join(", ", id, name, createTime, updateTime);
    }
}
```
### 创建MySQL Dao
+ DAO总述
   - DAO必需为一个接口类
   - DAO必须通过@DAO.ds()指定使用的数据源，数据源名称需数据源配置的datasource.name属性相同(不区分大小写)
   - 每个DAO方法执行一条SQL，并以@SQL注解标注，支持的SQL类型详见下文
   - 所有SQL中，参数必须以#{参数名}方式指定，参数名称与Bean成员属性名称或使用@P标记的名称一致(区分大小写)，支持的参数类型详见下文
   - 方法参数支持JDBC基本数据类型、Entity、List，以及各种List<PrimitiveType>或List<Entity>
   - 不同类型的SQL，提供了不同的返回值
+ DAO注解

| 注解        | 注解位置    |  说明  |
| :----:   | :----:   | :----: |
| @DAO | DAO接口类 | @DAO.ds()指定数据源 |
| @P | DAO参数 | 指定方法参数的名称，List<Entity>或Entity参数不用指定 |
| @SQL | DAO方法 | 指定方法执行的SQL语句 |

+ 支持的SQL类型

| SQL类型 | 说明  |
| :----:   | :----:   | 
| delete from ${table} ... | 删除操作  |
| insert into ${table} (...) values (...) | 条或批量插入，单条插入传入Entity参数、多条插入传入List<Entity>参数见TestSingleDao.insert、TestSingleDao.batchInsert |
| insert into ${table} (...) values (...) on duplicate key update ... | 插入时可根据唯一约束执行on duplicate key update，该语句只能插入单条记录或更新单条记录 |
| select * from ${table} ... | 查询操作  |
| select * from ${table} ... limit ${skip}, ${limit} | 分页查询操作 |
| update ${table} set ... | 更新操作 |

+ 参数类型

| 数据类型        | Java数据类型    |  SQL数据类型  | 说明 |
| :----:   | :----:   | :----: | :----: |
| Java基础数据类型，及Wrapper类 | 基本类型及Wrapper类型 | 数值类型	 |  |
| 日期类型 | java.util.Date;java.sql.Date;java.sql.Timestamp | DATETIME;TIMESTAMP | 不建议使用java.sql.Date类型，该类型只能存储日期 |
| 大数值 | ava.math.BigInteger;java.math.BigDecimal | bigint;bigdecimal | 不建议使用此两种类型，可能会出现丢失精度或其他未知错误 |
| String | ava.lang.String | 字符类型 |  |

+ Dao 返回值

| SQL类型        | 返回值    |  说明  |
| :----:   | :----:   | :----: |
| 单条插入 | long;void | 非指定自增主键插入，返回自增主键;指定自增主键，返回0 |
| 单条插入/更新 | long;void | 非指定自增主键插入，返回自增主键;指定自增主键，返回0;更新操作，返回0 |
| 批量插入 | long;void | 非指定自增主键插入，返回第一条插入记录的自增主键;指定自增主键，返回0 |
| 更新 | int | 返回更新记录数 |
| 删除 | int | 删除记录数 |
| 查询 | Entity;List<Entity>;Page<Entity>;JDBC基本类型List<JDBC基本类型> | 取得的第一条记录;取得的所有记录;分页查询结果;根据需要，例如select count(1) from ...;根据需要，例如select distinct type from ... |

+ 单表DAO
``` java
/**
 * $Id$
 * Copyright(C) 2019 DengBoCong, All Rights Reserved.
 */
package com.voxlearning.utopia.test.storage.jdbc.dao;
 
import java.util.List;
 
import com.voxlearning.utopia.storage.annos.DAO;
import com.voxlearning.utopia.storage.annos.P;
import com.voxlearning.utopia.storage.annos.SQL;
import com.voxlearning.utopia.storage.struct.Page;
import com.voxlearning.utopia.storage.struct.Pageable;
import com.voxlearning.utopia.test.storage.jdbc.bean.NameEnum;
import com.voxlearning.utopia.test.storage.jdbc.bean.TestSingleBean;
 
/**
 * 单表操作DAO
 * @author DengBoCong
 * @version 1.0.0 2019-011-23 11:05:21
 */
@DAO(ds="test_dao")
public interface TestSingleDao {
 
    @SQL("insert into test_single (id, name, create_time, update_time) values (null, #{name}, #{createTime}, #{updateTime})")
    long insert(TestSingleBean entity);
     
    @SQL("insert into test_single (id, name, create_time, update_time) values (null, #{name}, #{createTime}, #{updateTime})")
    long batchInsert(List<TestSingleBean> entities);
     
    @SQL("delete from test_single")
    int deleteAll();
     
    @SQL("select * from test_single where id = #{id}")
    TestSingleBean query(@P("id")long id);
     
    @SQL("select * from test_single")
    List<TestSingleBean> queryAll();
     
    @SQL("select * from test_single where name in (#{names})")
    List<TestSingleBean> queryIn(@P("names")List<String> names);
     
    @SQL("select * from test_single where name like #{name}")
    List<TestSingleBean> queryLike(@P("name")String name);
 
    @SQL("select name from test_single where id = #{id}")
    NameEnum queryName(@P("id")long id);
     
    @SQL("select name from test_single where id in (#{ids})")
    List<NameEnum> queryNames(@P("ids")List<Long> ids);
 
    @SQL("select * from test_single order by id, name limit #{skip}, #{limit}")
    Page<TestSingleBean> pageQuery(Pageable pageable);
}
```
### 调用MySQL Dao
+ 创建DAO实例
``` java
/**
 * $Id$
 * Copyright(C) 2019 DengBoCong, All Rights Reserved.
 */
package com.voxlearning.utopia.test.storage.jdbc.dao;
 
import com.voxlearning.utopia.storage.jdbc.JdbcDaoFactory;
 
/**
 * 存储层实例接口
 * @author DengBoCong
 * @version 1.0 2019-23-08 11:13:24
 */
public interface IProjectDao {
 
    TestSingleDao singleDao = JdbcDaoFactory.getDao(TestSingleDao.class);
}
```
+ 调用DAO
``` java
/**
 * $Id$
 * Copyright(C) 2019 DengBoCong, All Rights Reserved.
 */
package com.voxlearning.utopia.test.storage.jdbc;
 
import java.util.ArrayList;
import java.util.Arrays;
import java.util.Date;
import java.util.List;
 
import com.voxlearning.utopia.test.storage.jdbc.bean.TestSingleBean;
import com.voxlearning.utopia.test.storage.jdbc.dao.IProjectDao;
 
/**
 * 存储层实例接口
 * @author DengBoCong
 * @version 1.0 2019-23-08 11:13:24
 */
public class TestSingleDaoFacade implements IProjectDao {
 
    public static void main(String[] args) {
         
        int index = 1;
        // 插入一条数据
        long id = singleDao.insert(createEntity(index++));
        System.out.printf("Insert single record to test_single with auto increment id : %d%n", id);
        System.out.println();
 
        // 插入多条数据
        List<TestSingleBean> entities = new ArrayList<>();
        for (int i = 0; i < 3; i++) {
            entities.add(createBean(index++));
        }
        id = singleDao.batchInsert(entities);
        System.out.printf("Batch insert first auto increment id is %d%n", id);
        System.out.println();
    }
 
    private static TestSingleBean createEntity(int index) {
        TestSingleBean entity = new TestSingleBean();
        entity.setName("name_" + index);
        entity.setCreateTime(new Date());
        entity.setUpdateTime(entity.getCreateTime());
        return entity;
    }
}
```


