# java反射操作

* java反射实现对类的操作

```java
public class ReflectUse {
    /**
    * 使用反射中的部分方法完成登录操作
    */
    public static void reflectUse() {
        Class clazz = Employee.class;
        //如果是通过字符串读入,使用forName方法得到class对象
        try {
            //实例化对象
            //java老版本使用clazz.newInstance();
            Constructor constructor = clazz.getDeclaredConstructor();
            Object emp = constructor.newInstance();

            //设置属性
            Field field1 = clazz.getDeclaredField("name");
            Field field2 = clazz.getDeclaredField("password");
            Field field3 = clazz.getDeclaredField("age");
            field1.setAccessible(true);
            field2.setAccessible(true);
            field3.setAccessible(true);
            field1.set(emp, "admin");
            field2.set(emp, "admin");
            field3.set(emp, 18);

            //获取方法并使用
            //如果需要规定参数类型,需要增加类参数,clazz.getDeclaredMethod("method",parameter1.class);
            Method login = clazz.getDeclaredMethod("login");
            // invoke方法能够传入参数,可传入obj[]数组一一对应每个参数
            System.out.println(login.invoke(emp));

            Method toString = clazz.getDeclaredMethod("toString");
            System.out.println(toString.invoke(emp));
        }catch (Exception e) {
            e.printStackTrace();
        }

    }

    /**
    * 反射的基本操作
    */
    public void reflect(){
        try {
            Class clazz = Class.forName("com.java.reflect.Employee");
            //获取名称
            System.out.println(clazz.getName());
            System.out.println(clazz.getPackageName());
            //获取类的修饰符
            int modifiers = clazz.getModifiers();
            System.out.println(Modifier.isPublic(modifiers));

            //获取对应类加载器
            ClassLoader classLoader = clazz.getClassLoader();
            System.out.println(classLoader);

            //获取类的父类
            System.out.println(clazz.getAnnotatedSuperclass());

        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
    }
}
//进行反射的类
class Employee {
    private static int ID_FINAL = 1;
    private int id;
    private String name;
    private String password;
    private int age;

    static {
        ID_FINAL++;
    }

    public Employee() {
        this.id = ID_FINAL;
    }

    public Employee(int id, String name, String password, int age) {
        this.id = ID_FINAL;
        this.name = name;
        this.password = password;
        this.age = age;
    }

    public boolean login(){
        return "admin".equals(this.name) && "admin".equals(this.password);
    }

    @Override
    public String toString(){
        return "Employee [id=" + this.id + ", name=" + this.name + ", password=" + this.password + ", age=" + this.age + "]";
    }
}
```

* 类加载器
* 使用class.getClassLoader方法可以获取类的加载器,使用类加载器加载指定类
  * 核心加载器BootClassLoader:用于加载java核心的类如:String和Object,这种加载器是利用操作系统语言实现的,尝试获取该加载器将返回null
  * 平台类加载器PlatformClassLoader:java的类库由此加载器加载
  * 应用加载器AppClassLoader:个人编写的类由此加载器加载
* 加载过程是核心类->平台类->应用类

* 获取函数参数名称等信息:

```java
// 获取到method
Method method = clazz.getDeclaredMethod(methodNameStr);

// 获取参数列表
// 在jdk1.8之后,才能获取函数参数设置的名称,之前获取的名称为arg + 参数号,如arg0,arg1
// jdk1.8之后需要获取参数设置的名称需要在编译时增加参数 -parameters
Parameter[] parameters = method.getParameters();
```
