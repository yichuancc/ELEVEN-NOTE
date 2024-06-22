# asList装箱问题

Arrays.asList方法不会将基础类型的数组自动装箱并生成新的列表
如果想要将基础类型的数组自动装箱，可以考虑使用Guava包中提供的asList方法，如果只有一个元素，可以明确使用Colletions.singletonList来表达。

```java
//反例
List<Intger> intArrayToInteger(int[] intArr) {
    List<Integer> integerList = Arrays.asList(intArr);
    return integerList;
}
// 编译错误

//正例
List<Intger> intArrayToInteger(int[] intArr) {
    List<Integer> integerList = Arrays.stream(intArr).boxed().collect(Collectors.toList());
    return integerList;
}

List<Intger> intArrayToInteger(int[] intArr) {
    List<Integer> integerList = Ints.asList(intArr);
    return integerList;
}


List<Intger> intArrayToInteger(int[] intArr) {
    List<int[]> integerList = Collections.singletonList(intArr);
    return integerList;
}
```

# boolean可读的坑

类似命名为"getX()"并且返回值是布尔类型的方法，建议改名为"isX()"

```java
// 是否快速记账,之前的命名,别扭
public static boolean getIsQuickAddTrans() {}
// 修改之后
public static boolean isQuickAddTrans() {}
```

# Boolean返回null

Boolean接口返回null值，可能会导致调用者代码出现问题。

```java
反例
public Boolean isNormalFee(){
    //...
    return null;
}

正例
public Boolean isNormalFee() {
    //...
    return false;
}
```

# lock紧跟try-finally

Lock方法调用后应该紧跟try-fianlly块，防止因代码异常导致锁无法释放。

```java
//反例
Lock lock;
void fun{
    lock.lock();
    otherHandle();
    try{
        handle();
    }finally{
        lock.unlock();
    }
}

//正例
Lock lock;
void fun{
    lock.lock();
    try{
        otherHandle();
        handle();
    }finally{
        lock.unlock();
    }
}
```

# map迭代entrySet()

当循环中只需要获取Map 的主键key时，迭代keySet() 是正确的；但是，当需要主键key 和取值value 时，迭代entrySet() 才是更高效的做法，其比先迭代keySet() 后再去通过get 取值性能更佳。

```java
//反例
//Map 获取value 反例:

HashMap<String, String> map = new HashMap<>();

for (String key : map.keySet()){
    String value = map.get(key);
}

//正例
//Map 获取key & value 正例:

HashMap<String, String> map = new HashMap<>();

for (Map.Entry<String,String> entry : map.entrySet()){
	String key = entry.getKey();
	String value = entry.getValue();
}
```

# mybatis多查询

当遇到多个查询条件，使用where 1=1 可以很方便的解决我们的问题，但是这样很可能会造成非常大的性能损失，因为添加了 “where 1=1 ”的过滤条件之后，数据库系统就无法使用索引等查询优化策略，数据库系统将会被迫对每行数据进行扫描（即全表扫描） 以比较此行是否满足过滤条件，当表中的数据量较大时查询速度会非常慢；

```java
//反例
<select id="queryBookInfo" parameterType="com.tjt.platform.entity.BookInfo" resultType="java.lang.Integer">
select count(*) from t_rule_BookInfo t where 1=1
<if test="title !=null and title !='' ">
		AND title = #{title}
</if>
<if test="author !=null and author !='' ">
		AND author = #{author}
</if>
</select>

//正例
<select id="queryBookInfo" parameterType="com.tjt.platform.entity.BookInfo" resultType="java.lang.Integer">
	select count(*) from t_rule_BookInfo t
<where>
<if test="title !=null and title !='' ">
	title = #{title}
</if>
<if test="author !=null and author !='' ">
		AND author = #{author}
</if>
</where>
</select>
```

# object重写

Object.hashcode的约定中要求时，如果两个对象是相等的，那么二者分别调用hashcode方法也应当返回同样的结果，如果重写equals方法不重写hashcode方法会导致如hashset hashmap等以来hashcode方法的集合无法工作。

# optional分支优化

```java
String myString = null;

// Noncompliant  引发空指针异常
System.out.println("Equal? " + myString.equals("foo"));

// Noncompliant  这样写有点冗余
System.out.println("Equal? " + (myString != null && myString.equals("foo")));


// Compliant Solution
System.out.println("Equal?" + "foo".equals(myString));  // 规范写法
```

# optional比较值

Optional 类应当使用 equals 方法比较其包含的值，而非使用 == 或 != 运算符。

```java
//反例
boolean isEquals(Optional<Integer> x,Optional<Integer> y) {
     return x == y;
}

//正例
boolean isEquals(Optional<Integer> x,Optional<Integer> y) {
     return x.equals(y);
}
```

# Option不存在异常

Optional 实例被认为不存在，如果调用get() 和 orElseThrow() 方法始终会抛异常。

```java
//反例
void fun(Optional<String> opt){
    if(opt.isPresent()){
        // do logic
    }else{
        String val = opt.get();
    }
}


//正例
void fun(Optional<String> opt){
    if(opt.isPresent()){
        // do logic
    }else{
        String val = opt.orElse("alter");
    }
}
```

# return精简不必要代码

```java
//反例
// 是否通过函数
public boolean isPassed(Double passRate) {
    if (Objects.nonNull(passRate) && passRate.compareTo(PASS_THRESHOLD) >= 0) {
        return true;
    }
    return false;
}

//正例
// 是否通过函数
public boolean isPassed(Double passRate) {
    return Objects.nonNull(passRate) && passRate.compareTo(PASS_THRESHOLD) >= 0;
}
```

# stringbuilder拼接

一般的字符串拼接在编译期Java 会对其进行优化，但是在循环中字符串的拼接Java 编译期无法执行优化，所以需要使用StringBuilder 进行替换。

```java
//反例：
//在循环中拼接字符串反例
String str = "";
for (int i = 0; i < 10; i++){
    //在循环中字符串拼接Java 不会对其进行优化
    str += i;
}

//正例：
//在循环中拼接字符串正例
String str1 = "Love";
String str2 = "Courage";
String strConcat = str1 + str2; //Java 编译器会对该普通模式的字符串拼接进行优化
StringBuilder sb = new StringBuilder();
for (int i = 0; i < 10; i++){
    //在循环中，Java 编译器无法进行优化，所以要手动使用StringBuilder
    sb.append(i);
}
```

# Threadlocal静态

ThreadLocal 变量应该声明 static 在常量池中。如果被声明非静态变量，会导致每个线程中维护Threadlocal中变量对所有线程的映射表，资源浪费，切降低性能。

```java
//反例
private final ThreadLocal<Integer> cnt = new ThreadLocal<>();

//正例
private static final ThreadLocal<Integer> cnt = new ThreadLocal<>();
```

# 内部类static修饰

如果未引用外部类信息，内部类应该static修饰提升性能。

```java
//反例
class A {
    private String pa;

    //未引用外部类信息 应当修饰static
    class interA{
        private String pb;
    }
}

//正例
class A {
    private String pa;

    static class interA{
        private String pb;
    }

    class interB{
        void m(){
            if("javayige".equals(pa))
        }
    }
}
```

# 函数替换匿名内部类

```java
//反例
// 发送结算数据
sendWorkerSettleData(WorkerPushDataType.CHECKER, () -> {
    Date beginDate = DateUtils.addDays(currDate, -aheadDays);
    Date endDate = DateUtils.addDays(currDate, 1);
    return auditTaskDAO.statCheckerSettleData(beginDate, endDate);
});

//正例
// 发送结算数据
sendWorkerSettleData(WorkerPushDataType.CHECKER, () -> statCheckerSettleData(currDate, aheadDays));

// 统计验收员结算数据函数
private List<WorkerSettleData> statCheckerSettleData(Date currDate, int aheadDays) {
    Date beginDate = DateUtils.addDays(currDate, -aheadDays);
    Date endDate = DateUtils.addDays(currDate, 1);
    return auditTaskDAO.statCheckerSettleData(beginDate, endDate);
}
```

# 删除不必要的变量

```java
//反例
// 查询用户函数
public List<UserDO> queryUser(Long id, String name) {
    UserQuery userQuery = new UserQuery();
    userQuery.setId(id);
    userQuery.setName(name);
    List<UserDO> userList = userDAO.query(userQuery);
    return userList;
}

//正例
// 查询用户函数
public List<UserDO> queryUser(Long id, String name) {
    UserQuery userQuery = new UserQuery();
    userQuery.setId(id);
    userQuery.setName(name);
    return userDAO.query(userQuery);
}
```

# 判断预防空指针

if else分支判断的很多情况都是进行非空条件的判断，Optional是Java8开始提供的新特性，使用这个语法特性，也可以减少代码中if else的数量，例如：

```java
//反例
String str = "Hello World!";

if (str != null) {
    System.out.println(str);
} else {
    System.out.println("Null");
}

//正例
Optional<String> optional = Optional.of("Hello World!");
optional.ifPresentOrElse(System.out::println, () -> System.out.println("Null"));
```

# 加锁缓存实例

由于基础类型包装类的实例可能存在缓存，不同对象引用同一个实例。当对基础类型包装类的对象上锁时，会导致上锁的代码段与其他代码段共享同一个锁。

```java
//反例
private final Integer lock;

void foo(){
	synchronized(lock) {
        //...
    }
}

//正例
private final Object lock;

void foo(){
    synchronized(lock) {
        //...
    }
}
```

# 单例多线程

在实现单例模式，如果未考虑多线程情况喜爱，这样写会导致多性格线程同时调用有风险。

```java
//反例
public class Singleton{
    private static Singleton uniqueProcess;
    publci Singleton getInstance(){
        if(null == uniqueProcess) {
            synchronized(Singleton.class) {
                uniqueProcess = new Singleton();
            }
        }
        return uniqueProcess;
    }
}

//正例
public class Singleton{
    private volatile static Singleton uniqueProcess;
    publci Singleton getInstance(){
        if(null == uniqueProcess) {
            synchronized(Singleton.class) {
                uniqueProcess = new Singleton();
            }
        }
        return uniqueProcess;
    }
}
```

# 参数准入校验

【代码缺陷】
编译时检测使用字面量作为部分类的方法的参数进行调用世，必定会导致运行时抛出异常的方法调用，这些方法包括：
● java.time.Duration::parse
● java.time.Instant::parse
● ImmutableMap::of
● UUID::fromString
● ......等等

```java
//反例
LocalDateTime time = LocalDateTime.parse("2023-10-21T22:45:00+08:00");

//正例
LocalDateTime time = LocalDateTIme.parse("2023-10-21T22:45:00")

void fun(String timeStr) {
  	LocalDateTime time = LocalDateTIme.parse(timeStr); // 不检测变量准入
}
```

# 基础类型构造

基础类型的构造器方法jdk9已经被标记为deprecated，应使用包装类的提供的valueOf方法替换构造方法。

```java
//反例
void fun(){
    Long long = new Long("123");
}
//正例
void fun() {
    Long limit = Long.valueOf("123");
}
```

# 字符串转基本类型

```java
//使用Integer.parseInt(String) 替代Integer.valueOf(String)。
public void foo(Bean bean) {
    // 旧写法，这里的bean.getId() 是String类型
    final int id = Integer.valueOf(bean.getId());

    // 推荐写法， 类似还有Long.parseLong(String)
    final int id = Integer.parseInt(bean.getId());
}
```

# 小心数组判断

问题
我们在比较熟组的相等性时，通常我们希望比较的是熟组内容的相等性，而非他们是否为同一对象。
但许多常用的比较方法比较的是熟组引用而非熟组内容相等，包括 ：

* 对象的 equals()方法，

* Guava的com.google.common.base.Objects#equal()方法，

* JDK的java.uti.Objects#equals()方法。

如果确实需要比较熟组引用是否相等，明确使用==避免歧义，否则应用使用java.util.Arrays#equals()来比较数组的内容是否相等。另外还有个输出的问题，数组的toString()方法会输出她的引用信息比如xx@13aabb等，这种没有任何意义，考虑使用Arrays.toString方法来输出具有可读性信息。

```java
//反例
booleanarrayEqualsTest(Object[] array1,Object[] array2) {
    return array1.equals(array2); //实际比较的是引用是否相等
}
Integer[] intArray = newInteger[]{1,2,3};
System.out.println(intArray.toString()) //将会输出类似xx@13aabb信息

//正例
booleanarrayEqualsReference(Object[] array1,Object[] array2) {
    returnarray1== array2; //比较的是引用是否相等
}
Integer[] intArray = newInteger[]{1,2,3};
System.out.println(Arrays.toString(intArray)) //将会输出[1,2,3]
```

# 小时设置的坑

Calendar.HOUR 表示的是12小时制度，而Calendar.HOUR_OF_DAY才表示24小时制度，一般而言，在业务代码中使用12小时制的情况极少。

```java
//反例
public void cal(Calendar now) {
	now.set(Calendar.HOUR,12);
}

//正例
public void cal(Calendar now) {
	now.set(Calendar.HOUR_OF_DAY,12);
}
```

# 异常日志输出

异常信息应该包括两类信息：案发现场信息和异常堆栈信息。如果不处理，那么通过关键字throws往上抛出。

* 正例：logger.error(各类参数或者对象toString() + "_" + e.getMessage(), e);
* 不要使用 e.printStackTrace()，e.printStackTrace()是通过System.err把日志输出到控制台，不会输出到日志中，不利于后期管理查看。
*  e.getMessage()记录错误基本描述信息，e记录详细的堆栈异常信息。

# 成员变量权限

如果声明一个字段为'public static' 却不加上'final' , 任何类都能去修改这个共享的字段，甚至赋值为null。 建议： 添加final或改为private并提供get方法。

如我们项目中的BaseApplication的Context的对象，我们可以改为私有，提供get方法，而不是直接使用ApplicationContext.context来获取。

```java
// 反例
// 例如这种，应该添加final
public static String EXTRA_KEY_TIME_ID = "extra_time_id";
public static String EXTRA_KEY_CUSTOM_START_TIME = "extra_start_time";
// 正例
// 修改后
public static final String EXTRA_KEY_TIME_ID = "extra_time_id";
public static final String EXTRA_KEY_CUSTOM_START_TIME = "extra_start_time";
```

# 接口兼容性

代码评审的时候,要重点关注是否考虑到了接口的兼容性.因为很多bug都是因为修改了对外旧接口，但是却不做兼容导致的。

```java
//老接口
void oldService(A,B){
  //兼容新接口，内部兼容逻辑 传个null代替C
  newService(A,B,null);
}

//新接口，暂时不能删掉老接口，需要做兼容。
void newService(A,B,C){
  ...
}
```

# 数组对象输出

数组的toString方法会舒畅他的引用信息，如I@24aabb 这不代表任何实际意义，考虑使用Arrays.toString方法输出具有可读性的信息。

```java
//反例
Integer[] iArr = new Integer[]{1,2,3};
System.out.println(iArr.toString());

//正例
Integer[] iArr = new Integer[]{1,2,3};
System.out.println(Arrays.toString(iArr);
```

# 数组引用填充

Arrays.fill(Object[] , object) 是用于对象的引用填充到数组的每一个元素中。

String[] temp = new String(32);

Arrays.fill(foo,"java");
//temp数组中的元素都指向"java"字符串在常量池中的引用，但是，数组只允许子类型的数组赋值，如果违背在编译时不会报错，运行时将会抛出ArrayStoreException。

```java
//反例
String[] temp1 = new String[12];
Arrays.fill(temp1,1);

//正例
String[] temp2 = new String[12];
Arrays.fill(temp2,"yige");
```

# 时间操作的坑

在进行时间的计算时候，如果需要计算下一年，严禁使用数字直接操作，必须使用api直接操作，比方说下一年，新增365天就没有考虑到闰年的情况。

```java
//反例
public Calendar foo(Calendar date) {
	date.add(Calendar.DTE,365);
	return date;
}

//正例
public Calendar foo(Calendar date) {
	date.add(Calendar.YEAR,1);
    return date;
}
```

# 枚举属性私有不可变

枚举通常被当做常量使用，如果枚举中存在公共属性字段或设置字段方法，那么这些枚举常量的属性很容易被修改；理想情况下，枚举中的属性字段是私有的，并在私有构造函数中赋值，没有对应的Setter 方法，最好加上final 修饰符。

```java
//反例：
public enum SwitchStatus {
    // 枚举的属性字段反例
    DISABLED(0, "禁用"),
    ENABLED(1, "启用");

    public int value;
    private String description;

    private SwitchStatus(int value, String description) {
    	this.value = value;
    	this.description = description;
    }


    public String getDescription() {
    	return description;
    }


    public void setDescription(String description) {
    	this.description = description;
    }
}

//正例：
public enum SwitchStatus {
    // 枚举的属性字段正例
    DISABLED(0, "禁用"),
    ENABLED(1, "启用");

    // final 修饰
    private final int value;
    private final String description;

    private SwitchStatus(int value, String description) {
    	this.value = value;
    	this.description = description;
    }

    // 没有Setter 方法
    public int getValue() {
    	return value;
    }


    public String getDescription() {
    	return description;
    }
}
```

# 注册表模型优化

这种方式和策略模式有相似之处，但注册表更自由，不需要提炼接口，只需要将自定义实现在注册表中注册即可。

```java
//反例
if (param.equals(value1)) {
    doAction1(someParams);
}else if (param.equals(value2)) {
    doAction2(someParams);
}else if (param.equals(value3)) {
    doAction3(someParams);
}

//正例
Map<?, Function<?> action> actionMappings = new HashMap<>();

actionMappings.put(value1, (someParams) -> { doAction1(someParams)});
actionMappings.put(value2, (someParams) -> { doAction2(someParams)});
actionMappings.put(value3, (someParams) -> { doAction3(someParams)});

actionMappings.get(param).apply(someParams);
```

# 浮点丢失精度

避免使用 == 运算符对浮点数变量进行比较，应该比较通过BigDecimal类来实现。

```java
//反例
boolean foo(double x, double y){
    return x == y;
}

//正例
boolean foo(String xStr,String yStr){
    return BigDecimal.valueOf(xStr).equals(BigDecimal.valueOf(yStr);
}
```

# 类型溢出的坑

不以L结尾的数值常量类型为int，对其进行乘法运算的结果完全有可能超出int能表示的范围而导致溢出。

```java
//反例：
static final long FOO == 24*60*60*1000*1000*1000;

//正例：
static final long FOO == 24L*60*60*1000*1000*1000;
//显式声明24长整形，避免结果溢出
```

# 类注解获取

调用注解类上实例的getClass() 方法将会返回一个随机代理类，想要获取实际的类型，应当使用 annotationType() 方法。

```java
//反例
@LieBao
public class Foo{
    static void m1(Anotation anno) {
        System.out.println(anno.getClass())
    }
}

//正例
@LieBao
public class Foo{
    static void m1(Anotation anno) {
        System.out.println(anno.annotationType())
    }
}
```

# 精度缺失的神坑

```java
如果使用 new BigDecimal(double d) 来将double 转换为BigDecimal 这种情况通常都会导致出现奇怪的精度损失。可以使用以下两种方式：
1. 工厂方法 BigDecimal.valueOf(double d)
2. new BigDecimal(String s) 构造器

//反例
BigDecimal ob = new BigDecimal(0.1);
System.out.println(ob);

//正例
BigDecimal of = BigDecimal.valueOf(0.2);
System.out.println(of);

BIgDecaimal on = new BigDecimal("0.3");
System.out.println(on);
```

# 线程安全sdf

java.text.SimpleDateFormate类不是线程安全的类，在多线程安全的情况下无法保证格式化结果的正确性，使用线程安全java.time.format.DateTimeFormatter类可以替换上述类，避免在多线程场景下出现问题。

```java
//反例
private static final SimpleDateFormat formatter = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");

public String fmt(Date date,DateFormat formatter) {
	return fomatter.format(date);
}

//正例
public String formatTime(LocalDateTime dateTime,DateTimeFormatter formatter){
	return fomatter.format(datetime);
}
```

# 线程池正确创建

线程池不允许使用Executors去创建，而是通过ThreadPoolExecutor的方式，规避资源耗尽的风险。

```java
// 反例
//通过Executors创建的线程池未设定线程数上限，存在导致资源耗尽风险
static final ExecutorService EXECUTOR_SERVICE = Executors.newCacheThreadPool();

void fun(Runnable task){
	EXECUTOR_SERVICE.submit(task);
}

// 正例
static final ThreadPoolExecutor EXECUTOR_SERVICE = new ThreadPoolExecutor(corePoolSize,maxiumPoolSize,1,TimeUnit.HOURS,
                                                                          new ArrayBlockingQueue<>(maxiumPoolSize));
```

# 线程调用执行

```java
创建了新的Thread实例但是未调用start() 方法来实际执行。

//反例
void foo(){
    Thread t = new Thread();
    //...
}

//正例
void foo(){
    Thread t = new Thread();
    //...
    t.start();
}
```

# 责任链模型优化

```java
//反例
public void handle(request) {
    if (handlerA.canHandle(request)) {
        handlerA.handleRequest(request);
    } else if (handlerB.canHandle(request)) {
        handlerB.handleRequest(request);
    } else if (handlerC.canHandle(request)) {
        handlerC.handleRequest(request);
    }
}

代码中也是存在一坨if else语句，但是和注册表不同之处在于，if条件判断权在每个handler组件中，每一个handler的判断方式也可能不尽相同，相当灵活，同一个request可能同时满足多个if条件。

//正例
public void handle(request) {
   handlerA.handleRequest(request);
}

public abstract class Handler {

  protected Handler next;

  public abstract void handleRequest(Request request);

  public void setNext(Handler next) { this.next = next; }
}

public class HandlerA extends Handler {
  public void handleRequest(Request request) {
    if (canHandle(request)) doHandle(request);
    else if (next != null) next.handleRequest(request);
  }
}

```

# 资损场景规约列举

*  DB更新操作，没有校验或传出update函数的返回值
*  BigDecimal除法没有指定精度参数
*  危险类型的金额变量：资金代码中使用double类型的局部变量
*  金额变量进行危险的类型转换，导致精度丢失
*  使用 == 或者!= 比较Java String对象
*  for循环以size方法作为边界，并在循环体中存在add remove
*  单例模式，多线程使用未加锁
*  异常捕获逻辑中包含空finally块
*  日期年份格式使用YYYY大写
*  日期转换未考虑闰年
*  Money类最小单位分非整数
*  禁止使用new BigDecimal(）把double/float转换为BigDecimal
*  BigDecimal类型对比不能使用equals

# 避免在Finally返回

禁止在Finally块中返回或者抛出异常，这样会导致try...catch块中的返回值和异常信息被覆盖。

```java
//反例
int foo{
    try{
        int res = 0;
        return res;
    } catch(Exception e) {
        throw CstException(e);
    } finally{
        return otherRes;
    }
}

//正例
int foo{
    try{
        int res = 0;
        return res;
    } catch(Exception e) {
        throw CstException(e);
    } finally{
        //使用try...withResource的方式避免使用Finally块
    }
}
```

# 避免空数组和集合

若程序运行返回null，需要调用方强制检测null，否则就会抛出空指针异常；返回空数组或空集合，有效地避免了调用方因为未检测null 而抛出空指针异常的情况，还可以删除调用方检测null 的语句使代码更简洁。

```java
//反例：
//返回null 反例
public static Result[] getResults() {
	return null;
}


public static List<Result> getResultList() {
	return null;
}

public static Map<String, Result> getResultMap() {
	return null;
}



//正例：
//返回空数组和空集正例
public static Result[] getResults() {
	return new Result[0];
}


public static List<Result> getResultList() {
	return Collections.emptyList();
}


public static Map<String, Result> getResultMap() {
	return Collections.emptyMap();
}
```

# 集合size大于0

集合的size 方法在集合为空时会返回0，size() >= 0 调用始终会返回true。

```java
//反例
void foo(Collection coll) {
	//始终为true
    if(coll.size() >= 0) {
        //...
    }
}

//正例
void foo(Collection coll) {
	//始终为true
    if(coll.isEmpty()) {
        //...
    }
}
```

# 集合不可操作异常

Arrays.asList 方法返回的集合会抛出 UnsupportedOperationException 。 这是由于Arrays.asList 方法返回的集合时java.util.Array$ArrayList 类，而非java.util.ArrayList类。

```java
//反例
void foo(String[] strs){
    List<String> tepmList = Arrays.asList(strs);
    tepmList.add("str");
}

//正例
void foo(String[] strs){
    List<String> tepmList = new ArrayList<>(Arrays.asList(strs));
    tepmList.add("str");
}
```

# 静态代码块赋值

对于集合类型的静态成员变量，应该使用静态代码块赋值，而不是使用集合实现来赋值。

```java
//反例：
//赋值静态成员变量反例
private static Map<String, Integer> map = new HashMap<String, Integer>(){
{
    map.put("Leo",1);
    map.put("Java",2);
    map.put("Coool",3);
}
};
private static List<String> list = new ArrayList<>(){
    {
        list.add("Sagittarius");
        list.add("Charming");
        list.add("Perfectionist");
    }
};



//正例：
//赋值静态成员变量正例
private static Map<String, Integer> map = new HashMap<String, Integer>();
static {
    map.put("Leo",1);
    map.put("Java",2);
    map.put("Coool",3);
}



private static List<String> list = new ArrayList<>();
static {
    list.add("Sagittarius");
    list.add("Charming");
    list.add("Perfectionist");
}
```

# 频繁contains

在Java 集合类库中，List的contains 方法普遍时间复杂度为O(n)，若代码中需要频繁调用contains 方法查找数据则先将集合list 转换成HashSet 实现，将O(n) 的时间复杂度将为O(1)。

```java
//反例：
//频繁调用Collection.contains() 反例
List<Object> list = new ArrayList<>();
for (int i = 0; i <= Integer.MAX_VALUE; i++){
    //时间复杂度为O(n)
    if (list.contains(i))
    System.out.println("list contains "+ i);
}



//正例：
//频繁调用Collection.contains() 正例
List<Object> list = new ArrayList<>();
Set<Object> set = new HashSet<>();
for (int i = 0; i <= Integer.MAX_VALUE; i++){
    //时间复杂度为O(1)
    if (set.contains(i)){
    System.out.println("list contains "+ i);
    }
}
```

