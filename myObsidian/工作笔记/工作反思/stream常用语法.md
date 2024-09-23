获取对象列表的某个属性列表
```
list.stream().map(o -> o.getName()).collect(Collectors.toList())
```

获取列表中的重复字符
```
list.stream().collect(Collectors.groupingBy(e -> e, Collectors.counting())).entrySet().stream().filter(e -> e.getValue() > 1).map(Map.Entry::getKey).collect(Collectors.toList())
```

列表字符串去重
```
list.stream().collect(Collectors.toSet())
```