# Spring核心原理

## IOC控制反转

### 基本概念

- IOC(Inverse Of Controll,控制反转)：就是原来代码里面需要自己手动创建的对象，依赖，反转给Spring来帮忙实现。我们需要创建一个容器，同时需要一种描述来让容器知道要创建的对象与对象之间的关系。
- 在Spring中BeanFactory就是IOC容器，在Spring初始化的时候，创建容器，并将需要创建对象和对象的关系（xml，注解）通过BeanDefinitionReader加载到BeanDefinition中并保存在BeanDefinitionMap中，然后再由IOC容器创建bean对象.

两种bean的注册方式

- 方法1：通过@Bean+@Configuration的方式直接定义要创建的对象与对象的关系
- 方式2：通过@Component定义类，这种方式必须使用@ComponetScan定位Bean扫描路径

### 常用注解

#### @Component

- 定义当前类会被IOC容器接管
- @Repository：在Dao层使用
- @Service：在Service层使用
- @Controller：在Web层使用

#### @Lazy

定义的类，在IOC容器加载的时候,并不会创建bean,而是在第一次被调用的时候创建

@Scope

定义类在IOC容器的类型范围

- singleton：单实例(单例模式，默认),全局有且仅有一个实例(跟随IOC的生命周期)
- protoype：多实例(多例模式),每次获取Bean的时候会有一个新的实例(垃圾回收器负责回收)
- request：同一次请求，每一次HTTP请求都会产生一个新的bean，同时该bean仅在当前HTTP request内有效
- session：同一个会话级别，每一次HTTP请求都会产生一个新的bean，同时该bean仅在当前HTTP session内有效

### @Bean

定义在方法上,将返回的对象放入到IOC容器中

- initMethod：属性已经注入完毕后调用
- detoryMethod：类在被销毁的时候调用

### Bean的生命周期

1. bean的实例化：spring启动后,会查找和加载需要被spring管理的Bean,并且实例化
2. bean的属性注入(bean的初始化)：bean被实例化后将Bean的引入河值注入到bean的属性中
   - 查看是否调用一些aware接口,比如BeanFactoryAware,BeanFactoryAware,ApplicationContextAware接口,分别会将Bean的名字,BeanFactory容器实例,以及Bean所在用的上下文引用传入给Bean
   - 在初始化之前,会查看是否调用了BeanPostProcessor的预初始化方法,可以对bean进行扩展
   - 调用InitializingBean的afterPropertiesSet()方法：如果Bean实现了InitializingBean接口，spring将调用他们的afterPropertiesSet()方法，类似的，如果Bean使用init-method生命了初始化方法的话，这个方法也会被调用。
   - 初始化成功之后,会查看是否调用BeanPostProcessor的初始化后的方法：如果Bean实现了BeanPostProcessor接口，spring就将调用他们的postprocessAfterInitialization()方法。可以对bean进行扩展
3. bean的正常使用：可以被应用程序正常使用了,他们将驻留在上下文中,直到应用的上下文被销毁
4. bean的销毁：调用DisposableBean的destory()方法：如果Bean实现DisposableBean接口，spring将嗲用他的destory()就扣方法，相同的，如果Bean使用了destory-method生命销毁方法，该方法也会被调用。(但由于bean也分为单例和多例,单例bean会随着IOC容器的销毁而销毁,多例的bean不会随着IOC容器的销毁而销毁,他是通过JVM里面的垃圾回收器负责回收)

### 工厂后处理器BeanFactoryPostProcessor

- BeanFactoryPostProcessor：Bean后置工厂处理器,在BeanDefinitionMap填充完毕,Bean实例化之间执行；通过工厂后置处理器，可以达到动态注册BeanDefinition，动态修改BeanDefinition，以及动态修改Bean的作用

- 可以用于手动注册BeanDefinition

  ```java
  
  ```

  