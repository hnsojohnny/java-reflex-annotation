## <center>java注解</center>

### 注解
> [这个可以更好的理解注解](https://blog.csdn.net/briblue/article/details/73824058)

> [注解的使用](https://blog.csdn.net/xiaohanluo/article/details/52538909)

### 注解类的写法
```
public @interface Test(){
}
```
这样就写了一个名为`Test`的注解。

### `java`内置注解
  * `@Override` 检查某个方法是否是复写方法。
  * `@Deprecated` 标记某个方法或者类已经过时，或者废弃。
  * `@SuppressWarnings` 忽略警告，阻止警告，编译的时候如果有警告，会忽略警告。
  * `@SafeVarargs` 参数安全类型注解。忽略关于调用含有泛型参数的方法或构造器的警告。
  * `@Functionallnterface` 表明某一个声明的接口将被用作功能性接口，就是一个具有一个方法的普通接口。

### `java`元注解
java中元注解有`@Retention`、`Documented`、`Target`、`Inherited`、`Repeatable`5种。
  * `@Retention` 表示一个注解保留时间。
  * `@Documented` 指标注的注解被写入`javadoc`文档中。
  * `@Target` 指出注解运用的地方
  * `@Inherited` 如果一个注解被`inherited`标注，如果一个父类被这个注解进行注解的话，那么他的子类没有被任何注解应用的话，这个子类就继承了超类的注解。
  * `@Repeatable` 被标注的注解可以多次作用于同一个对象。

#### `@Retention`
`Retention`表示注解保留时间有三种：
  * `RetentionPolicy.SOURCE` 注解只在源码阶段保留，在编译器进行编译时它将被忽略。
  * `RetentionPolicy.CLASS` 注解只被保留到编译进行的时候，他不会被加载到`JVM`中。在编译的过程中保存到`class`文件，在`class`文件被加载时忽略。
  * `RetentionPolicy.RUNTIME` 注解可以保留到程序运行的时候，他会被加载到`JVM`中。

#### `Target`
`Target`表示注解运用的地方：
  * `ElementType.TYPE` 在类标注
  * `ElementType.FIELD` 在属性标注
  * `ElementType.METHOD` 在方法上标注
  * `ElementType.PARAMETER` 用于描述参数
  * `ElementType.CONSTRUCTOR` 用在构造器标注
  * `ElementType.LOCAL_VARIABLE` 用于描述局部变量
  * `ElementType.ANNOTATION_TYPE` 用于表述注解
  * `ElementType.PACKAGE` 用于描述包

### 注解列举
```
如果只有一个属性并且是value命名的话，就可以直接赋值
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.FIELD)
public @interface Test{
  String value();
  String name();
}

public class Test{
  @Test("123")
  private String test;
}
```
继承：
```
@Inherited
@Retention(RetentionPolicy.RUNTIME)
@interface Test{}

@Test
public class TestA{}

public class TestB extends TestA{}
```
重复
```
@interface Persons{
  Person[] value();
}

@Repeatable(Persons.class)
@interface Person{
  String name();
}

@Person("张三")
@Person("李四")
@Person("王五")
public class TestPerson{
}
```

### 利用反射获取注解信息
类为了方便都是乱写的。
#### 获取类注解
```
Class clz = Test.class;
Test test = (Test)clz.getAnnotation(Test.class);
```
#### 获取属性注解
```
Class clz = Test.class;
Field field = clz.getDeclaredField("test",String.class);
Test test = (Test)field.getAnnotation(Test.class);
```
#### 获取方法注解和获取属性注解类似
