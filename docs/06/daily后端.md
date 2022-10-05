## 1.JavaSE

### 1.try(){ }catch()的用法

主要是针对进行流操作时，经常忘记关闭流，造成[内存](https://so.csdn.net/so/search?q=内存&spm=1001.2101.3001.7020)溢出的问题而进行的优化。

前提:try(变量)变量从类型上需要实现Closeable,否无法使用。

传统写法

```java
public class Main {
    public static void main(String[] args) {
        InputStream inputStream=null;
        try{
            inputStream=new FileInputStream("aa");
            //对流进行操作
        }catch (Exception e){
            e.printStackTrace();
        }finally {//关闭流
            if(inputStream!=null){
                try {
                    inputStream.close();
                }catch (IOException e){
                    e.printStackTrace();
                }
            }

        }
    }
}

```

JDK1.7新写法

```java
public class Main {
    public static void main(String[] args) {
        //括号里可以声明对象，声明的对象必须实现autoCloseable接口，可以声明多个变量。
        //在括号里声明的对象，无需手动关闭，用完后会自动关闭
        try(InputStream inputStream=new FileInputStream("aa");){
            inputStream.read();//try方法块里可以直接使用括号内定义的变量
        }catch (Exception e){

            e.printStackTrace();
        }
    }
}

```

## 2.MySQL

### 1.数据储存的三个地方

硬盘,内存,cpu寄存器

### 2.MySQL的存储过程(MySQL高级)

- 存储过程

  将能够完成特定功能的SQL指令进行封装(SQL指令集),编译之后存储在数据库服务器上,并且为之去一个名字,客户端可以通过名字直接调用这个SQL指令集,获取执行结果

- 存储过程的基本格式语句

  ```mysql
  1、创建存储过程
  create procedure 存储过程名称(in/out/inout 参数名 参数类型(长度))
  begin
       SQL语句;
       /*
       举例
       -- DECLARE声明 用来声明变量的
  		DECLARE de_name VARCHAR(10) DEFAULT '';
  		
  		SET de_name = "jim";
  		
  		-- 测试输出语句（不同的数据库，测试语句都不太一样。
  		SELECT de_name;
       */
  end;
   
   
  说明：
  in：该类型参数作为输入，也就是需要调用时传入值
  out：该类型参数作为输出，也就是该参数可以作为返回值
  inout：既可以作为输入参数，也可以作为输出参数
  参数类型长度：不指定长度时mysql会默认一个长度，如int会默认int(11)，为什么是11，因为int的有符号类型的最大长度就是-2147483648，是11位的。
   
   
  2、查看存储过程
  select * from information_schema.routines where routine_schema = 'xxx'; --查看指定数据库的存储过程及状态信息
   
  show create procedure 存储过程名字 ; --查看某个存储过程的定义sql语句
   
   
  3、删除
   
  drop procedure [if exists] 存储过程名字;
  ```

-  存储过程中的语句必须包含在BEGIN和END之间。

- DECLARE中用来声明变量，变量默认赋值使用的DEFAULT，语句块中改变变量值，使用SET 变量=值

详细内容:

MySQL高级.pdf

[MySQL存储过程](https://blog.csdn.net/weixin_45970271/article/details/124180709)

## 3.MyBatis

### 1.MyBtis的前身

MyBtis的前身为iBatis,所以在编译器实用mybatis导包过程中我们可以看到有时候到的包名为ibatis.

### 2.MyBatis面试题(关键字ORM)

- 要点
  - mybatis是什么
  - mybatis的功能实现以及技术体系
- 

### 3.注解开发

#### 3.1@Mapper

@Mapper注解，目的就是为了不再写mapper映射文件，是注解开发时用的。

```java
@Mapper
public interface Inter {
    @Insert("insert into sysuser values('e212te','2','jjj','pwd','ljk','男',1)")
    int addUser();
}
```

#### 3.2@Param

1，使用@Param注解

```mysql
当以下面的方式进行写SQL语句时：

  @Select("select column from table where userid = #{userid} ")
   public int selectColumn(int userid);

当你使用了使用@Param注解来声明参数时，如果使用 #{} 或 ${} 的方式都可以。

  @Select("select column from table where userid = ${userid} ")
   public int selectColumn(@Param("userid") int userid);

当你不使用@Param注解来声明参数时，必须使用使用 #{}方式。如果使用 ${} 的方式，会报错。

  @Select("select column from table where userid = ${userid} ")
   public int selectColumn(@Param("userid") int userid);
```

2，不使用@Param注解

不使用@Param注解时，参数只能有一个，并且是Javabean。在SQL语句里可以引用JavaBean的属性，而且只能引用JavaBean的属性。

### 4.MyBatis通配符 (#{ }和#{ })

#{}写法：

```mysql
select * from Student where SID = #{id}

打印的日志内容底层显示：
Preparing: select * from Student where SID = ?
Parameters: 1
#{}写法是在SQL上通过？占位占位，将参数和sql分别传递给数据库，相当于JDBC编程PreateStatement
```

${}写法：

```mysql

select * from Student where SID = ${id}
打印日志问题：
Preparing: select * from Student where SID = 1
Parameters:
${}写法直接将参数拼接到sql语句上，相当于jdbc中的Statement操作
```

使用：
${}会有SQL注入的问题，#{}采用预编译机制先对sql编译，无误后传递给数据库
一般推荐使用#{}写法

## 4.Spring

### 1.跨域问题

在对应的需要跨域的contrcontroller层加上@CrossOrigin
