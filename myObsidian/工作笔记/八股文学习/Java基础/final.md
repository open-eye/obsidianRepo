用于修饰不同的地方不同的作用
### 修饰变量
变量又分为基础类型，引用类型
- 修饰基础类型，则引用为常量，该值定义后无法修改
- 修饰引用类型，比如对象、数组，对象可以修改属性值，数组可以增减元素，但变量地址定义后无法修改
被final修饰的变量在定义的时候就要赋值，否则会编译报错
### 修饰方法
被final修饰的方法无法被子类重写，但该方法依然可以被子类继承

### 修饰类
被final修饰的类，无法被继承