## 1、时间日期类

### 1、LocalDate；LocalTime；LocalDateTime类

```java
//获取当前时间
LocalDate.now();//年月日
LocalTime.now();//时分秒
LocalDateTime.now();//年月日时分秒
//获取指定时间
LocalDateTime.of(指定时间量);
//获取时间
getXxx();	//如：
now.getYear();//获取年
now.getMonth();//获取月（枚举类型）
now.getMonthValue();//月数（int类型）
//判断
now.isAfter(now2);//判断一个日期是否在指定日期后
now.isBefore(now2);//判断一个日期是否在指定日期前
now.isEqual(now2);//判断一个日期是否与指定日期相等
now.isLeapYear();//判断是否为闰年
//修改日期：每次修改完时间量都会返回新日期
now.withXxx(指定时间量);//指定时间
now.plusXxx();//增时间
now.minusXxx();//减时间
//格式化日期
DateTimeFormatter.ofPattern("yyyy年MM月dd日");
now.format(DateTimeFormatter formatter)
//解析日期
parse();
```

```java
import java.time.LocalDate;
import java.time.LocalDateTime;
import java.time.LocalTime;
import java.time.Month;
import java.time.format.DateTimeFormatter;
import java.time.temporal.TemporalAccessor;

public class MyTest {
    public static void main(String[] args) {
        //获取当前时间
        LocalDate now = LocalDate.now();
        LocalTime now1 = LocalTime.now();
        LocalDateTime now2 = LocalDateTime.now();
        //获取指定时间
        LocalDateTime of = LocalDateTime.of(2015, 9, 1, 12, 12, 12);
        //获取时间
        of.getYear();//获取年
        Month month = of.getMonth();//获取月
        of.getSecond();//获取秒
        //判断
        boolean leapYear = now.isLeapYear();//判断是否为闰年
        boolean after = now2.isAfter(of);//判断一个日期是否在指定日期后
        boolean before = now2.isBefore(of);//判断一个日期是否在指定日期前
        boolean equal = now2.isEqual(of);//判断一个日期是否与指定日期相等
        //修改时间：修改完返回的是新日期
        now.withYear(2011);//指定年
        now.plusYears(1);//年加1
        now.minusYears(1);//年减1
        //格式化日期
        DateTimeFormatter dateTimeFormatter = DateTimeFormatter.ofPattern("yyyy年MM月dd日 HH:mm:ss");
        String format = now2.format(dateTimeFormatter);
        System.out.println(format);
        //解析日期
        LocalDate parse = LocalDate.parse("2019年08月04日", DateTimeFormatter.ofPattern("yyyy年MM月dd日"));
        System.out.println(parse);
        DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy年MM月dd日 HH:mm:ss");
        TemporalAccessor parse1 = formatter.parse("2019年08月01日 10:10:10");
        System.out.println(parse1);
    }
}
```

### 2、Instant 时间戳类

```java
A：获取对象的方法： now()
	注意默认获取出来的是当前的美国时间和我们相差八个小时
		Instant ins = Instant.now();
		我们在东八区，所以可以加8个小时，获得我们的北京时间
B：Instant中设置偏移量的方法:  atOffset()
    	//偏移8小时，即当前北京时间
		OffsetDateTime time = ins.atOffset(ZoneOffset.ofHours(8));
C：获取指定时区时间的方法： atZone()
		ZoneId.systemDefault() //获取本地的默认时区ID
    	//获取系统默认时区时间
		ZonedDateTime zonedDateTime = ins.atZone(ZoneId.systemDefault());
D：get系列的方法
	getEpochSecond() //获取从1970-01-01 00:00:00到当前时间的秒值
	toEpochMilli();//获取从1970-01-01 00:00:00到当前时间的毫秒值
	getNano()//把获取到的当前时间的毫秒数，换算成纳秒
	如：若当前时间为：2019-08-04T15:09:51.208+08:00
		则getNano()返回值为：208000000	
F： ofEpochSecond(int second) 给计算机元年增加指定秒数
	ofEpochMilli(int milli) 给计算机元年增加指定毫秒数		
```

### 3、Duration和Period

```java
Duration : 用于计算两个“时间”间隔的类
静态方法:between() //计算两个时间的间隔,默认是秒
如：要计算for循环耗时
	Instant start = Instant.now();
		for(long i=0;i<1000L;i++){
			System.out.println("循环内容");
		}
	Instant end = Instant.now();
	Duration between = Duration.between(start, end);
	System.out.println(between.toMillis());
	toMillis()//将秒转成毫秒
Period : 用于计算两个“日期”间隔的类
静态方法: between(LocalDate d1,LocalDate d2)//计算两个日期之间的间隔
		getYears();//间隔了多少年
		getMonths();//间隔了多少月
		getDays();//间隔多少天
```

### 4、TemporalAdjuster（接口）

```java
1、使用TemporalAdjusters自带的常量来设置日期：
	//一年的最后一天
	LocalDate with = LocalDate.now().with(TemporalAdjusters.lastDayOfYear());
2、采用TemporalAdjusters中的next方法来指定日期
	 LocalDate date = LocalDate.now().with(TemporalAdjusters.next(DayOfWeek.SUNDAY));
	例如:TemporalAdjusters.next(DayOfWeek.SUNDAY) 本周的星期天
	例如:TemporalAdjusters.nextOrSame(DayOfWeek.MONDAY) 下一周的星期一
3、采用自定义的方式来指定日期，比如指定下个工作日
	LocalDateTime ldt = LocalDateTime.now();
	LocalDateTime workDay = ldt.with(new TemporalAdjuster() {
		@Override
	 public Temporal adjustInto(Temporal temporal) {
		 //向下转型
		 LocalDateTime ld = (LocalDateTime) temporal;
		 //获取这周的星期几
		 DayOfWeek dayOfWeek = ld.getDayOfWeek();
	 if (dayOfWeek.equals(DayOfWeek.FRIDAY)) {
			 return ld.plusDays(3);//如果这天是星期五,那下个工做日就是再过3天
		} else if (dayOfWeek.equals(DayOfWeek.SATURDAY)) {
			return ld.plusDays(2);//如果这天是星期六,那下个工做日就是再过2天
		} else {
			//其他就加一天
			return ld.plusDays(1);
	 }
        }
    });
    System.out.println(workDay);	
```

### 5、ZonedDate,ZonedTime,ZonedDateTime:带时区的时间或日期

```
  用法和 LocalDate、LocalTime、LocalDateTime一样，
  只不过ZonedDate,ZonedTime,ZonedDateTime 这三个类带有当前系统的默认时区
```

### 6、ZoneID 世界时区类

```java
getAvailableZoneIds();//获取世界各个地方的时区的集合
ZoneId zoneId = ZoneId.systemDefault(); //Asia/Shanghai
ZoneId timeID=ZoneId.of("Asia/Shanghai");//获取指定时区ID对象
//根据指定时区ID对象获取指定时区当前时间
ZonedDateTime time=LocalDateTime.now().atZone(timeID);
```

## 2、Lambda表达式

```
Lambda表达式是匿名内部类的简写方式，但必须函数式接口,才能用Lambda表达式
函数式接口的定义: 只包含一个抽象方法的接口
```

#### Java给我们提供的函数式接口

**4大核心函数式接口**

| **函数式接口**               | **参数类型** | **返回 类型** | **用途**                                                     |
| ---------------------------- | ------------ | ------------- | ------------------------------------------------------------ |
| **Consumer 消费型接口**      | T            | void          | 对类型为T的对象应用操作，包含方法： void accept(T t)         |
| **Supplier 供给型接口**      | 无           | T             | 返回类型为T的对象，包含方法： T get();                       |
| **Function<T, R>函数型接口** | T            | R             | 对类型为T的对象应用操作，并返回结果。结果是R类型的对象。包含方法： R apply(T t); |
| **Predicate 断言型接口**     | T            | boolean       | 确定类型为T的对象是否满足某约束，返回boolean 值。包含方法 boolean test(T t); |

**其他函数式接口**

| **函数式接口**                                    | **参数类型**    | **返回类型**    | **用途**                                                     |
| ------------------------------------------------- | --------------- | --------------- | ------------------------------------------------------------ |
| **BiFunction<T,U,R>**                             | T U             | R               | 对类型为 T, U 参数应用操作， 返回 R 类型的结果。 包含方法为 R apply(T t, U u); |
| **UnaryOperator (Function的子接口)**              | T               | T               | 对类型为T的对象进行一元运算， 并返回T类型的结果。 包含方法为 T apply(T t); |
| **BinaryOperator (BiFunction的子接口)**`          | T T             | T               | 对类型为T的对象进行二元运算， 并返回T类型的结果。包含方法为T apply(T t1, T t2); |
| **BiConsumer<T,U>**                               | T U             | void            | 对类型为T, U 参数应用操作。 包含方法为 void accept(T t, U u) |
| **ToIntFunction ToLongFunction ToDoubleFunction** | T               | int long double | 分 别 计 算 int 、 long 、 double、 值的函数                 |
| **IntFunction LongFunction DoubleFunction**       | int long double | R               | 参数分别为int、 long、 double 类型的函数                     |

**形式一**

```java
import java.util.Arrays;
import java.util.Comparator;
import java.util.List;

public class MyTest {
    public static void main(String[] args) {
        List<Integer> list = Arrays.asList(10, 20, 1);
        //匿名内部类的写法
        list.sort(new Comparator<Integer>() {
            @Override
            public int compare(Integer a, Integer b) {

                return a-b;
            }
        });
        
        //第一步 简写，->左端是方法的参数列表，右端是方法的实现逻辑
        list.sort((Integer x,Integer y)->{
            return x-y;
        });
        //第二步简写 形参的数据类型可以省略不写
        list.sort((x,y) -> {
            return x - y;
        });

        //第三步简写：如果方法实现逻辑只有一行，可以省略{} 和 return
        //如果实现逻辑不是一行，则不能省略{}和return
        list.sort((x,y)->x-y);
    }
}
```

**形式二：方法引用**

```shell
条件:实现的抽象方法的参数列表和返回值类型，必须与方法引用方法的参数列表和返回值类型保持一致！
```

```java
对象::实例方法
Consumer<String> con=(str)-> System.out.println(str);
        //方法引用形式
        Consumer<String> con1= System.out::println;
类::静态方法
BinaryOperator<Integer> bin=(a,b)->Math.max(a,b);
        //方法引用形式
        BinaryOperator<Integer> bin1= Math::max;
类::实例方法
//当实现的抽象方法的第一个参数是引用方法的调用对象，并且第二个参数是引用方法的参数时，可使用 类::实例方法 格式
Comparator<String> com=new Comparator<String>() {
            @Override
            public int compare(String o1, String o2) {
                return o1.compareTo(o2);
            }
        };
        //方法引用形式
        Comparator<String> com1=String::compareTo;
```

**形式三：构造器引用**

```java
格式：ClassName::new
Supplier<MyTest> su=()->new MyTest();
//构造器引用形式
Supplier<MyTest> su1= MyTest::new;

Function<String,MyTest> fun=(str)->new MyTest(str);
//构造器引用形式
Function<String,MyTest> fun1= MyTest::new;
```

## 3、Stream流

### 1、Stream概述

```
概述：Stream流是数据渠道，用于操作数据源（集合、数组等）所生成的元素序列。
注意：
	Stream流 自己不会存储元素。
	Stream流 不会改变源对象。相反，他们会返回一个持有结果的新Stream。
	Stream流 操作是延迟执行的。这意味着他们会等到需要结果的时候才执行。
Stream 的操作三个步骤：
	1.创建 Stream
		一个数据源（如：集合、数组），获取一个流
	2.中间操作
		一个中间操作链，对数据源的数据进行处理
	3.终止操作(终端操作)
		一个终止操作，执行中间操作链，并产生结果
```

### 2、获取Stream流

```java
1.Java8 中的 Collection 接口被扩展，提供了两个获取流的方法：
		default Stream<E> stream() : 返回一个顺序流
		default Stream<E> parallelStream() : 返回一个并行流 
2.Java8 中的 Arrays 的静态方法 stream() 可以获取数组流：
		static <T> Stream<T> stream(T[] array): 返回一个流
		 重载形式，能够处理对应基本类型的数组：
		 public static IntStream stream(int[] array)
		 public static LongStream stream(long[] array)
		 public static DoubleStream stream(double[] array) 
3.由值创建流,可以使用静态方法 Stream.of(), 通过显示值创建一个流。它可以接收任意数量的参数。
		 public static<T> Stream<T> of(T... values) : 返回一个流 
4.由函数创建流：创建无限流可以使用静态方法 Stream.iterate()和Stream.generate(), 创建无限流。
public static<T> Stream<T> iterate(final T seed, finalUnaryOperator<T> f)//迭代
public static<T> Stream<T> generate(Supplier<T> s)//生成
```

### 3、Stream 的中间操作

```java
注意：多个中间操作可以连接起来形成一个流水线，除非流水线上触发终止操作，否则中间操作不会执行，
在终止操作时一次性全部处理，称为“惰性求值”。
1.筛选与切片
	filter(Predicate p)//过滤,从流中排除某些元素。
	distinct() //去重，通过流所生成元素的 hashCode() 和 equals() 去除重复元素
	limit(long maxSize) //截断流，使其元素不超过给定数量。
	skip(long n)//跳过元素，返回一个扔掉了前 n 个元素的流。若流中元素不足 n 个，则返回一个空流。与 limit() 互补
2.映射
	map(Function f)	//接收一个函数作为参数，该函数会被应用到每个元素上，并将其映射成一个新的元素。
	flatMap(Function f)	//接收一个函数作为参数，将流中的每个值都换成另一个流，然后把所有流连接成一个流.
	mapToDouble(ToDoubleFunction f)//接收一个函数作为参数，该函数会被应用到每个元素上，产生一个新的 DoubleStream。
	mapToInt(ToIntFunction f)//接收一个函数作为参数，该函数会被应用到每个元素上，产生一个新的 IntStream。
	mapToLong(ToLongFunction f)//接收一个函数作为参数，该函数会被应用到每个元素上，产生一个新的 LongStream。
	
3.排序
	sorted()//产生一个新流，其中按自然顺序排序,元素实现Compareble接口
	sorted(Comparator comp)	//产生一个新流，其中按比较器顺序排序,传入一个比较器
```

### 4、Stream 的终止操作

```java
注意：终端操作会从流的流水线生成结果。其结果可以是任何不是流的值，例如：List、Integer，甚至是 void 。
1.查找与匹配
	allMatch(Predicate p)//检查是否匹配所有元素,即有一个元素不匹配，则为false
	anyMatch(Predicate p)//检查是否至少匹配一个元素,即有一个匹配则返回true
	noneMatch(Predicate p)//检查是否没有匹配所有元素
	findFirst()	//返回第一个元素
	findAny()//返回当前流中的任意元素
	count()	//返回流中元素总数  
	max(Comparator c)//返回流中最大值 
	min(Comparator c)//返回流中最小值 
	forEach(Consumer c)	//内部迭代
2.归约
	reduce(T iden, BinaryOperator b) //参1是起始值, 参2二元运算,可以将流中元素反复结合起来，得到一个值,返回 T 。
	reduce(BinaryOperator b) //这个方法没有起始值,可以将流中元素反复结合起来，得到一个值。返回 Optional<T> 
	备注：map 和 reduce 的连接通常称为 map-reduce 模式，因 Google 用它来进行网络搜索而出名。
3.收集
	collect(Collector c)//将流转换为其他形式。接收一个Collector接口的实现，用于给Stream中元素做汇总的方法
	Collector 接口中方法的实现决定了如何对流执行收集操作(如收集到 List、Set、Map)。
	Collectors 实用类提供了很多静态方法，可以方便地创建常见收集器实例;
4.Collectors 中的方法
	List<T> toList()//把流中元素收集到List
	Set<T>  toSet()//把流中元素收集到Set
	Collection<T> toCollection()//把流中元素收集到创建的集合 
	例:把所有员工的名字通过map()方法提取出来之后,在放到指定的集合中去:
    Collection<Employee>emps=list.stream().map(提取名字).collect(Collectors.toCollection(ArrayList::new));
	Long counting()//计算流中元素的个数
	Integer	summingInt()//对流中元素的整数属性求和
	Double averagingInt()//计算流中元素Integer属性的平均值
	IntSummaryStatistics summarizingInt()//收集流中Integer属性的统计值。
		例子:DoubleSummaryStatistics dss= list.stream().collect(Collectors.summarizingDouble(Employee::getSalary));
		从DoubleSummaryStatistics 中可以获取最大值,平均值等
	double average = dss.getAverage();//平均值
	long count = dss.getCount();//个数
	double max = dss.getMax();//最大值
	String joining(String str) //使用str连接流中每个字符串
	Optional<T> maxBy() //根据比较器选择最大值
	Optional<T> minBy() //根据比较器选择最小值
	Map<K, List<T>> groupingBy() //根据某属性值对流分组
	Map<Boolean, List<T>> partitioningBy() //根据true或false进行分区
```

> 参考引用链接：https://blog.csdn.net/y_engineer/article/list/1