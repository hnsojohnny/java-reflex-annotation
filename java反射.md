## <center>java反射</center>

#### 反射机制是在运行状态中，对于任意的一个实体类，我们都能知道这个类的所有属性方法；对于任意一个对象，都能调用它的任一方法和属性。

公共类：
```
public class People{
  public String sex;
}

public class Stu extends People{
  public String name;
  private int age = 13;
  public Stu(String name){

  }
  private void play(String name){

  }
}
Stu stu = new Stu();
```

### 获取Class
_java面向对象，所有要获取类的属性方法，要先获取到类的对象_

* `Object.getClass()` 直接利用对象获取响应的class对象。
  ```
    Class clz = stu.getClass();
  ```
* `.class` 直接类名`.class`，不实例化对象。
   ```
     Class clz = Stu.class;
   ```
* `Class.forName()` 根据类的包名+类名来获取。
   ```
     Class clz = Class.forName("包名+Stu");
   ```

### 属性
#### 获取属性
_获取属性两种方法：_
* `getDeclaredField()`获取类的属性包括公共属性和私有属性，但不能获取到从父类继承的属性。
   ```
   Class clz = Stu.class;
   获取指定属性
   Field field = clz.getDeclaredField("age");
   获取类除了父类继承的属性外的所有属性
   Field[] feilds = clz.getDeclaredFields();
   ```
* `getField`获取类的所有公共属性，包括父类继承的公共属性。
   ```
   Class clz = Stu.class;
   获取指定属性
   Field field = clz.Field("name");
   获取类除了私有属性外所有的共有属性，包括父类继承的
   Field[] feilds = clz.getFields();
   ```
#### 读取属性值
```
比如我们获取`stu`的`age`
Class clz = Stu.class;
Field field = clz.getDeclaredField("age");
如果属性为私有属性，要对属性进行操作要加：
field.setAccessible(true);
ing age = field.getInt(stu);
```
#### 给属性赋值
```
第一个参数表示要放入的对象，第二参数为要给属性赋值的参数
field.set(stu, 16);
```

### 方法
#### 获取方法
获取方法和获取属性类似，只不过是把获取属性的`field`单词改为`method`:
```
getDeclaredMethods();
getMethods();
```
但是获取某个方法需要多加一个参数(第一个参数为方法名字，第二个参数为方法里面的参数数据类型，如果没有参数，就为`null`):
比如我们获取`stu`的`play`方法；
```
Method method = stu.getDeclaredMethod("play", String.class);
```
#### 方法的执行
```
第一个参数为方法所在的类的实体类，第二个参数为方法的参数
method.setAccessible(true);
method.invoke(stu,"小明");
```
### Constructor
#### 获取到Constructor
```
Constructor constructor = stu.getConstructor(String.class);
```
#### 利用Constructor创建对象
```
Stu stu = (Stu)constructor.newInstance("李三");
```
直接用：
`Stu stu = stu.newInstance();`也可以创建对象，但是只是无参的情况,并且只适应于构造函数为`public`。而Constructor则没有此限制。

#### 容易忘记：
如果`field`,`method`,`Constructor`不为public修饰的时候要记得加上‘setAccessible’方法。
