## 开发环境配置
+ 开发工具使用IDEA，build工具使用gradle，IDEA需要安装lombok插件，JDK使用的是1.8版本
+ 首先安装JDK，JDK从刚才的地址中下载后，直接安装，安装完后，在命令行使用java -version 检验是否成功，没有成功，需要手动设置环境变量，详见：http://c.biancheng.net/view/1290.html
+ 从软件仓库中下载最新的gradle的zip包，解压到自己的目录中，设置环境变量，详见：https://www.cnblogs.com/chen110xi/p/6625392.html
+ 从软件仓库中下载IDEA，社区版或者正式版均可，安装完后，正式版需要破解密钥：http://soft.17zuoye.net/idea/IDEA_16_LICENSE_SERVER.txt，破解方式详见：https://blog.csdn.net/zhangwenwu2/article/details/54948959
+ 打开IDEA, 选择clone下来的java项目所在目录的build.gradle 文件，以项目的方式打开，打开后需要安装lombok插件，在File->setting->Pluins->Browse repositories 中搜索Lombok，找到后安装即可, 由于网络问题可能安装失败，可以在https://plugins.jetbrains.com/idea这里搜索Lombok，找到IDEA对应版本的安装包下载下来本地安装即可，本地安装详见：https://jingyan.baidu.com/article/3d69c5513e5953f0cf02d7b4.html
+ lombok安装成功后，需要设置Annotation Processors，IDEA版本不一样，设置的位置相似，一般在File->setting->Build,Execution,Deployment->Compiler->Annotation Processors, 点开后勾上Enable annotation processing
+ 完成之后在IDEA的右侧Gradle菜单栏中刷新gradle即可

## JVM参数
+ 点击idea顶上的Help菜单，选中下拉菜单的Edit Custom VM Options...菜单即打开idea.vmoptions文件
+ 运行项目前请配置运行参数(-Xmx512m)，避免多应用同时启动出现死机情况
<img src="https://raw.githubusercontent.com/DengBoCong/guide-log/master/res/image2019-3-4 18_52_55.png" width="600" height= "350">


## 插件
+ Lombok Plugin 自动生成对象元数据
+ FindBugs-IDEA 找出潜在bug
+ VisualVM Lancher 评估自己写的代码内存占用

## 配置
+ serialVersionUID生成（ Preferences ->Editor -> inspections: 勾选Serializable class without 'serialVersionUID' ）
+ 编码默认修改为UTF-8
+ 换行符统一修改为Linux和Mac下用的\n

## 命名风格
+ 【强制】代码中的命名均不能以下划线或美元符号开始,也不能以下划线或美元符号结束。

   - 反例: _name / __name / $name / name_ / name$ / name__

+ 【强制】变量值表示多个状态并存的时候不要使用多IF判断，利用位移操作取十进制值即可。

正例: 
``` java
func state(a,b,c int) int{
    int mod = 0b000;
    if (a > 0) mod = mod | 0b001;
    if (b > 0) mod = mod | 0b010;
    if (c > 0)  mod = mod | 0b100;
    return mod;
}
```

反例:
``` java
if (a > 0 && b < 0 && c < 0) return 1;
if (a > 0 && b > 0 && c < 0) return 2;
 ...
```
 

+ 【强制】代码中的命名严禁使用拼音与英文混合的方式,更不允许直接使用中文的方式。

   - 说明:正确的英文拼写和语法可以让阅读者易于理解,避免歧义。注意,即使纯拼音命名方式也要避免采用。

   - 正例: production / tobbit /confluence/ beijing 等国际通用的名称,可视同英文。

   - 反例: DaZhePromotion [ 打折 ] / getPingfenByName() [ 评分 ] / int 某变量 = 3


+ 【强制】类名使用 UpperCamelCase（首字母大写的驼峰式） 风格,但以下情形例外: DO / BO / DTO / VO / AO / PO / UID 等。

   - 正例: MarcoPolo / UserDO / XmlService / TcpUdpDeal / TaPromotion

   - 反例: macroPolo / UserDo / XMLService / TCPUDPDeal / TAPromotion

+ 【强制】方法名、参数名、成员变量、局部变量都统一使用 lowerCamelCase（首字母大写的驼峰式） 风格。

   - 正例: localValue / getHttpMessage() / inputUserId


+ 【强制】常量命名全部大写,单词间用下划线隔开,力求语义表达完整清楚,不要嫌名字长。

   - 正例: MAX _ STOCK _ COUNT

   - 反例: MAX _ COUNT


+ 【强制】抽象类命名使用 Abstract 或 Base 开头 ; 异常类命名使用 Exception 结尾 ; 测试类命名以它要测试的类的名称开始,以 Test 结尾。


+ 【强制】类型与中括号紧挨相连来表示数组。

   - 正例: 定义整形数组 int[] arrayDemo;

   - 反例: 在 main 参数中,使用 String args[]来定义。


+ 【强制】 POJO 类中布尔类型的变量,都不要加 is 前缀 ,否则部分框架解析会引起序列化错误。

   - 反例: 定义为基本数据类型 Boolean isDeleted 的属性,它的方法也是 isDeleted() , RPC框架在反向解析的时候,“误以为”对应的属性名称是 deleted ,导致属性获取不到,进而抛出异常。


+ 【强制】包名统一使用小写,点分隔符之间有且仅有一个自然语义的英语单词。包名统一使用单数形式,但是类名如果有复数含义,类名可以使用复数形式。

   - 正例: 应用工具类包名为 com.voxlearning.utopia.service 、类名为 MessageUtils( 此规则参考 spring的框架结构 )


+ 【强制】杜绝完全不规范的缩写,避免望文不知义。

   - 反例: AbstractClass “缩写”命名成 AbsClass;condition “缩写”命名成 condi ,此类随意缩写严重降低了代码的可阅读性。


+ 【推荐】为了达到代码自解释的目标,任何自定义编程元素在命名时,使用尽量完整的单词组合来表达其意，临时变量函数内交换变量除外。

   - 正例: 在 JDK 中,表达原子更新的类名为:AtomicReferenceFieldUpdater。

   - 反例: 变量 int a 的随意命名方式。


+ 【推荐】如果模块、接口、类、方法使用了设计模式,在命名时需体现出具体模式。

   - 说明: 将设计模式体现在名字中,有利于阅读者快速理解架构设计理念。

   - 正例: public class OrderFactory;
         public class LoginProxy;
         public class ResourceObserver;


+ 【推荐】接口类中的方法和属性不要加任何修饰符号 (public 也不要加 ) ,保持代码的简洁性,并加上有效的 Javadoc 注释。尽量不要在接口里定义变量,如果一定要定义变量,肯定是与接口方法相关,并且是整个应用的基础常量。

   - 正例: 接口方法签名 void commit();
         接口基础常量 String COMPANY = " alibaba " ;

   - 反例: 接口方法定义 public abstract void f();

   - 说明: JDK 8 中接口允许有默认实现,那么这个 default 方法,是对所有实现类都有价值的默认实现。


+ 【推荐】接口和实现类的命名有两套规则:
   - 【强制】对于 Service 和 DAO 类,基于 SOA 的理念,暴露出来的服务一定是接口,内部的实现类用 Impl 的后缀与接口区别。

         - 正例: CacheServiceImpl 实现 CacheService 接口。
   - 【推荐】 如果是形容能力的接口名称,取对应的形容词为接口名 ( 通常是– able 的形式 ) 。

         - 正例: AbstractTranslator 实现 Translatable 接口 。


+ 【参考】枚举类名建议带上 Enum 后缀,枚举成员名称需要全大写,单词间用下划线隔开。

   - 说明: 枚举其实就是特殊的类,域成员均为常量,且构造方法被默认强制是私有。

   - 正例: 枚举名字为 ProcessStatusEnum 的 成员名称: SUCCESS / UNKNOWN _ REASON 。

   - 说明: 建议实现枚举of(String)方法,方便业务层转换。

   - 正例: 

``` java
public static ChipsQuestionType of(String str) {
    try {
        return ChipsQuestionType.valueOf(str);
 } catch (Exception e) {
        return UNKNOWN;
 }
}
```
 

+ 【参考】各层命名规约:

   - Service / DAO 层方法命名规约

         - 获取单个对象的方法用 get 做前缀。

         - 获取多个对象的方法用 list 做前缀,复数形式结尾如:listObjects。

         - 获取统计值的方法用 count 做前缀。

         - 插入的方法用 save/insert 做前缀。

         - 删除的方法用 remove/delete 做前缀。

         - 修改的方法用 update 做前缀。

   - 领域模型命名规约

         - 数据对象: xxxDO , xxx 即为数据表名。

         - 数据传输对象: xxxDTO , xxx 为业务领域相关的名称。

         - 展示对象: xxxVO , xxx 一般为网页名称。

         - POJO 是 DO / DTO / BO / VO 的统称,禁止命名成 xxxPOJO 。


+ 【强制】在 long 或者 Long 赋值时,数值后使用大写的 L ,不能是小写的 l ,小写容易跟数字1 混淆,造成误解。

   - 说明: Long a = 2 l; 写的是数字的 21,还是 Long 型的 2?


## 代码格式

+ 【强制】大括号的使用约定。如果是大括号内为空,则简洁地写成{}即可,不需要换行 ; 如果是非空代码块则:

   - 左大括号前不换行。

   - 左大括号后换行。

   - 右大括号前换行。

   - 右大括号后还有 else 等代码则不换行 ; 表示终止的右大括号后必须换行。


+ 【强制】左小括号和字符之间不出现空格 ; 同样,右小括号和字符之间也不出现空格;而左大括号前需要空格。

   - 反例: if(空格 a == b 空格)


+ 【强制】 if / for / while / switch / do 等保留字与括号之间都必须加空格。


+ 【强制】任何二目、三目运算符的左右两边都需要加一个空格。

   - 说明: 运算符包括赋值运算符=、逻辑运算符&&、加减乘除符号等。


+ 【强制】注释的双斜线与注释内容之间有且仅有一个空格。

   - 正例: 

``` java
// 我是注释, 注意前面有空格
if (a > 0) mod = mod | 0b001;
```

+ 【强制】方法参数在定义和传入时,多个参数逗号后边必须加空格。

   - 正例: 下例中实参的 a,b,c后边必须要有一个空格。
``` java
int state(int a, int b, int c)
```

 

+ 【推荐】单个方法的总行数不超过 80 行。

   - 说明:包括方法签名、结束右大括号、方法内代码、注释、空行、回车及任何不可见字符的总行数不超过 80 行。

   - 正例:代码逻辑分清红花和绿叶,个性和共性,绿叶逻辑单独出来成为额外方法,使主干代码更加清晰;共性逻辑抽取成为共性方法,便于复用和维护。


+ 【推荐】不同逻辑、不同语义、不同业务的代码之间插入一个空行分隔开来以提升可读性。

   - 说明:任何情形,没有必要插入多个空行进行隔开。


+ 【强制】避免通过一个类的对象引用访问此类的静态变量或静态方法,无谓增加编译器解析成本,直接用类名来访问即可。


+ 【强制】所有的覆写方法,必须加@Override 注解。

   - 说明: getObject() 与 get 0 bject() 的问题。一个是字母的 O ,一个是数字的 0,加@ Override可以准确判断是否覆盖成功。另外,如果在抽象类中对方法签名进行修改,其实现类会马上编译报错。


+ 【强制】外部正在调用或者二方库依赖的接口,不允许修改方法签名,避免对接口调用方产生影响。接口过时必须加@ Deprecated 注解,并清晰地说明采用的新接口或者新服务是什么。


+ 【强制】 Object 的 equals 方法容易抛空指针异常,应使用常量或确定有值的对象来调用equals 。

   - 正例: " hello " .equals(object);

   - 反例:  object.equals( " hello " );

   - 说明: 推荐使用 java . util . Objects # equals(JDK 7 之后版本)


【强制】所有的相同类型的包装类对象之间值的比较,全部使用 equals 方法比较。

   - 说明: 对于 Integer var = ?在-128 至 127 范围内的赋值,  Integer 对象是在IntegerCache . cache 产生, 会复用已有对象, 这个区间内的 Integer 值可以直接使用==进行判断,但是这个区间之外的所有数据,都会在堆上产生,并不会复用已有对象,这是一个大坑, 推荐使用 equals 方法进行判断。


+ 【强制】序列化类新增属性时,请不要修改 serialVersionUID 字段,避免反序列失败 ; 如果完全不兼容升级,避免反序列化混乱,那么请修改 serialVersionUID 值。

   - 说明:注意 serialVersionUID 不一致会抛出序列化运行时异常



+ 【强制】禁止在 POJO 类中,同时存在对应属性 xxx 的 isXxx() 和 getXxx() 方法。

   - 说明:框架在调用属性 xxx 的提取方法时,并不能确定哪个方法一定是被优先调用到。



+ 【推荐】循环体内,字符串的连接方式,使用 StringBuilder 的 append 方法进行扩展。


   - 说明:下例中,反编译出的字节码文件显示每次循环都会 new 出一个 StringBuilder 对象,然后进行 append 操作,最后通过 toString 方法返回 String 对象,造成内存资源浪费。

   - 反例:

``` java
String str = "offset";
for (int i = 0; i < 100; i++) {
    str = str + "idx";
}
```


+ 【推荐】慎用 Object 的 clone 方法来拷贝对象。


   - 说明: 对象的 clone 方法默认是浅拷贝,若想实现深拷贝需要重写 clone 方法实现域对象的深度遍历式拷贝。


+ 【推荐】类成员与方法访问控制从严:

   - 如果不允许外部直接通过 new 来创建对象,那么构造方法必须是 private 。

   - 工具类不允许有 public 或 default 构造方法。

   - 类非 static 成员变量并且与子类共享,必须是 protected 。

   - 类非 static 成员变量并且仅在本类使用,必须是 private 。

   - 类 static 成员变量如果仅在本类使用,必须是 private 。

   - 若是 static 成员变量,考虑是否为 final 。

   - 类成员方法只供类内部调用,必须是 private 。

   - 类成员方法只对继承类公开,那么限制为 protected


+ 【强制】关于 hashCode 和 equals 的处理,遵循如下规则:

   - 只要重写 equals ,就必须重写 hashCode 。

   - 因为 Set 存储的是不重复的对象,依据 hashCode 和 equals 进行判断,所以 Set 存储的对象必须重写这两个方法。

   - 如果自定义对象作为 Map 的键,那么必须重写 hashCode 和 equals 。

   - 说明: String 重写了 hashCode 和 equals 方法,所以我们可以非常安全的使用 String 对象作为 key 来使用。


+ 【强制】 ArrayList 的 subList 结果不可强转成 ArrayList ,否则会抛出 ClassCastException异常,即 java . util . RandomAccessSubList cannot be cast to java.util.ArrayList 。

   - 说明: subList 返回的是 ArrayList 的内部类 SubList ,并不是 ArrayList 而是 ArrayList的一个视图,对于 SubList 子列表的所有操作最终会反映到原列表上。
   - 说明: 在 subList 场景中,高度注意对原集合元素的增加或删除,均会导致子列表的遍历、增加、删除产生 ConcurrentModificationException 异常。


+ 【强制】使用工具类 Arrays.asList() 把数组转换成集合时,不能使用其修改集合相关的方法,它的 add / remove / clear 方法会抛出 UnsupportedOperationException 异常。


   - 说明: asList 的返回对象是一个 Arrays 内部类,并没有实现集合的修改方法。 Arrays.asList体现的是适配器模式,只是转换接口,后台的数据仍是数组。

``` java
String[] str = new String[] { "rock", "n" };
List list = Arrays.asList(str);
//第一种情况:

list.add("roll"); 运行时异常。
//第二种情况:

str[0] = "GNR"; 那么 list.get(0) 也会随之修改。
```


+ 【强制】泛型通配符<? extends T >来接收返回的数据,此写法的泛型集合不能使用 add 方法,而 <? super T> 不能使用 get 方法,作为接口调用赋值时易出错。

   - 说明: 道理同上, 扩展说一下 PECS(Producer Extends Consumer Super) 原则:第一、频繁往外读取内容的,适合用<? extends T >。第二、经常往里插入的,适合用 <? super T> 。


+ 【强制】在使用正则表达式时,利用好其预编译功能,可以有效加快正则匹配速度。


   - 说明:不要在方法体内定义: Pattern pattern = Pattern . compile(“ 规则 ”);



+ 【参考】下列情形,需要进行参数校验:


   - 调用频次低的方法。

   - 执行时间开销很大的方法。此情形中,参数校验时间几乎可以忽略不计,但如果因为参数错误导致中间执行回退,或者错误,那得不偿失。

   - 需要极高稳定性和可用性的方法。

   - 对外提供的开放接口,不管是 RPC / API / HTTP 接口。

   - 敏感权限入口。



+ 【参考】下列情形,不需要进行参数校验:

   - 极有可能被循环调用的方法。但在方法说明里必须注明外部参数检查要求。

   - 底层调用频度比较高的方法。毕竟是像纯净水过滤的最后一道,参数错误不太可能到底层才会暴露问题。一般 DAO 层与 Service 层都在同一个应用中,部署在同一台服务器中,所以 DAO 的参数校验,可以省略。

   - 被声明成 private 只会被自己代码所调用的方法,如果能够确定调用方法的代码传入参数已经做过检查或者肯定不会有问题,此时可以不校验参数。


+ 【参考】特殊注释标记,请注明标记人与标记时间。注意及时处理这些标记,通过标记扫描,经常清理此类标记。线上故障有时候就是来源于这些标记处的代码。

   - 待办事宜 (TODO) : ( 标记人,标记时间, [ 预计处理时间 ])表示需要实现,但目前还未实现的功能。这实际上是一个 Javadoc 的标签,目前的 Javadoc还没有实现,但已经被广泛使用。只能应用于类,接口和方法 ( 因为它是一个 Javadoc 标签 ) 。

   - 错误,不能工作 (FIXME) : ( 标记人,标记时间, [ 预计处理时间 ])在注释中用 FIXME 标记某代码是错误的,而且不能工作,需要及时纠正的情况。


+ 【参考】 catch 时请分清稳定代码和非稳定代码,稳定代码指的是无论如何不会出错的代码。对于非稳定代码的 catch 尽可能进行区分异常类型,再做对应的异常处理。

   - 说明:对大段代码进行 try - catch ,使程序无法根据不同的异常做出正确的应激反应,也不利\于定位问题,这是一种不负责任的表现。

   - 正例:用户注册的场景中,如果用户输入非法字符,或用户名称已存在,或用户输入密码过于简单,在程序上作出分门别类的判断,并提示给用户。

   ## 并发相关

+ 【强制】获取单例对象需要保证线程安全,其中的方法也要保证线程安全。

   - 说明:资源驱动类、工具类、单例工厂类都需要注意。


+ 【强制】在高并发场景中,避免使用”等于”判断作为中断或退出的条件。


   - 说明: 如果并发控制没有处理好,容易产生等值判断被“击穿”的情况,使用大于或小于的区间判断条件来代替。

   - 反例: 判断剩余奖品数量等于 0 时,终止发放奖品,但因为并发处理错误导致奖品数量瞬间变成了负数,这样的话,活动无法终止



+ 【强制】线程池不允许使用 Executors 去创建,而是通过 ThreadPoolExecutor 的方式,这样的处理方式让写的同学更加明确线程池的运行规则,规避资源耗尽的风险。

   - 说明: Executors 返回的线程池对象的弊端如下:

      - FixedThreadPool 和 SingleThreadPool: 允许的请求队列长度为 Integer.MAX_VALUE ,可能会堆积大量的请求,从而导致 OOM 。
      - CachedThreadPool 和 ScheduledThreadPool: 允许的创建线程数量为 Integer.MAX_VALUE ,可能会创建大量的线程,从而导致 OOM 。


+ 【强制】 SimpleDateFormat 是线程不安全的类,一般不要定义为 static 变量,如果定义为static ,必须加锁,或者使用 DateUtils 工具类。

   - 说明:如果是 JDK 8 的应用,可以使用 Instant 代替 Date , LocalDateTime 代替 Calendar ,DateTimeFormatter 代替 SimpleDateFormat ,官方给出的解释: simple beautiful strong immutable thread - safe 。

 

+ 【强制】高并发时,同步调用应该去考量锁的性能损耗。能用无锁数据结构,就不要用锁 ; 能锁区块,就不要锁整个方法体 ; 能用对象锁,就不要用类锁。

   - 说明:尽可能使加锁的代码块工作量尽可能的小,避免在锁代码块中调用 RPC 方法。


+ 【强制】对多个资源、数据库表、对象同时加锁时,需要保持一致的加锁顺序,否则可能会造成死锁。

   - 说明:线程一需要对表 A 、 B 、 C 依次全部加锁后才可以进行更新操作,那么线程二的加锁顺序也必须是 A 、 B 、 C ,否则可能出现死锁。


 + 【强制】并发修改同一记录时,避免更新丢失,需要加锁。要么在应用层加锁,要么在缓存加锁,要么在数据库层使用乐观锁,使用 version 作为更新依据。

   - 说明: 如果每次访问冲突概率小于 20%,推荐使用乐观锁,否则使用悲观锁。乐观锁的重试次数不得小于 3 次。



+ 【强制】多线程并行处理定时任务时, Timer 运行多个 TimeTask 时,只要其中之一没有捕获抛出的异常,其它任务便会自动终止运行,使用 ScheduledExecutorService 则没有这个问题。



+ 【推荐】使用 CountDownLatch 进行异步转同步操作,每个线程退出前必须调用 countDown方法,线程执行代码注意 catch 异常,确保 countDown 方法被执行到,避免主线程无法执行至 await 方法,直到超时才返回结果。

   - 说明:注意,子线程抛出异常堆栈,不能在主线程 try - catch 到。


+ 【推荐】避免 Random 实例被多线程使用,虽然共享该实例是线程安全的,但会因竞争同一seed 导致的性能下降。

   - 说明: Random 实例包括 java.util.Random 的实例或者 Math.random() 的方式。

   - 正例: 在 JDK 7 之后,可以直接使用ThreadLocalRandom ,而在 JDK 7 之前,需要编码保证每个线程持有一个实例。


+ 【参考】 volatile 解决多线程内存不可见问题。对于一写多读,是可以解决变量同步问题, 但是如果多写,同样无法解决线程安全问题。比如 count ++操作。

   - 说明: AtomicInteger能保证线程安全问题，如果是JDK8以后的推荐用LongAdder性能更好( 减少乐观锁的重试次数 )，实现方式有点像ConcurrentHashMap一样，采用空间换时间的方式，提高在线程竞争激烈的情况下，提供计数的效率。

   - 解析: LongAdder内里面存在一个Cell数组，计数是将线程id进行hash算法后，得到一个小于等于当前计算机核数的值，根据该值定位到一个cell数组，然后操作该cell进行单独计数。最终求总计数值时候，把所有的cell数组值相加即可。这样将多个线程操作一个AtomicLong转变为多个线程操作多个cell的情况，自然竞争小了，效率高了。可是存储空间大了。


+ 【参考】 HashMap 在容量不够进行 resize 时由于高并发可能出现死链,导致 CPU 飙升,在开发过程中可以使用其它数据结构或加锁来规避此风险。


+ 【参考】 ThreadLocal 无法解决共享对象的更新问题, ThreadLocal 对象建议使用 static修饰。这个变量是针对一个线程内所有操作共享的,所以设置为静态变量, 所有此类实例共享此静态变量 ,也就是说在类第一次被使用时装载,只分配一块存储空间,所有此类的对象 ( 只要是这个线程内定义的 ) 都可以操控这个变量。

   - 说明: hreadLocal 适用于如下两种场景

   - 每个线程需要有自己单独的实例
   - 实例需要在多个方法中共享，但不希望被多线程共享

   - 对于第一点，每个线程拥有自己实例，实现它的方式很多。例如可以在线程内部构建一个单独的实例。ThreadLocal 可以以非常方便的形式满足该需求。

   - 对于第二点，可以在满足第一点（每个线程有自己的实例）的条件下，通过方法间引用传递的形式实现。ThreadLocal 使得代码耦合度更低，且实现更优雅。

   ## Mysql相关

+ 【强制】不要使用 count( 列名 ) 或 count( 常量 ) 来替代 count( * ) , count( * ) 是 SQL 92 定义的标准统计行数的语法,跟数据库无关,跟 NULL 和非 NULL 无关。

   - 说明: count( * ) 会统计值为 NULL 的行,而 count( 列名 ) 不会统计此列为 NULL 值的行


+ 【强制】 count(distinct col) 计算该列除 NULL 之外的不重复行数,注意 count(distinct col 1, col 2 ) 如果其中一列全为 NULL ,那么即使另一列有不同的值,也返回为 0。


+ 【强制】当某一列的值全是 NULL 时, count(col) 的返回结果为 0,但 sum(col) 的返回结果为NULL ,因此使用 sum() 时需注意 NPE 问题。

   - 正例:可以使用如下方式来避免 sum 的 NPE 问题: SELECT IF(ISNULL(SUM(g)) ,0, SUM(g)) FROM table;

 

+ 【强制】SELECT语句必须指定具体字段名称，禁止写成"*"。
   - 读取不需要的列会增加CPU、IO、NET消耗

   - 不能有效的利用覆盖索引

   - 使用SELECT *容易在增加或者删除字段后出现程序BUG

 

+ 【强制】insert语句指定具体字段名称，不要写成insert into t1 values(…)，道理同上。

+ 【强制】禁止在WHERE条件的属性上使用函数或者表达式。

   - 反例：SELECT uid FROM t_user WHERE from_unixtime(day)>='2017-02-15' 会导致全表扫描

   - 正例：SELECT uid FROM t_user WHERE day>= unix_timestamp('2017-02-15 00:00:00')


+ 【强制】禁止负向查询，以及%开头的模糊查询。


   - 负向查询条件：NOT、!=、<>、!<、!>、NOT IN、NOT LIKE等，会导致全表扫描

   - %开头的模糊查询，会导致全表扫描

 

+ 【强制】禁止大表使用JOIN查询，禁止大表使用子查询会产生临时表，消耗较多内存与CPU，极大影响数据库性能。


+ 【强制】WHERE子句中禁止只使用全模糊的LIKE条件进行查找，必须有其他等值或范围查询条件，否则无法利用索引。

+ 【强制】禁止联表更新语句

   - 反例: update t1,t2 where t1.id=t2.id…

+ 【强制】不得使用外键与级联,一切外键概念必须在应用层解决。

   - 说明:以学生和成绩的关系为例,学生表中的 student _ id 是主键,那么成绩表中的 student _ id则为外键。如果更新学生表中的 student _ id ,同时触发成绩表中的 student _ id 更新,即为级联更新。外键与级联更新适用于单机低并发,不适合分布式、高并发集群 ; 级联更新是强阻塞,存在数据库更新风暴的风险 ; 外键影响数据库的插入速度。

+ 【强制】禁止使用存储过程,存储过程难以调试和扩展,更没有移植性。

+ 【强制】数据订正(特别是删除、修改记录操作)时,要先 select ,避免出现误删除,确认无误才能执行更新语句。


+ 【推荐】 IN 操作能避免则避免,若实在避免不了,需要仔细评估 in 后边的集合元素数量,控制在 1000 个之内。


+ 【推荐】建组合索引的时候,区分度最高的在最左边。

   - 正例:如果 where a =? and b =? ,如果 a 列的几乎接近于唯一值,那么只需要单建 idx _ a 索引即可。

   - 说明:存在非等号和等号混合时,在建索引时,请把等号条件的列前置。如: where c >? and d =? 那么即使 c 的区分度更高,也必须把 d 放在索引的最前列,即索引 idx_d_c。


+ 【推荐】防止因字段类型不同造成的隐式转换,导致索引失效。