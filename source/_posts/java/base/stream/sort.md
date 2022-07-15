# Java8 Stream Sort

## 按照姓名首字母排序

```java
public class SortDemo{
    public void test(List<TableBodyRow> sourceList) {
        Collator collator = Collator.getInstance(Locale.CHINA);
        List<TableBodyRow> targetList = sourceList.parallelStream()
                .sorted(Comparator.comparing((o1, o2) -> collator.compare(o1.getName() == null ? "" : o1.getName(), o2.getName() == null ? "" : o2.getName())))
                .collect(Collectors.toList());
    }
}
```

### 多条件排序

```java
public class SortDemo{
    public void test(List<TableBodyRow> sourceList) {
        Collator collator = Collator.getInstance(Locale.CHINA);
        List<TableBodyRow> targetList = sourceList.parallelStream()
                .sorted(Comparator.comparing(TableBodyRow::getSex)
                        .thenComparing(TableBodyRow::getAge)
                        .thenComparing((o1, o2) -> collator.compare(o1.getName() == null ? "" : o1.getName(), o2.getName() == null ? "" : o2.getName())))
                .collect(Collectors.toList());
    }
}
```
