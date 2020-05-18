# 5. JDBC
## 5.1. JDBC是什么
JDBC(Java Database Connectivity) 是一个独立特定数据库管理系统、通用的SQL数据库存取和操作的公共接口（一组API）。为访问不同的数据提供了一种统一的途径，使用JDBC可以链接任何提供了JDBC驱动程序。

### 5.1.1. JDBC的本质
JDBC是SUN公司制定的一套接口（interface）</br>
在java.sql.*;包下</br>
接口都有调用者和实现者。</br>
面向接口调用、面向接口实现，这都属于面向接口编程。</br>
可以降低程序的耦合度，提高程序的扩展力

因为每一个数据库的底层实现原理都不一样，各数据库的程序员负责编写该接口的实现。各数据库的驱动得去网上下载。这些驱动中有很多.class文件，就是对接口的实现。

### 5.1.2. JDBC开发前的准备工作
从官网下载对应的驱动jar包，然后将其配置到classpath中。用idea不需要配置。idea有自己的方式。

## 5.2. JDBC编程
- 第一步：注册驱动
- 第二步：获取连接（表示JVM的进程和数据库进程之间的通道打开了，属于进程之间的通信，重量级，使用完一定要关闭）
- 第三步：获取数据库操作对象（专门执行sql语句的对象）
- 第四步：执行SQL与（DQL DML）
- 第五步：处理查询结果集（只有第四部是select语句时）
- 第六步：释放资源（使用完资源之后一定要关闭资源。java和数据库属于进程间的通信，开启之后一定要关闭）

```java
import java.sql.*;

public class JDBCTest01 {
    public static void main(String[] args) {
        Connection conn=null;
        Statement stmt=null;
        try {
            // 1.注册驱动
            // Class.forName(...);可以加载这个类
            // 这个类中有一个静态代码块，是java.sql.DriverManager.registerDriver(new Driver());
            // 当一个类被加载，静态代码块自动执行
            Class.forName("com.mysql.jdbc.Driver");
            // 2.获取连接
            String url = "jdbc:mysql://localhost:3306/wyj";
            String user = "root";
            String password = "12345678";
            conn = DriverManager.getConnection(url, user, password);
            // 3.获取数据库操作对象
            stmt = conn.createStatement();
            // 4.执行sql
            // jdbc的sql语句不需要提供分号
            String sql = "insert into dept (deptno,dname,loc) values (50,'人事部','北京')";
            // 专门执行DML语句（增删改）
            // 返回值是影响数据库中的记录条数
            int count = stmt.executeUpdate(sql);
            System.out.println(count==1?"保存成功":"保存失败"); //因为insert只影响一条记录

            // 5.处理查询结果集

        }catch (Exception e) {
            e.printStackTrace();
        }finally{
            // 6.释放资源
            // 为了保证资源一定释放，在finally中释放
            // 并且要从小到大释放 必须分开try
            if(stmt!=null){
                try {
                    stmt.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if(conn!=null){
                try {
                    conn.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

### 5.2.1. ResultSetMetaData
可用于获取关于 ResultSet 对象中列的类型和属性信息的对象

- ResultSetMetaData meta = rs.getMetaData();
- getColumnName(int column)：获取指定列的名称
- getColumnLabel(int column)：获取指定列的别名
- getColumnCount()：返回当前 ResultSet 对象中的列数。
- getColumnTypeName(int column)：检索指定列的数据库特定的类型名称。
- getColumnDisplaySize(int column)：指示指定列的最大标准宽度，以字符为单位。
- isNullable(int column)：指示指定列中的值是否可以为 null。
- isAutoIncrement(int column)：指示是否自动为指定列进行编号，这样这些列仍然是只读的。

### 5.2.2. 将连接数据库的所有信息配置到配置文件中
实际开发中，不建议把链接数据库的信息写死到java程序中

```
// JDBC.properties
driver=com.mysql.jdbc.Driver
url=jdbc:mysql://localhost:3306/wyj
user=root
password=12345678
```

```java
import java.sql.*;
import java.util.ResourceBundle;

public class JDBCTest01 {
    public static void main(String[] args) {
        ResourceBundle bundle = ResourceBundle.getBundle("JDBC");
        String driver = bundle.getString("driver");
        String user = bundle.getString("user");
        String password = bundle.getString("password");
        String url = bundle.getString("url");
        Connection conn=null;
        Statement stmt=null;
        ResultSet rs = null;
        try {
            // 1.注册驱动
            // Class.forName(...);可以加载这个类
            // 这个类中有一个静态代码块，是java.sql.DriverManager.registerDriver(new Driver());
            // 当一个类被加载，静态代码块自动执行
            Class.forName(driver);
            // 2.获取连接
            conn = DriverManager.getConnection(url, user, password);
            // 3.获取数据库操作对象
            stmt = conn.createStatement();
            // 4.执行sql
            // jdbc的sql语句不需要提供分号
            //String sql = "insert into dept (deptno,dname,loc) values (40,'研发部','北京')";
            String sql = "select * from dept";
            // 专门执行DML语句（增删改）
            // 返回值是影响数据库中的记录条数
            // 增删改语句
            //int count = stmt.executeUpdate(sql);
            //System.out.println(count==1?"保存成功":"保存失败"); //因为insert只影响一条记录

            //查询语句 返回结果集
            rs = stmt.executeQuery(sql);

            // 5.处理查询结果集
            // 只有查询语句才会需要这个步骤
            while(rs.next()){
                // JDBC下标从1开始
                // 参数填的应该是查询结果的列名，如果查询的时候把deptno重命名为deptid，下面就要用deptid
                // 可以都用String 或者 其指定的类型
                String deptno = rs.getString("deptno");
                String dname = rs.getString("dname");
                String loc = rs.getString("loc");
            }


        }catch (Exception e) {
            e.printStackTrace();
        }finally{
            // 6.释放资源
            // 为了保证资源一定释放，在finally中释放
            // 并且要从小到大释放 必须分开try
            if(rs!=null){
                try {
                    rs.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if(stmt!=null){
                try {
                    stmt.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if(conn!=null){
                try {
                    conn.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

## 5.3. SQL注入现象
一个账号密码登录系统，
```java
String sql = "select * from t_user where loginName='" + loginName + "' and loginPwd='" + loginPwd + "'";
```

当输入为：
```
账号：fdsa
密码：fadaef' or '1'='1
```
时会登录成功，然而并没有这个账户

### 5.3.1. 原因
这样输入之后 sql语句会变成：
`select * from t_user where loginName = 'fdsa' and loginPwd='fadaef' or '1'='1’`
这里1=1恒成立。会搜索出所有的用户记录

用户输入的信息中含有sql语句的关键字，并且这些关键字参与sql语句的编译过程。导致mysql有语句被扭曲。

### 5.3.2. 解决
只要用户提供的信息不参与sql语句的编译过程就可以解决问题。

把Statement换成PreparedStatement。这个接口继承了Statement接口。</br>
他是预编译的sql库操作对象

sql语句用？当做占位符。编译后在设置值。

原理：预先对SQL语句的框架进行编译，然后再给SQL语句传值。
```java
import java.sql.*;
import java.util.ResourceBundle;

public class JDBCTest01 {
    public static void main(String[] args) {
        ResourceBundle bundle = ResourceBundle.getBundle("JDBC");
        String driver = bundle.getString("driver");
        String user = bundle.getString("user");
        String password = bundle.getString("password");
        String url = bundle.getString("url");
        Connection conn=null;
        PreparedStatement ps=null;
        ResultSet rs = null;
        try {
            // 1.注册驱动
            // Class.forName(...);可以加载这个类
            // 这个类中有一个静态代码块，是java.sql.DriverManager.registerDriver(new Driver());
            // 当一个类被加载，静态代码块自动执行
            Class.forName(driver);
            // 2.获取连接
            conn = DriverManager.getConnection(url, user, password);
            // 3.获取预编译的数据库操作对象
            // 一个？表示一个占位符，接收一个数值
            String sql = "select * from dept where deptno=?";
            ps = conn.prepareStatement(sql);
            // 给占位符传值
            // 给第一个占位符赋值为"20"
            ps.setString(1,"20");
            // 4.执行sql
            rs = ps.executeQuery(sql);

            // 5.处理查询结果集
            // 只有查询语句才会需要这个步骤
            while(rs.next()){
                // JDBC下标从1开始
                // 参数填的应该是查询结果的列名，如果查询的时候把deptno重命名为deptid，下面就要用deptid
                // 可以都用String 或者 其指定的类型
                String deptno = rs.getString("deptno");
                String dname = rs.getString("dname");
                String loc = rs.getString("loc");
            }


        }catch (Exception e) {
            e.printStackTrace();
        }finally{
            // 6.释放资源
            // 为了保证资源一定释放，在finally中释放
            // 并且要从小到大释放 必须分开try
            if(rs!=null){
                try {
                    rs.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if(ps!=null){
                try {
                    ps.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if(conn!=null){
                try {
                    conn.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}

```

### 5.3.3. 特点
`preparedStatement`编译一次可以执行n次，会在编译阶段做类型的安全检查</br>
`Statement`编译一次使用一次，效率低。

大部分情况都是用`preparedStatement`。当业务方面必须支持sql注入时，可以使用`Statement`。

比如淘宝中的价格升序降序，需要在sql语句最后输入`order by xxxx asc` 或 `order by xxx desc`，这里的asc和desc就需要是`Statement`。

## 5.4. JDBC的事务
**JDBC中的事务是自动提交的**，只要执行任意一条DML语句，则自动提交一次。</br>
这是JDBC默认的事务行为。</br>
但是在实际的业务当中，通常都是N条DML语句共同联合才能完成的。必须保证他们这些DML语句在同一个事务中同时成功或者失败。

### 5.4.1. JDBC单机事务操作步骤
1. 设置主动提交 `conn.setAutoCommit(false);`
2. 手动提交 `conn.commit();`
3. 若失败，回滚事务 `conn.rollback();`

```java
import java.sql.*;
import java.util.ResourceBundle;

public class JDBCTest02 {
    public static void main(String[] args) {
        ResourceBundle bundle = ResourceBundle.getBundle("JDBC");
        String driver = bundle.getString("driver");
        String user = bundle.getString("user");
        String password = bundle.getString("password");
        String url = bundle.getString("url");
        Connection conn=null;
        PreparedStatement ps=null;
        ResultSet rs = null;
        try {
            // 1.注册驱动
            // Class.forName(...);可以加载这个类
            // 这个类中有一个静态代码块，是java.sql.DriverManager.registerDriver(new Driver());
            // 当一个类被加载，静态代码块自动执行
            Class.forName(driver);
            // 2.获取连接
            conn = DriverManager.getConnection(url, user, password);
            // 将自动提交设置为主动提交
            conn.setAutoCommit(false);
            // 3.获取预编译的数据库操作对象
            String sql = "update t_act set balance = ? where actno = ?";
            ps = conn.prepareStatement(sql);
            ps.setDouble(1,10000);
            ps.setInt(2,111);
            int count = ps.executeUpdate();

            ps.setDouble(1,10000);
            ps.setInt(2,222);
            count += ps.executeUpdate();

            // 手动提交事务
            conn.commit();
        }catch (Exception e) {
            // 如果失败 手动回滚事务
            // 因为如果是手动commit了，正确的语句还是会插入，错误的就会报错，需要手动回滚！！
            if(conn!=null){
                try {
                    conn.rollback();
                } catch (SQLException ex) {
                    ex.printStackTrace();
                }
            }
            e.printStackTrace();
        }finally{

            // 6.释放资源
            // 为了保证资源一定释放，在finally中释放
            // 并且要从小到大释放 必须分开try
            if(rs!=null){
                try {
                    rs.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if(ps!=null){
                try {
                    ps.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if(conn!=null){
                try {
                    conn.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

## 5.5. JDBC工具类的封装
```java
package utils;

import java.sql.*;
import java.util.ResourceBundle;

public class DBUtil {
    /**
     * 工具类中的构造方法都是私有的
     * 因为工具类当中的方法都是惊天的，不需要new对象，直接采用类名调用
     */
    private DBUtil(){
    }

    // 静态代码块在类加载时执行，并且只执行一次
    static{
        try {
            Class.forName("com.mysql.jdbc.Driver");
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
    }

    /**
     * 获取数据库连接对象
     * @return 返回连接对象
     * @throws SQLException
     */
    public static Connection getConnection() throws SQLException {
        ResourceBundle bundle = ResourceBundle.getBundle("JDBC");
        String driver = bundle.getString("driver");
        String user = bundle.getString("user");
        String password = bundle.getString("password");
        String url = bundle.getString("url");
        return DriverManager.getConnection(url, user, password);
    }

    /**
     * 关闭资源
     * @param conn 连接对象
     * @param ps 连接数据库操作对象
     * @param rs 结果集
     */
    public static void close(Connection conn, Statement ps, ResultSet rs){
        if(rs!=null){
            try {
                rs.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
        if(ps!=null){
            try {
                ps.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
        if(conn!=null){
            try {
                conn.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }
}
```

```java
import utils.DBUtil;

import java.sql.*;

public class JDBCTest02 {
    public static void main(String[] args) {
        Connection conn=null;
        PreparedStatement ps=null;
        ResultSet rs = null;
        try {
            // 1.注册驱动
            // 2.获取连接
            conn = DBUtil.getConnection();
            // 3.获取预编译的数据库操作对象
            String sql = "update t_act set balance = ? where actno = ?";
            ps = conn.prepareStatement(sql);
            ps.setDouble(1,10000);
            ps.setInt(2,111);
            int count = ps.executeUpdate();

        }catch (Exception e) {
            e.printStackTrace();
        }finally{
            // 6.释放资源
            DBUtil.close(conn, ps, rs);
        }
    }
}
```

## 5.6. 行级锁（悲观锁）
`select * where job='manager' for update;`
在最后加了一个`for update`，查询到的几条记录就被锁住了，在当前事务未结束前无法进行修改。

## 5.7. 乐观锁
每条记录有一个版本号，可以多线程并发，也不需要排队。</br>
比如事务1先修改了，并把版本号由1改成了2。当事务2来修改时，发现版本号是2了，已经别人改过了，就回滚。

## 5.8. DAO设计模式
Data Access Object

是什么：是访问数据信息的类，包含了对数据的CRUD（增删改查）。而不包含任何业务相关的信息。

为什么这么做：实现功能的模块化。更有利于代码的维护和商集。

### 5.8.1. JavaBean
POJO是一个简单的、普通Java对象，它包含业务逻辑处理或持久化逻辑等

```java
public class car {
	/**
	 * 这是一个五座小汽车
	 */
	
	private int 车轮 = 4 ;
	private int 方向盘 = 1;
	private int 座位 = 5;
	
	
	public int get车轮() {
		return 车轮;
	}
	public void set车轮(int 车轮) {
		this.车轮 = 车轮;
	}
	public int get方向盘() {
		return 方向盘;
	}
	public void set方向盘(int 方向盘) {
		this.方向盘 = 方向盘;
	}
	public int get座位() {
		return 座位;
	}
	public void set座位(int 座位) {
		this.座位 = 座位;
	}
	
}
```
一开始学习的java的时候，我们把上述代码称之为一个对象类，而到了后期，我们称之为一个javaBean。因为后期java为了便于操作数据，通常是使用对象为容器，把需要操作的数据赋值给对象，而为了便于赋值，那我们必须要有这种get/set方法。

总结如下：
1、所有属性为private
2、提供默认构造方法
3、提供getter和setter
4、实现serializable接口

### 5.8.2. 如何做
使用JDBC编写DAO可能会包含的方法。

JavaBeans是Java中一种特殊的类，可以将多个对象封装到一个对象（bean）中。特点是可序列化，提供无参构造器，提供getter方法和setter方法访问对象的属性。名称中的“Bean”是用于Java的可重用软件组件的惯用叫法。


```java
// 增删改
void update();
// 查询一条记录
<T> T get(Class<T> class,  String sql, Object ... args){
    T entity = null;
    Connection connection = null;
    PreparedStatement ps = null;
    ResultSet ts = null;
    try{
        // 1.获取conncetion
        connection = JBUtils.getConnection();
        // 2.获取PreparedStatemtnt
        ps = connection.prepareStatement(sql);
        // 3.填充占位符
        for(int i=0; i < args.length; i++){
            ps.setObject(i+1,args[i]);
        }
        // 4.进行查询获得ResultSet
        rs = ps.executeQuery();
        // 5.若有记录，准备一个Map<String,Object>
        // 键存放列的别名，值存放列的值
        if(rs.next()){
            Map<String, Object> values=new HashMap<String,Object>();
            //6. 得到ResultSetMetaData对象
            ResultSetMetaData rsmd = rs.getMetaData();
            //7. 处理ResultSet, 把指针向下移动
            //8. 由ResultSetMetaData对象得到结果集中有多少列
            int columnCount = rsmd.getColumnCount();
            //9. 由ResultSetMetaData对象得到每一列的别名
            for(int i=0;i<columnCount;i++){
                String columnLabel=rsmd.getColumnLabel(i+1);
                String columnValue=rs.getObject(i+1);
                //10. 填充map对象
                values.put(columnLabel, columnValue);
            }
            //11. 用反射创建Class对应的对象
            entity=class.newInstance();
            //12. 遍历Map对象，用反射填充对象的属性名：
            // 属性名为Map中的key，属性值为value
            // 这个是推荐的Map遍历方法
            for(Map.Entry<String,Object> entry:values.entrySet()){
                String propertyName = entry.getKey();
                Object value = entry.getValue();

                setFieldValue(entity, propertyName, value); // 自定义方法，用于给对象赋值
            } 
        }
    }catch(Exception e){
        e.printStackTrace();
    }finally{
        JBUtlis.close(conn,ps,rs);
    }
    return entity;
}
// 查询多条记录
<T> List<T> getForList(Class<T>, String sql, Object ... args){};
// 返回某条记录的某个字段的值
<E> E getForValue(String sql, Object ... args);

//调用
dao.get(Student.class, sql, ...);
```

更好的包装成bean
```java
/**
	 * 获取一个对象
	 * 
	 * @param sql
	 * @param params
	 * @return
	 */
	public T getBean(Connection conn,String sql, Object... params) {
		T t = null;
		try {
			t = queryRunner.query(conn, sql, new BeanHandler<T>(type), params);
		} catch (SQLException e) {
			e.printStackTrace();
		} 
		return t;
	}

@Override
	public List<Book> getBooks(Connection conn) {
		// 调用BaseDao中得到一个List的方法
		List<Book> beanList = null;
		// 写sql语句
		String sql = "select id,title,author,price,sales,stock,img_path imgPath from books";
		beanList = getBeanList(conn,sql);
		return beanList;
	}
```

## 5.9. 批量插入
### 5.9.1. 批量执行SQL语句
当需要成批插入或者更新记录时，可以采用Java的批量更新机制，这一机制允许多条语句一次性提交给数据库批量处理。通常情况下比单独提交处理更有效率

JDBC的批量处理语句包括下面三个方法：

addBatch(String)：添加需要批量处理的SQL语句或是参数；
executeBatch()：执行批量处理语句；
clearBatch():清空缓存的数据
通常我们会遇到两种批量执行SQL语句的情况：

多条SQL语句的批量处理；
一个SQL语句的批量传参；
### 5.9.2. 高效的批量插入
举例：向数据表中插入20000条数据

数据库中提供一个goods表。创建如下：
```sql
CREATE TABLE goods(
id INT PRIMARY KEY AUTO_INCREMENT,
NAME VARCHAR(20)
);
```

### 5.9.3. 实现层次一：使用Statement
```java
Connection conn = JDBCUtils.getConnection();
Statement st = conn.createStatement();
for(int i = 1;i <= 20000;i++){
	String sql = "insert into goods(name) values('name_' + "+ i +")";
	st.executeUpdate(sql);
}
5.2.2 实现层次二：使用PreparedStatement
long start = System.currentTimeMillis();
		
Connection conn = JDBCUtils.getConnection();
		
String sql = "insert into goods(name)values(?)";
PreparedStatement ps = conn.prepareStatement(sql);
for(int i = 1;i <= 20000;i++){
	ps.setString(1, "name_" + i);
	ps.executeUpdate();
}
		
long end = System.currentTimeMillis();
System.out.println("花费的时间为：" + (end - start));//82340
		
		
JDBCUtils.closeResource(conn, ps);
```

### 5.9.4. 实现层次三
```java
/*
 * 修改1： 使用 addBatch() / executeBatch() / clearBatch()
 * 修改2：mysql服务器默认是关闭批处理的，我们需要通过一个参数，让mysql开启批处理的支持。
 * 		 ?rewriteBatchedStatements=true 写在配置文件的url后面
 * 修改3：使用更新的mysql 驱动：mysql-connector-java-5.1.37-bin.jar
 * 
 */
@Test
public void testInsert1() throws Exception{
	long start = System.currentTimeMillis();
		
	Connection conn = JDBCUtils.getConnection();
		
	String sql = "insert into goods(name)values(?)";
	PreparedStatement ps = conn.prepareStatement(sql);
		
	for(int i = 1;i <= 1000000;i++){
		ps.setString(1, "name_" + i);
			
		//1.“攒”sql
		ps.addBatch();
		if(i % 500 == 0){
			//2.执行
			ps.executeBatch();
			//3.清空
			ps.clearBatch();
		}
	}
		
	long end = System.currentTimeMillis();
	System.out.println("花费的时间为：" + (end - start));//20000条：625                                                                         //1000000条:14733  
		
	JDBCUtils.closeResource(conn, ps);
}
```

### 5.9.5. 实现层次四
```java
/*
* 层次四：在层次三的基础上操作
* 使用Connection 的 setAutoCommit(false)  /  commit()
*/
@Test
public void testInsert2() throws Exception{
	long start = System.currentTimeMillis();
		
	Connection conn = JDBCUtils.getConnection();
		
	//1.设置为不自动提交数据
	conn.setAutoCommit(false);
		
	String sql = "insert into goods(name)values(?)";
	PreparedStatement ps = conn.prepareStatement(sql);
		
	for(int i = 1;i <= 1000000;i++){
		ps.setString(1, "name_" + i);
			
		//1.“攒”sql
		ps.addBatch();
			
		if(i % 500 == 0){
			//2.执行
			ps.executeBatch();
			//3.清空
			ps.clearBatch();
		}
	}
		
	//2.提交数据
	conn.commit();
		
	long end = System.currentTimeMillis();
	System.out.println("花费的时间为：" + (end - start));//1000000条:4978 
		
	JDBCUtils.closeResource(conn, ps);
}
```

## 5.10. 数据库连接池
### 5.10.1. JDBC数据库连接池的必要性
在使用开发基于数据库的web程序时，传统的模式基本是按以下步骤：　　

- 在主程序（如servlet、beans）中建立数据库连接
    - 进行sql操作
    - 断开数据库连接
-  这种模式开发，存在的问题:
   - 普通的JDBC数据库连接使用 DriverManager 来获取，每次向数据库建立连接的时候都要将 Connection 加载到内存中，再验证用户名和密码(得花费0.05s～1s的时间)。需要数据库连接的时候，就向数据库要求一个，执行完成后再断开连接。这样的方式将会消耗大量的资源和时间。**数据库的连接资源并没有得到很好的重复利用。**若同时有几百人甚至几千人在线，频繁的进行数据库连接操作将占用很多的系统资源，严重的甚至会造成服务器的崩溃。
    - **对于每一次数据库连接，使用完后都得断开。**否则，如果程序出现异常而未能关闭，将会导致数据库系统中的内存泄漏，最终将导致重启数据库。（回忆：何为Java的内存泄漏？）
    - 这种开发不能控制被创建的连接对象数，系统资源会被毫无顾及的分配出去，如连接过多，也可能导致内存泄漏，服务器崩溃。

数据库连接池的基本思想就是为数据库连接建立一个“缓冲池”，预先在缓冲池中放置一定数量的链接，当需要建立数据库连接时，只需要从缓冲池中取出一个，使用完毕之后再放回去。</br>
数据可连接池负责分配、管理和释放数据库连接，它允许应用程序重复使用一个现有的数据库连接，而不是重新建立一个。

优点：
- 资源重用
- 更快的系统反应速度
- 新的资源分配手段
- 统一的连接管理，避免数据库连接泄露

JDBC的数据库连接使用javax.sql.DataSource接口来表示。常用的开源代码由：DBCP, C3P0。

### 5.10.2. 使用DBCP数据库连接池
```java
/**
1. 加入jar包，依赖于 commons pool
2. 创建数据库连接池
3. 指定属性
4. 获取连接
*/
public void testDBCP() throws SQLException{
    BasicDataSource dataSource = null;
    // 1. 创建DBPC数据源实例
    dataSource = new BasicDataSource();

    // 2. 为数据源实例指定必须的属性
    dataSource.setUsername("root");
    dataSource.setPassword("12345678");
    dataSource.setUrl("jdbc:mysql://localhost:3306/wyj");
    dataSOurce.setDriverClassName("com.mysql.jdbc.Driver");

    //3. 指定可选的属性
    //3.1 指定初始化连接个数
    dataSource.setInitialSize(10);
    //3.2 指定同一时刻最大连接数
    dataSource.setMaxActive(50);
    //3.3 指定最小连接数,连接池中保存的最少的空闲连接数量
    dataSource.setMinIdle(5);
    //3.4 最长的等待时间，单位为毫秒，超过将抛出异常
    dataSource.setMaxWait(1000 * 5);

    // 4. 从数据源中获取数据库连接
    Connection conn = dataSource.getConnection();
}
```

### 5.10.3. 通过工厂模式创建连接池
```
// dbcp.properties
username = root
password = 12345678
url = jdbc:mysql://localhost:3306/wyj

initialSize = 10
maxActive = 50
minIdle = 5
maxWait = 1000*5
```

```java
Properties properties = new Properties();
InputStream inStream = JDBCTeat.class.getClassLoader().getResourceAsStream("dbcp.properties");
properties.load(inStream);

DataSource dataSource = BasicDataSourceFactory.createDataSource(properties);
```

## 5.11. JDBC总结
```java
public void testUpdateWithTx() {
		
	Connection conn = null;
	try {
		//1.获取连接的操作（
		//① 手写的连接：JDBCUtils.getConnection();
		//② 使用数据库连接池：C3P0;DBCP;Druid
		//2.对数据表进行一系列CRUD操作
		//① 使用PreparedStatement实现通用的增删改、查询操作（version 1.0 \ version 2.0)
//version2.0的增删改public void update(Connection conn,String sql,Object ... args){}
//version2.0的查询 public <T> T getInstance(Connection conn,Class<T> clazz,String sql,Object ... args){}
		//② 使用dbutils提供的jar包中提供的QueryRunner类
			
		//提交数据
		conn.commit();
			
	
	} catch (Exception e) {
		e.printStackTrace();
			
			
		try {
			//回滚数据
			conn.rollback();
		} catch (SQLException e1) {
			e1.printStackTrace();
		}
			
	}finally{
		//3.关闭连接等操作
		//① JDBCUtils.closeResource();
		//② 使用dbutils提供的jar包中提供的DbUtils类提供了关闭的相关操作
			
	}
}
```
