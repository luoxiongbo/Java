# JDBC

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

### 3.JDBC编程六步(重)

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

> 获取驱动
>
> 建立连接
>
> 获取执行SQL的对象
>
> 执行SQL
>
> [ 处理查询结果集 ]
>
> 释放资源

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

// 加载驱动可以不写但建议写一下
Class.forName(driver);

// 得到连接
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

### 7. 示例一: 用户登录业务介绍

powerDesigner的安装和使用(开发阶段使用)

业务步骤

- 表的设计
- 用户界面设计
- Java代码实现



⚠:SQL注入

用户输入的信息中**含有SQL关键字的语句**, 并且这些关键字**参与了SQL语句的编译**过程, **导致SQL语句的原义被扭曲**, 进而达到SQL注入

SQL注入的解决:

- 只要是含有SQL关键字的语句不进行编译就可以

- PreparedStatement : 预编译的数据库操作对象

  - 继承了Statement接口

  - 原理:预先对SQL语句的框架进行编译, 然后再给SQL语句传"值"

    - 所以你在输入的值就只是一个字符串, 不参加编译
    - 因为只对SQL框架进行了编译(发送SQL语句框架给DMBS, 然后DMBS进行SQL语句的预编译)

    

|          | Statement          | PreparedStatement                    |
| -------- | ------------------ | ------------------------------------ |
| 注入问题 | 存在SQL注入        | 不存在SQL注入                        |
| 编译问题 | 编译一次, 执行一次 | 编译一次, 可多次执行                 |
| 安全问题 | 没有安全检查       | 会在编译阶段做类型的安全检查(String) |

⚠: 必须使用Statement的情况:

- 需要SQL注入和拼接的时候
  - desc 和 asc

### 8.JDBC的事务自动提交机制的演示

测试结果 : JDBC中只要执行任意一条DML语句, 就提交一次(Debug的时候发现打了断点之前的程序已经出结果了, 说明执行一条就提交一次)

但是在实际业务中, 通常都是N条DML语句共同联合才能完成的, 必须保证他们这些DML语句在同一事务中同时成功或者同时失败.



### 9. 工具类DBUtil

显然连接和释放资源是很常用的, 为了减少代码冗余, 增加代码复用, 所以将连接和释放资源封装起来, 方便使用

也是行级锁的实例演示:

```java
Connection conn = DBUtil.getConnection();

String sql = "select ename, job, sal from emp where job = ? for update";
ps = conn.prepareStatement(sql);
ps.setString(1, "MANAGER");
re = ps.executeQuery();
while(re.next()) { 		       System.out.println(rs.getString("ename")+","+rs.getString("jon")+","+rs.getDouble("sal"));
}

conn.setAutoCommit(false);
comm.commit();
```



### 10.JDBC实现模糊查询(?的使用)

```java
try {
     Connection connection = DriverManager.getConnection(url, username, password);
     PreparedStatement statement = connection.prepareStatement("SELECT * FROM employees WHERE name LIKE ?")) {

    // 设置模糊查询参数
    String keyword = "John"; // 搜索关键字
    statement.setString(1, "%" + keyword + "%");

    // 执行查询
    ResultSet resultSet = statement.executeQuery();

    // 处理查询结果
    while (resultSet.next()) {
        int id = resultSet.getInt("id");
        String name = resultSet.getString("name");
        // 处理每条记录...
        System.out.println("ID: " + id + ", Name: " + name);
    }

} catch (SQLException e) {
    e.printStackTrace();
}
```

PreparedStatement的使用, 返回ResultSet查询结果集

### 11.悲观锁(行级锁)和乐观锁

乐观锁和悲观锁的区别就是 : 他们对待并发的态度是否是悲观和乐观的

- 悲观锁对待并发就是悲观的, 它假设并发冲突, 因此在操作数据之前就获取了锁

- 乐观锁对待并发就是乐观的, 它假设并发冲突是很少发生的, 因此不对数据进行加锁, 而是在提交更新时检查是否有其他事务对数据进行了修改
  - 如果发现该事务版本号没有发生变化, 那么就更新操作
  - 如果发现该事务版本号发生了变化, 那么就回滚, (或者重新尝试)



### 12. 批处理

Batch批处理, 将1000条SQL语句一起发送给MySQL数据库进行处理, 而且搭配PreparedStatement使用可以使其只编译一次, 那么就只需要一次编译和5次发送, 执行多个SQL语句的机制，以减少与数据库的通信次数，提高性能。

通过使用批处理，可以将多个SQL语句一次性发送给数据库执行，从而减少网络开销和数据库操作的开销。

1. 创建`Statement`或`PreparedStatement`对象：首先需要创建一个`Statement`或`PreparedStatement`对象，用于执行SQL语句。
2. 设置批处理模式：通过调用`Statement`或`PreparedStatement`对象的`addBatch()`方法，将要执行的SQL语句添加到批处理队列中。可以多次调用`addBatch()`方法，添加多个SQL语句。
3. 执行批处理：通过调用`Statement`或`PreparedStatement`对象的`executeBatch()`方法，执行批处理队列中的所有SQL语句。该方法将返回一个`int[]`数组，表示每个SQL语句执行后所影响的行数。
4. 处理执行结果：根据需要，可以对批处理的执行结果进行处理。例如，可以根据`int[]`数组中的返回值判断每个SQL语句的执行成功与否。

### 13.数据库池

-  传统的JDBC数据库连接, 每次向数据库建立连接的时候都要将Connection加载到内存中, 再验证IP地址, 用户名和密码, 需要连接数据库的时候, 就向数据库要求一个, 这说明频繁的连接操作将占用很多的系统资源, 容易造成服务器奔溃

1. **连接开销**：与数据库建立连接是一项开销较大的操作，涉及网络通信、身份验证、资源分配等步骤。频繁地创建和关闭连接会增加系统的开销。
2. **并发需求**：随着应用程序和用户量的增加，数据库的并发访问需求也相应增加。如果每个用户请求都独立创建一个连接，会导致连接资源的浪费和数据库的压力过大。
3. **长时间连接**：有些应用场景，比如Web应用，需要保持与数据库的持久连接，以提高响应速度。但长时间的持久连接可能会占用数据库的并发连接数，造成资源浪费。

那么可以采用数据库连接池化技术来解决上述问题

它通过预先创建一定数量的数据库连接，并将这些连接保存在连接池中，以便随时供应用程序获取和使用。连接池负责对连接进行管理、分配和释放，从而减少了连接的创建和关闭操作，提高了系统性能和资源利用率。

⚠:放回去不是close()

![image-20230625145530588](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230625145530588.png)

- C3P0
  - 建立一个池
  - 对池进行初始化
  - 得到连接

- 德鲁伊德
  - 配置文件
  - 将配置信息传参给Druid仓库函数
  - 从Druid中得到连接



### 14. Apache-DBUtils

**问题的引出:**

传统连接数据得到的数据查询集的**缺点**

- **关闭connection后,** **result结果集无法使用**
  - 结果集合与connection是**关联的**, 即如果关闭连接就不能再使用结果集
- resultSet不利于数据的管理
  - 方法太少并且枯燥, 只有自己的get[列]的方法, 所以不利于处理和管理

![image-20230625221745948](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230625221745948.png)

**土方法实现DBUtils:**

1. 首先是建立一个关系类, 该类来接收每一条查询语句
2. 当断开连接时并不影响该类对象的数据的数据
3. 而且可以更好地对数据进行操作

```java
actors.add(new Actor(id, name, sex, borndate, phone));
```

**DBUtils的使用:**

- commons-dbutils 是 Apache组织提供的一个开源JDBC工具类库，它是对JDBC的封
  使用dbutils能极大简化jdbc编码的工作量[真的]。

- DbUtils类

  1. QueryRunner类:该类封装了SQL的执行，是线程安全的。可以实现增、删、改、查、批处理
  2. 使用QueryRunner类实现查询
  3. ResultSetHandler接口:该接口用于处理java.sql.ResultSet，将数据按要求转换为另一种形式

  

- ArrayHandler:把结果集中的第一行数据转成对象数组。
- ArrayListHandler:把结果集中的每一行数据都转成一个数组，再存放到List中。
- BeanHandler:将结果集中的第一行数据封装到一个对应的JavaBean实例中。
- BeanListHandler:将结果集中的每一行数据都封装到一个对应的JavaBean实例中，存放到List里。
- ColumnListHandler:将结果集中某一列的数据存放到List中。
- KeyedHandler(name):将结果集中的每行数据都封装到Map里，再把这些map再存到一个map里，其key为指定的key。
- MapHandler:将结果集中的第一行数据封装到一个Map里,key是列名，value就是对应的值。
- MapListHandler:将结果集中的每一行数据都封装到一个Map里，然后再存放到List

```java
老韩解读
//(1) query 方法就是执行 sql 语句，得到 resultset ---封装到 --> ArrayList 集合中
//(2) 返回集合
// 以下是参数解释
//(3) connection: 连接
//(4) sql : 执行的 sql 语句
//(5) new BeanListHandler<>(Actor.class): 在将 resultset -> Actor 对象 -> 封装到 ArrayList
// 底层使用反射机制 去获取 Actor 类的属性，然后进行封装
//(6) 1 就是给 sql 语句中的? 赋值，可以有多个值，因为是可变参数 Object... params
//(7) 底层得到的 resultset ,会在 query 关闭, 关闭 PreparedStatment
//以上底层代码你都看过, 不记得就去再看一遍
    
/**
* 分析 queryRunner.query 方法:
* public <T> T query(Connection conn, String sql, ResultSetHandler<T> rsh, Object... params) throws SQLException {
* PreparedStatement stmt = null;//定义 PreparedStatement
* ResultSet rs = null;//接收返回的 ResultSet
* Object result = null;//返回 ArrayList
*
* try {
* 		stmt = this.prepareStatement(conn, sql);//创建 PreparedStatement
* 		this.fillStatement(stmt, params);//对 sql 进行 ? 赋值
* 		rs = this.wrap(stmt.executeQuery());//执行 sql,返回 resultset
* 	result = rsh.handle(rs);//返回的 resultset --> arrayList[result] [使用到反射，对传入 class 对象处理]
* } catch (SQLException var33) {
* 		this.rethrow(var33, sql, params);
* } finally {
* 		try {
* 			this.close(rs);		//关闭 resultset
* 		} finally {
* 			this.close((Statement)stmt);		//关闭 preparedstatement 对象
* 		}
* }
*
* 	return result;
* }
*/
```

**所以**:

- DBUtils实际过程
  - 对得到执行SQL的对象步骤进行了封装
  - 对执行SQL语句的步骤进行封装 
  - 最后将得到的查询集进行封装, 然后处理返回

### 15. BasicDAO管理工具的使用

**问题的引出:**

- apache-dbutils+ Druid简化了JDBC开发，但还有不足:
  1. SQL语句是固定，不能通过参数传入，通用性不好，需要进行改进，更方便执行增删改查
  2. 对于select 操作，如果有返回值,返回类型不能固定，需要使用泛型
  3. 将来的表很多，业务需求复杂,不可能只靠一个Java类完成
  4. 引出=》 BasicDAO画出示意图，看看在实际开发中，应该如何处理

⚠:

1. DAO: data access object  数据访问对象
2. 这样的通用类，称为BasicDao，是专门和数据库交互的，即完成对数据库(表)的crud操作
3. 在BaiscDao的基础上，实现一张表对应一个Dao，更好的完成功能，比如 Customer表-Customer.java类(javabean)-CustomerDao.java

****

![image-20230626084044092](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230626084044092.png)

**总的来说是实现多个javaBean然后将对JavaBean的操作进行封装, 最后再呈现给用户, 程序员写代码更优雅, 解耦合很明显**

### 附件:

powerDesigner的使用

- 模型的建立

![image-20230626235519948](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230626235519948.png)

- 表的命名

![image-20230627000306317](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230627000306317.png)

- 表的建立

![image-20230627000427691](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230627000427691.png)

- 模型的保存

![image-20230627000626482](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230627000626482.png)

- SQL代码的保存

![image-20230627000539869](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230627000539869.png)

- 打开模型和表

![image-20230627001408342](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230627001408342.png)

- 将SQL文件拖动到source处即可运行

![image-20230627001534533](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230627001534533.png)

- 记得SQL文件的编译类型是UTF-8(为了正常编译)

![image-20230627001658736](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230627001658736.png)

- 终端可能和MySQL实际上的显示有区别, 所以要注意但不必担心

![image-20230627001854147](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230627001854147.png)

# 总结:

事务: 为了**全体执行**

悲观锁 : 让一部分信息有**安全机制**

批处理 : **多个**SQL语句**一次性发送**给数据库执行(<u>在连接的时候就说明了</u>)

数据库池 : 在池中放有**一定数的连接好的connection**, 可以直接用, 不够了就排队等着, 解决了并发遇到的问题, **Druid方法**

Apache-DBUtils : 为了让**处理结果集变得更专业**

BasicDAO : 对JavaBean分别进行实现, 并对每个JavaBean的操作进行封装, 这样可以**直接传SQL语句**和**返回想要的数据类型** 

