# 1.ArrayList

* 集合和数组的区别

    共同点:都是储存数据的容器(ArrayList底层其实使用数组实现的)

    不同点:数组的容量是固定的,集合的容量是可变的

## 1.1Arraylist的常用方法

* ArrayList<E> <E>:是一种特殊的数据类型,泛型.   <>泛型:对集合容器储存的数据类型进行限制

* 示例代码

```java
public class ArrayListDemo02 {
    public static void main(String[] args) {
        //创建集合
        ArrayList<String> array = new ArrayList<String>();

        //添加元素
        array.add("hello");
        array.add("world");
        array.add("java");

        //public boolean remove(Object o)：删除指定的元素，返回删除是否成功
//        System.out.println(array.remove("world"));
//        System.out.println(array.remove("javaee"));

        //public E remove(int index)：删除指定索引处的元素，返回被删除的元素
//        System.out.println(array.remove(1));

        //IndexOutOfBoundsException
//        System.out.println(array.remove(3));

        //public E set(int index,E element)：修改指定索引处的元素，返回被修改的元素
//        System.out.println(array.set(1,"javaee"));

        //IndexOutOfBoundsException
//        System.out.println(array.set(3,"javaee"));

        //public E get(int index)：返回指定索引处的元素
//        System.out.println(array.get(0));
//        System.out.println(array.get(1));
//        System.out.println(array.get(2));
        //System.out.println(array.get(3)); //索引越界异常

        //public int size()：返回集合中的元素的个数
        System.out.println(array.size());

        //输出集合
        System.out.println("array:" + array);
    }
}
```

## 1.2ArrayLsit遍历字符串删除指定字符串

### 1.2.1普通写法

```java
package itheima.arraylist;

import java.util.ArrayList;

public class Demo06ArrayList {
    public static void main(String[] args) {
        //创建集合对象
        ArrayList<String> list = new ArrayList();
        //添加集合元素
        list.add("123");
        list.add("123");
        list.add("123");
        list.add("124");
        //使用for循环遍历元素将相同的元素都删除
        for (int i = 0; i < list.size(); i++) {
            /*String s=list.get(i);
            if(s.equals("123")){
                list.remove(i);
                //结果:会留下一个"123" 原因:集合在删除元素时,集合元素会自动向前挪动,因此会造成漏元素
                //解决方法
                i--;
            }*/
            //标准写法
            String s=list.get(i);
            //推荐使用常量比较变量,为了预防空指针变量报错
            if("123".equals(s)){
                list.remove(i);
                //结果:会留下一个"123" 原因:集合在删除元素时,集合元素会自动向前挪动以为,会造成漏元素
                //解决方法
                i--;
            }
        }
        for (int i = 0; i < list.size(); i++) {
            System.out.println(list.get(i));
        }
    }
}
```
### 1.2.2迭代器改造
```java
package itheima.collection;

import java.util.ArrayList;
import java.util.Collection;
import java.util.Iterator;

public class Demo04Iterator {
    public static void main(String[] args) {
        ArrayList<String> list=new ArrayList<>();
        list.add("1");
        list.add("2");
        list.add("3");
        list.add("3");
        list.add("5");

        //获取迭代器对象
        Iterator<String> iterator = list.iterator();
        //每一个元素都判断 迭代器遍历集合
        while (iterator.hasNext()){
            String s= iterator.next();
            if ("3".equals(s)){
                iterator.remove();
            }
        }
        System.out.println(list);
    }
}

```

# 2.学生管理系统

1. 定义学生类

```java
public class Student {
    private String sid;
    private String name;
    private int age;
    private String birthday;

    public String getSid() {
        return sid;
    }

    public void setSid(String sid) {
        this.sid = sid;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public String getBirthday() {
        return birthday;
    }

    public void setBirthday(String birthday) {
        this.birthday = birthday;
    }

    public Student() {
    }

    public Student(String sid, String name, int age, String birthday) {
        this.sid = sid;
        this.name = name;
        this.age = age;
        this.birthday = birthday;
    }
}

```

2. 专司控制台的相关事宜的
```java
import java.util.Scanner;

/**
 * 专司控制台的相关事宜的
 */
public class Console {

    private final Scanner scanner = new Scanner(System.in);

    public void showMenu() {
        System.out.println("**********主菜单**********");
        System.out.println("# 1 显示学员列表信息");
        System.out.println("# 2 录入学员信息");
        System.out.println("# 3 删除学员信息");
        System.out.println("# 4 修改学员信息");
        System.out.println("# 0 退出系统");
    }

    public int readNumber(String prompt) {
        System.out.print(prompt + ":");
        return scanner.nextInt();
    }

    public String readString(String prompt) {
        System.out.print(prompt + ":");
        return scanner.next();
    }



    public void clear() {
        for (int i = 0; i < 3; i++) System.out.println();
    }

}
```

3. 学生管理系统功能实现
```java
package itheima.arraylist.project;

import itheima.arraylist.project.domain.Student;

import java.util.ArrayList;
import java.util.Scanner;

public class StudentManager {
    //创建集合容器对象
    private ArrayList<Student> list = new ArrayList<>();
    private Console console=new Console();

    public void run(){
        Scanner sc = new Scanner(System.in);

        //1.搭建主界面菜单  循环标号lo
        lo:while(true){
            System.out.println("-----欢迎来到学生管理系统-----");
            System.out.println("1. 添加学生");
            System.out.println("2. 删除学生");
            System.out.println("3. 修改学生");
            System.out.println("4. 查看学生");
            System.out.println("5. 退出系统");
            System.out.println("请输入选项进行操作");
            int choice=sc.nextInt();
            switch (choice){
                case 1:addstu();break;
                case 2:del();break;
                case 3:change();break;
                case 4:show();break;
                case 5:
                    System.out.println("感谢您的使用");
                    break lo;
                default:
                    System.out.println("您输入的选项错误,请重新输入");
                    break;
            }
        }
    }

    private void show() {
        //1.判断集合中是否存在数据,如果不存在直接给出提示
        if(list.size()==0){
            System.out.println("无信息,请重新添加查询");
            return ;
        }
        //有头
        showTitle();
        for (Student s:list) {
            System.out.printf("[# %-10s]\t%-5s\t%2d\t%6s\r\n", s.getSid(), s.getName(), s.getAge(), s.getBirthday());
        }
        //有尾 生成一些常见的用户想看到的统计信息
        showTotal();

    }

    private void showTotal() {
        System.out.println("----------------------------------------");
        System.out.println("Total:"+list.size());
    }

    private void showTitle() {
        System.out.println("sId\t\t\t\tName\tAge\t\tbirthday");
        System.out.println("----------------------------------------");
    }

    private void change() {
        //1.给出提示信息 //2.键盘接收要删除的学号
        String changeSid=console.readString("请输入你要修改的学生的学号");
        //3.调用getIndex方法,查找该学号在集合中出现索引的位置
        int index=getIndex(list,changeSid);
        //4.根据索引判断,学号在集合中是否存在
        while (index==-1) {
            //1.不存在,给出提示信息
            changeSid = console.readString("学生信息不存在,请重新输入你要修改的学生的学号,输入0退出");
            if("0".equals(changeSid)) return ;
            index=getIndex(list,changeSid);
        }
        //2.存在,接收新的学生信息
        String name=console.readString("请输入新的学生姓名");
        int age=console.readNumber("请输入新的学生年龄");
        String birthday=console.readString("请输入新的学生生日");
        //封装为一个新的学生的对象
        Student student = new Student(changeSid, name, age, birthday);
        list.set(index,student);
        System.out.println("修改成功");
    }

    private void del() {
        //1.给出提示信息 //2.键盘接收要删除的学号
        String deleteSid=console.readString("请输入你要删除的学生的学号");
        //3.调用getIndex方法,查找该学号在集合中出现索引的位置
        int index=getIndex(list,deleteSid);
        //4.根据索引判断,学号在集合中是否存在
        while (index==-1) {
            //1.不存在,给出提示信息
            deleteSid = console.readString("学生信息不存在,请重新输入你要删除的学生的学号,输入0退出");
            if("0".equals(deleteSid)) return ;
            index=getIndex(list,deleteSid);
        }
            //2.存在,使用remove方法删除集合中的元素
            list.remove(index);
        System.out.println("删除成功");
    }

    private void addstu() {
        String sid;
        //1.给出输入的提示信息
        while (true){
            sid=console.readString("请输入学生的学号");
            int index=getIndex(list,sid);
            if(index==-1){
                break;
            }
            else{
                System.out.println("学号已经存在,请输入一个新的学号");
            }
        }
        String name=console.readString("请输入学生的姓名");
        int age=console.readNumber("请输入学生的年龄");
        String birthday=console.readString("请输入学生的生日");
        //2.将键盘录入的学生信息封装为学生对象
        Student stu = new Student(sid,name,age,birthday);
        //3.将封装好的学生信息添加到集合中
        list.add(stu);
        System.out.println("添加成功");
    }

    //  删除/修改学生学号不存在的问题
    //  学号存在:返回正确的索引值  -->集合调用remove方法,根据索引修改或者删除
    //  学号不存在:返回-1  -->控制台:信息不存在
    public int getIndex(ArrayList<Student> list,String sid){
        //1.假设传入的学号,在集合中不存在
        int index=-1;
        //2.遍历集合,获取每一个学生对象,准备金进行查找
        for (int i = 0; i < list.size(); i++) {
            Student stu=list.get(i);
            //3.获取每一个学生对象的学号
            String id=stu.getSid();
            //4.使用获取的学生学号,和传入的学号进行对比
            if(id.equals(sid)){
                //存在:让index正确变量记录正确的索引位置
                index=i;
            }
        }
        return index;
    }
}

```

6. Main方法
```java
public class Main {
    public static void main(String[] args) {
        StudentManager manager = new StudentManager();
        manager.run();
    }
}
```