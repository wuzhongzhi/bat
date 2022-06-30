[

## 3\. Spring MVC

#### 3.1 什么是MVC？

 **参考答案**

MVC是一种设计模式，在这种模式下软件被分为三层，即Model（模型）、View（视图）、Controller（控制器）。Model代表的是数据，View代表的是用户界面，Controller代表的是数据的处理逻辑，它是Model和View这两层的桥梁。将软件分层的好处是，可以将对象之间的耦合度降低，便于代码的维护。

#### 3.2 DAO层是做什么的？

 **参考答案**

DAO是Data Access
Object的缩写，即数据访问对象，在项目中它通常作为独立的一层，专门用于访问数据库。这一层的具体实现技术有很多，常用的有Spring
JDBC、Hibernate、JPA、MyBatis等，在Spring框架下无论采用哪一种技术访问数据库，它的编程模式都是统一的。

#### 3.3 介绍一下Spring MVC的执行流程

 **参考答案**

  1. 整个过程开始于客户端发出的一个HTTP请求，Web应用服务器接收到这个请求。如果匹配DispatcherServlet的请求映射路径，则Web容器将该请求转交给DispatcherServlet处理。

  2. DispatcherServlet接收到这个请求后，将根据请求的信息（包括URL、HTTP方法、请求报文头、请求参数、Cookie等）及HandlerMapping的配置找到处理请求的处理器（Handler）。可将HandlerMapping看做路由控制器，将Handler看做目标主机。值得注意的是，在Spring MVC中并没有定义一个Handler接口，实际上任何一个Object都可以成为请求处理器。

  3. 当DispatcherServlet根据HandlerMapping得到对应当前请求的Handler后，通过HandlerAdapter对Handler进行封装，再以统一的适配器接口调用Handler。HandlerAdapter是Spring MVC框架级接口，顾名思义，HandlerAdapter是一个适配器，它用统一的接口对各种Handler方法进行调用。

  4. 处理器完成业务逻 辑的处理后，将返回一个ModelAndView给DispatcherServlet，ModelAndView包含了视图逻辑名和模型数据信息。

  5. ModelAndView中包含的是“逻辑视图名”而非真正的视图对象，DispatcherServlet借由ViewResolver完成逻辑视图名到真实视图对象的解析工作。

  6. 当得到真实的视图对象View后，DispatcherServlet就使用这个View对象对ModelAndView中的模型数据进行视图渲染。

  7. 最终客户端得到的响应消息可能是一个普通的HTML页面，也可能是一个XML或JSON串，甚至是一张图片或一个PDF文档等不同的媒体形式。

#### 3.4 说一说你知道的Spring MVC注解

 **参考答案**

 _@RequestMapping_ ：

作用：该注解的作用就是用来处理请求地址映射的，也就是说将其中的处理器方法映射到url路径上。

属性：

  * method：是让你指定请求的method的类型，比如常用的有get和post。

  * value：是指请求的实际地址，如果是多个地址就用{}来指定就可以啦。

  * produces：指定返回的内容类型，当request请求头中的Accept类型中包含指定的类型才可以返回的。

  * consumes：指定处理请求的提交内容类型，比如一些json、html、text等的类型。

  * headers：指定request中必须包含那些的headed值时，它才会用该方法处理请求的。

  * params：指定request中一定要有的参数值，它才会使用该方法处理请求。

 _@RequestParam_ ：

作用：是将请求参数绑定到你的控制器的方法参数上，是Spring MVC中的接收普通参数的注解。

属性：

  * value是请求参数中的名称。

  * required是请求参数是否必须提供参数，它的默认是true，意思是表示必须提供。

 _@RequestBody_ ：

作用：如果作用在方法上，就表示该方法的返回结果是直接按写入的Http responsebody中（一般在异步获取数据时使用的注解）。

属性：required，是否必须有请求体。它的默认值是true，在使用该注解时，值得注意的当为true时get的请求方式是报错的，如果你取值为false的话，get的请求是null。

 _@PathVaribale_ ：

作用：该注解是用于绑定url中的占位符，但是注意，spring3.0以后，url才开始支持占位符的，它是Spring
MVC支持的rest风格url的一个重要的标志。

#### 3.5 介绍一下Spring MVC的拦截器

 **参考答案**

拦截器会对处理器进行拦截，这样通过拦截器就可以增强处理器的功能。Spring
MVC中，所有的拦截器都需要实现HandlerInterceptor接口，该接口包含如下三个方法：preHandle()、postHandle()、afterCompletion()。

这些方法的执行流程如下图：

![](https://uploadfiles.nowcoder.com/images/20220224/4107856_1645694469544/31C010B3F63CB1CC1ADC5481E9E77BDB)

通过上图可以看出，Spring MVC拦截器的执行流程如下：

  * 执行preHandle方法，它会返回一个布尔值。如果为false，则结束所有流程，如果为true，则执行下一步。

  * 执行处理器逻辑，它包含控制器的功能。

  * 执行postHandle方法。

  * 执行视图解析和视图渲染。

  * 执行afterCompletion方法。

Spring MVC拦截器的开发步骤如下：

  1. 开发拦截器：

实现handlerInterceptor接口，从三个方法中选择合适的方法，实现拦截时要执行的具体业务逻辑。

  2. 注册拦截器：

定义配置类，并让它实现WebMvcConfigurer接口，在接口的addInterceptors方法中，注册拦截器，并定义该拦截器匹配哪些请求路径。

#### 3.6 怎么去做请求拦截？

 **参考答案**

如果是对Controller记性拦截，则可以使用Spring MVC的拦截器。

如果是对所有的请求（如访问静态资源的请求）进行拦截，则可以使用Filter。

如果是对除了Controller之外的其他Bean的请求进行拦截，则可以使用Spring AOP。

]

