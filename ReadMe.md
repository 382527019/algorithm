## RPC 服务端demo
### 3.1.2 服务端
* 请求数据类 RpcRequest .java
~~~
@Data
public class RpcRequest implements Serializable 
~~~
服务发布 BootStrap.java
~~~
public class BootStrap 
~~~
* 代理发布，收到请求交给executorService线程池处理。RpcProxyServer.java
~~~
public class RpcProxyServer 
~~~
* 拿到请求反射调用invoke（）执行目标方法返回结果。ProcessorHandler.java
~~~
public class ProcessorHandler implements Runnable
~~~
----

## 注解
### 3.2.1 服务端
* 注解类 RemoteService.java
~~~
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)//运行时用
@Component
public @interface RemoteService {
}
~~~
* 存储bean和method类 BeanMethod.java
~~~
public class BeanMethod 
~~~

* 存储发布服务实例的map
* processor(RpcRequest rpcRequest)拿到请求数据，根据key拿到服务实例反射调用执行服务方法。
~~~
public class Mediator 
~~~
* Bean后置处理器，容器创建后扫描自定义注解@RemoteService。
* 拿到标注的类上的方法放入存储发布服务实例的map。
* InitialMerdiator.java
~~~
//BeanPostProcessor
// 称Bean后置处理器，它是Spring中定义的接口，
// 在Spring容器的创建过程中（具体为Bean初始化前后）会回调BeanPostProcessor中定义的两个方法
@Component
public class InitialMerdiator implements BeanPostProcessor 
~~~
* 实现spring的 ApplicationListener
* spring容器启动完成后会监听到一个ContextRedfreshdEvent事件
* 拿到请求交给线程池处理AnnotationProcessorHandler（socket）
* SocketServerInitial.java
~~~
//spring容器启动完成后会监听到一个ContextRedfreshdEvent事件
@Component
public class SocketServerInitial implements ApplicationListener<ContextRefreshedEvent> 
~~~
AnnotationProcessorHandler.java
~~~
public class AnnotationProcessorHandler implements Runnable
~~~
* 启动容器，定义扫描包路径 BootStrap.java
~~~
@Configurable
@ComponentScan("com.example")
public class BootStrap 
~~~
