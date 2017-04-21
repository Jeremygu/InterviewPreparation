## 十二、 JDBC

### 问：什么是JDBC？

JDBC是一套面向对象的应用程序接口(API)，制定了访问各类关系数据库的统一标准接口，为访问各个数据库厂商的关系数据库提供了标准的实现，它由一组用Java语言编写的类和接口组成。JDBC允许开发者用JAVA写数据库应用程序，而不需要关心底层特定数据库的细节。

### 问：JDBC访问数据库的步骤
	
	//1. 加载JDBC驱动
	Class.forName("oracle.jdbc.driver.OracleDriver");

	//2. 连接Oracle数据库
	String url="jdbc:oracle:thin:@localhost:1521:DataBaseName";
	String username="admin";
	String password="root";
	Connection con=DriverManager.openConnection(url, username, password);

	//3. 利用JDBC检索出表中的数据
	PreparedStatment ps=Con.preparedStatment("select * from users;");
	ResultSet rs=ps.executeQuery();
	While(rs.next){
		Rs.getString(1); //或rs.getString("name")
	}
	
	//4. 关闭数据库连接
	con.close();

#### 问：Class.forName()方法有什么作用？

通过反射机制初始化参数指定的类，并且返回此类对应的Class对象。这个方法可以用来载入并注册跟数据库建立连接的驱动。

#### 问：解释下驱动(Driver)在JDBC中的角色。

JDBC驱动提供了特定厂商对JDBC API接口类的实现，驱动必须要提供java.sql包下面这些类的实现：Connection, Statement, PreparedStatement,CallableStatement, ResultSet和Driver。

加载驱动的几种方法：
> (1) 调用Class.forName()方法：Class.forName("com.microsoft.sqlserver.jdbc.SQLServerDriver");
> 
> (2) 通过添加jdbc.drivers系统属性：DriverManager.registerDriver(new com.mysql.jdbc.Driver());
> 
> (3) 通过registerDriver方法注册：System.setProperty("jdbc.drivers", "com.mysql.jdbc.Driver");

#### 问：PreparedStatement比Statement有什么优势？

(1) PreparedStatement接口继承Statement，PreparedStatement实例包含预编译的SQL语句，执行速度要快于Statement对象，因此，性能会更好。

(2) PreparedStatements是预编译的，可以避免不良用户直接敲sql语句产生sql注入攻击，安全性更强。

(3) 对于不同的查询参数值，PreparedStatement可以重用。

#### 问：什么时候使用CallableStatement？用来准备CallableStatement的方法是什么？

CallableStatement用来执行存储过程。存储过程是由数据库存储和提供的。存储过程可以接受输入参数，也可以有返回结果。如果有输出参数要注册说明是输出参数。非常鼓励使用存储过程，因为它提供了安全性和模块化。

准备一个CallableStatement的方法是：Connection.prepareCall()

#### 问：数据库连接池是什么意思？

像打开关闭数据库连接这种和数据库的交互可能是很费时的，尤其是当客户端数量增加的时候，会消耗大量的资源，成本是非常高的。

可以在应用服务器启动的时候建立很多个数据库连接并维护在一个池中。连接请求由池中的连接提供服务。在连接使用完毕以后，把连接归还到池中，以用于满足将来更多的请求。














1、JDBC如何做事务处理？

	Con.setAutoCommit(false)
	Con.commit();
	Con.rollback();
	Con.setAutoCommit(true)

6、执行存储过程用那一个类，如何操作输出参数?(操作)	

	CallableStatement c=con. prepareCall ("{call getCustomerName(?,?)}");
	c.setString(1,"1");
	c.registerOutParameter(2,java.sql.Types.VARCHAR);
	c.execute();
	c.getString(2);

10、Jdo是什么?	

JDO是Java对象持久化的新的规范，为java data object的简称,也是一个用于存取某种数据仓库中的对象的标准化API。JDO提供了透明的对象存储，因此对开发人员来说，存储数据对象完全不需要额外的代码（如JDBC API的使用）。这些繁琐的例行工作已经转移到JDO产品提供商身上，使开发人员解脱出来，从而集中时间和精力在业务逻辑上。另外，JDO很灵活，因为它可以在任何数据底层上运行。JDBC只是面向关系数据库（RDBMS)JDO更通用，提供到任何数据底层的存储功能，比如关系数据库、文件、XML以及对象数据库（ODBMS）等等，使得应用可移植性更强。(o/rMapping工具 集合处理)

11、在ORACLE大数据量下的分页解决方法。一般用截取ID方法，还有是三层嵌套方法	

	create or replace package myPack
	is
		type c_type is ref cursor;
		procedure getPage(v_sql varchar2,pageSize number,pageIndex number,c out c_type);
	end;
	
	create or replace  package  body myPack
	is
		procedure getPage(v_sql varchar2,pageSize number,pageIndex number,c out c_type)
	  is
	    pageTotal int:=0;
	    pageFirstRow int:=0;
	    pageLastRow int:=0;
	    rowTotal int:=0;
	  begin
	    execute immediate 'select count(*)  from ('||v_sql||')' into rowTotal;
	    pageTotal:=ceil(rowTotal/pageSize);
	    if(pageIndex<1) then
	           raise_application_error(-20001,'页数不能小于1');
	    end if;
	    if(pageIndex>pageTotal) then
	           raise_application_error(-20001,'页数太大，不能读取');
	    end if;
	    pageFirstRow:=(pageIndex-1)*pageIndex+1;
	    pageLastRow:=pageFirstRow+pageSize;
	    open c for ' select * from '||v_sql||' where rownum<'||
	         pageLastRow||'minus select * from '||v_sql
	         ||' where rownum<'||pageFirstRow;    
	  end;
	end;
