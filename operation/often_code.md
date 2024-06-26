# Java

## 集合

### group对象分组

在Java中，使用 **Stream** 和 **Collector**来根据列表中的某个字段进行分组。

下面定义一个 **MyObject** 类来表示列表中的对象，其中包含了 **name** 和 **group** 字段。使用 **Collectors.groupingBy** 方法根据 **group**字段进行分组，并将结果存储在一个 **Map** 中，其中键是分组的字段值，值是相应的对象列表。遍历分组的 **Map** 并打印结果。

```java
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;
import java.util.stream.Collectors;

public class Main {
    public static void main(String[] args) {
        // 有一个包含对象的列表，每个对象都有一个字段'group'用于分组
        List<MyObject> myList = new ArrayList<>();
        myList.add(new MyObject("Alice", "A"));
        myList.add(new MyObject("Bob", "B"));
        myList.add(new MyObject("Charlie", "A"));
        myList.add(new MyObject("Dave", "B"));
        myList.add(new MyObject("Eve", "A"));  
        
		// 使用Collectors.groupingBy根据'group'字段进行分组
   		Map<String, List<MyObject>> groups = myList.stream()
            	.collect(Collectors.groupingBy(MyObject::getGroup));

        // 遍历分组并打印结果
        for (Map.Entry<String, List<MyObject>> entry : groups.entrySet()) {
            String key = entry.getKey();
            List<MyObject> group = entry.getValue();
            System.out.println("Group " + key + ":");
            for (MyObject item : group) {
                System.out.println(item);
            }
            System.out.println();
        }
}

static class MyObject {
    private String name;
    private String group;

    public MyObject(String name, String group) {
        this.name = name;
        this.group = group;
    }

    public String getName() {
        return name;
    }

    public String getGroup() {
        return group;
    }

    @Override
    public String toString() {
        return "MyObject{" +
                "name='" + name + '\'' +
                ", group='" + group + '\'' +
                '}';
    }
}
}
```

### 获取对象某字段list集合

```java
//正常获取
List<SysOrgInfo> orgInfos = new ArrayList<>();
Set<String> orgCodes = orgInfos.stream().map(SysOrgInfo::getOrgCode).collect(Collectors.toSet());

//数据过滤，例如不为空
Set<String> userIds = sysEmployees.stream().map(SysEmployee::getUserId).filter(StringUtils::isNotEmpty)
                .collect(Collectors.toSet());
```

## mybatis plus

### 获取某个字段集合

```java
return this.listObjs(new LambdaQueryWrapper<TreatmentTemplateOrg>()
                     .select(TreatmentTemplateOrg::getTemplateId)
                     .eq(TreatmentTemplateOrg::getOrgCode, orgCode), Object::toString);
```

