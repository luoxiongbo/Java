# 1.JDBC

Java  DataBase  Connectivity

### 1. JDBC的本质

SUN公司指定的一套接口

面向接口编程: 

​		解耦合 : 降低程序的耦合度, 提高程序的扩展力(比如多态)

- SUN公司写了一个接口
  - 各大厂商对这个接口进行实现
  - 这个接口实现了"多态"的编程思想
- 面向接口编程的程序员
  - 只需要调用JDBC即可
  - 底层的实现是厂商写的代码, 程序员也不用去关心
- 厂商写的实现接口的.class文件也是驱动(Driver)
- 数据库驱动都是以.jar包的形式存在, .jar包当中有很多.class文件, 它们实现了JDBC接口

模拟实现

配置文件里写上数据库, 那么程序员只需要从配置文件中得到数据库类型, 其他的任何都不用改

这更好的实现了面向接口编程

### 2.JDBC开发前的准备工作

如果是记事本的话, 需要把.jar包配置到环境变量classpath当中

## 3.JDBC编程六步(重)

- 注册驱动, 必须将**jar**(数据库厂商写的驱动)文件引入

```java
com.mysql.cj.jdbc.Driver driver = new com.mysql.cj.jdbc.Driver();
// 类加载常用
Class.forName("com.mysql.cj.jdbc.Driver");
注意上面参数是一个字符串, 字符串可以写到xxx.properties文件中
```

- 获取连接

```java
String url = "jdbc:mysql://localhost:3306/maoyu";

Properties properties = new Properties();
properties.setProperty("user", "root");
properties.setProperty("password", "root");

Connection connect = driver.connect(url, properties);
```

- 获取数据库操作对象

```java
Statement statement = connect.createStatement();
```

- 执行SQL
  - excuteUpdate执行DML(insert, delete, update)
  - excuteQuery(select)

```java
String sql = "insert into actor values('罗雄波', '学生', 300)";

int rows = statement.executeUpdate(sql);		// 返回值是:影响数据库中的记录条数
```

- 处理查询结果集(有上一步才有这一步)

- 释放资源

```java
statement.close();
connect.close();
```



⚠:

- URL统一资源定位器

  - 协议(https)
    - 通信之前提前定好的数据传送格式

  - IP(localhost)
  - PORT(3306)
  - 资源名(mysql_maoyu)

### 4. JDBC常用操作实现

```java
Properties properties = new Properties();
// 引入了配置文件
properties.load(new FileInputStream("src\\mysql.properties"));

String url = properties.getProperty("url");
String user = properties.getProperty("user");
String password = properties.getProperty("password");
String driver = properties.getProperty("driver");

Class.forName(driver);

Connection connection = DriverManager.getConnection(url, user, password);
System.out.println("方式5"+connection);
```

### 5.处理查询结果集

当使用SQL中的查询语句的时候, 查询的结果会以结果集的形式返回一个集合

我们将对查询结果集进行处理, 输出或者其他操作

⚠:结果集有一个指针, 这个指针开始时指向第一行的上面, 调用.next()会指向第一行

​	读到一行, 如果要将每一列读取出来, 那么记住行数是从1开始的

​	为了使程序更加健壮, getString的时候参数传的是查询结果集中的列名

### 6.IDEA上使用JDBC

Java工程导入依赖

- 项目创建lib文件夹
- 将我桌面上的那个jar包复制到这个文件夹下面
- 然后将粘贴进来的包右击Add as library

![image-20230623011718866](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230623011718866.png)

- 添加完毕

老杜方式

- 右击项目中的Open Moudule Settings

![image-20230623013039780](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230623013039780.png)

- 点击

![image-20230623013134767](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230623013134767.png)

- 点击

![image-20230623013415165](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230623013415165.png)

- 点击

![image-20230623013501462](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230623013501462.png)

- 点击

![image-20230623013540115](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230623013540115.png)

- 点击

![image-20230623013557506](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230623013557506.png)

- 结束



