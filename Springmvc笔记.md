

[TOC]

#  

#     1.SpringMVC框架简介

**Spring的模型-视图-控制器（MVC）**框架是围绕一个 **DispatcherServlet** 来设计的，这个Servlet会把请求分发给各个处理器，并支持可配置的处理器映射、视图渲染、本地化、时区与主题渲染等，甚至还能支持文件上传。

处理器是你的应用中注解了 @Controller 和 @RequestMapping 的类和方法，Spring为处理器方法提供了极其多样灵活的配置。Spring 3.0以后提供了 @Controller 注解机制、 @PathVariable 注解以及一些其他的特性，你可以使用它们来进行***RESTful web***站点和应用的开发。

在**Spring Web MVC**中，你可以使用任何对象来作为命令对象或表单返回对象，而无须实现一个框架相关的接口或基类。

**Spring**的数据绑定非常灵活：比如，它会把数据类型不匹配当成可由应用自行处理的运行时验证错误，而非系统错误。你可能会为了避免非法的类型转换在表单对象中使用字符串来存储数据，但无类型的字符串无法描述业务数据的真正含义，并且你还需要把它们转换成对应的业务对象类型。有了Spring的验证机制，意味着你再也不需这么做了，并且**直接将业务对象绑定到表单对象上通常是更好的选择**。

**Spring**的**视图解析**也是设计得异常灵活。控制器一般负责准备一个 Map 模型、填充数据、返回一个合适的视图名等，同时它也可以直接将数据写到响应流中。视图名的解析高度灵活，支持多种配置，包括通过文件扩展名、 Accept 内容头、bean、配置文件等的配置，甚至你还可以自己实现一个视图解析器 ViewResolver 。

模型（MVC中的M，model）其实是一个 Map 类型的接口，彻底地把数据从视图技术中抽象分离了出来。你可以与基于模板的渲染技术直接整合，如JSP、Velocity和Freemarker等，或者你还可以直接生成XML、JSON、Atom以及其他多种类型的内容。 Map 模型会简单地被转换成合适的格式，比如JSP的请求属性（attribute），一个Velocity模板的模型等。

------



# 2.Spring Web MVC的新特性

### 2.1 Spring的web模块支持许多web相关的特性：

- **清晰的职责分离。**每个角色——控制器，验证器，命令对象，表单对象，模型对象， DispatcherServlet ，处理器映射，视图解析器，等等许多——的工作，都可以由相应的对象来完成。

- **强大、直观的框架和应用bean的配置。**这种配置能力包括能够从不同的上下文中进行简单的引用，比如在web控制器中引用业务对象、验证器等。

- **强大的适配能力、非侵入性和灵活性。**Spring MVC支持你定义任意的控制器方法签名，在特定的场景下你还可以添加适合的注解（比如 @RequestParam 、 @RequestHeader 、 @PathVariable 等）

- **可复用的业务代码，使你远离重复代码**。你可以使用已有的业务对象作为命令对象或表单对象，而不需让它们去继承一个框架提供的什么基类。

- **可定制的数据绑定和验证**。类型不匹配仅被认为是应用级别的验证错误，错误值、本地化日期、数字绑定等会被保存。你不需要再在表单对象使用全String字段，然后再手动将它们转换成业务对象。

- **可定制的处理器映射和视图解析。**处理器映射和视图解析策略从简单的基于URL配置，到精细专用的解析策略，Spring全都支持。在这一点上，Spring比一些依赖于特定技术的web框架要更加灵活。

- **灵活的模型传递**。Spring使用一个名称/值对的Map来做模型，这使得模型很容易集成、传递给任何类型的视图技术。

- **可定制的本地化信息、时区和主题解析。**支持用/不用Spring标签库的JSP技术，支持JSTL，支持无需额外配置的Velocity模板，等等。

- **一个简单但功能强大的JSP标签库**，通常称为Spring标签库，它提供了诸如数据绑定、主题支持等一些特性的支持。这些定制的标签为标记（markup）你的代码提供了最大程度Spring Web MVC的新特性12的灵活性。关于标签库描述符（descriptor）的更多信息，请参考附录第42章 Spring JSP标签库

- **一个Spring 2.0开始引入的JSP表单标签库**。它让你在JSP页面中编写表单简单许多。关于标签库描述符（descriptor）的更多信息，请参考附录 第43章 Spring表单的JSP标签库新增生命周期仅绑定到当前HTTP请求或HTTP会话的Bean类型。严格来说，这不是Spring MVC自身的特性，而是Spring MVC使用的上下文容器 WebApplicationContext 所提供的特性。这些bean的scope在6.5.4 请求、会话及全局会话scope一节有详细描述。

  # 允许其他MVC实现

有些项目可能更倾向于使用非Spring的MVC框架。 许多团队希望仍然使用现有的技术栈，比如JSF等，这样他们掌握的技能和工具依然能发挥作用。
如果你确实不想使用Spring的Web MVC，但又希望能从Spring提供的一些解决方案中受益，那么将你所使用的框架和Spring进行集成也很容易。***只需要在 ContextLoaderListener 中启动一个Spring的根应用上下文（root application context），然后你就可以在任何action对象中通过其 ServletContext 属性（或通过Spring对应的helper方法）取得。***不需要任何侵入性的插件，因此不需要复杂的集成。从应用层的视角来看，你只是将Spring当成依赖库使用，并且将它的根应用上下文实例作为应用进入点。
即使不用Spring的Web MVC框架，你配置的其他Spring的bean和服务也都能很方便地取得。在这种场景下，Spring与其他web框架的使用不冲突。Spring只是在许多问题上提出了其他纯web MVC框架未曾提出过的解决方案，比如bean的配置、数据存取、事务处理等，仅此而已。因此，如果你只是想使用Spring的一部分特性来增强你的应用，比如Spring提供的JDBC/Hibernate事务抽象等，那么你可以将Spring作为一个中间层和/或数据存取层来使用。

------



# 3. DispatcherServlet

**Spring MVC**框架，与其他很多**web**的**MVC**框架一样：请求驱动；所有设计都围绕着一个中央
**Servlet**来展开，它负责把所有请求分发到控制器；同时提供其他**web**应用开发所需要的功
能。不过**Spring**的中央处理器， **DispatcherServlet** ，能做的比这更多。它与**Spring IoC容器**
做到了无缝集成，这意味着，Spring提供的任何特性，在**Spring MVC**中你都可以使用。

**DispatcherServlet** 应用的其实就是一个“**前端控制器**”的设计模式（其他很多优秀的
web框架也都使用了这个设计模式

**DispatcherServlet** 其实就是个 **Servlet** （它继承自 HttpServlet 基类），同样也需要在你
**web**应用的 **web.xml** 配置文件下声明。你需要在 **web.xml** 文件中把你希望 **DispatcherServlet** 处理的请求映射到对应的**URL**上去。这就是标准的**Java EE Servlet**配置；下面的代码就展示了对 **DispatcherServlet** 和路径映射的声明：

```xml
<web-app>
    <servlet>
        <servlet-name>example</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>example</servlet-name>
        <url-pattern>/example/*</url-pattern>
    </servlet-mapping>
</web-app>
```

在上面的例子中，所有路径以 **/example** 开头的请求都会被名字为 **example** 的 **DispatcherServlet** 处理。在**Servlet 3.0+**的环境下，你还可以用编程的方式配置**Servlet**容器。下面是一段这种基于代码配置的例子，它与上面定义的 **web.xml** 配置文件是等效的。

```java
public class MyWebApplicationInitializer implements WebApplicationInitializer {
@Override
public void onStartup(ServletContext container) {
    ServletRegistration.Dynamic registration = container.addServlet("dispatcher",
                                 				new DispatcherServlet());
    registration.setLoadOnStartup(1);
    registration.addMapping("/example/*");
    }
}
```

**WebApplicationInitializer** 是**Spring MVC**提供的一个接口，它会查找你所有基于代码的配置，并应用它们来初始化**Servlet 3**版本以上的**web**容器。它有一个抽象的实现 **AbstractDispatcherServletInitializer** ，用以简化 **DispatcherServlet** 的注册工作：你只需要指定其**servlet**映射（**mapping**）即可。

## 3.1 WebApplicationContext中特殊的bean类型

**Spring**的 **DispatcherServlet** 使用了特殊的**bean**来处理请求、渲染视图等，这些特定的**bean**是**Spring MVC**框架的一部分。如果你想指定使用哪个特定的**bean**，你可以在**web**应用上下文 **WebApplicationContext** 中简单地配置它们。当然这只是可选的，**Spring MVC**维护了一个默认的**bean**列表，如果你没有进行特别的配置，框架将会使用默认的**bean**。下一小节会介绍更多的细节，这里，我们将先快速地看一下， **DispatcherServlet** 都依赖于哪些特殊的bean来进行它的初始化。

| bean的类型作用                                | 作用                                                         |
| --------------------------------------------- | :----------------------------------------------------------- |
| **HandlerMapping**                            | **处理器映射**。它会根据某些规则将进入容器的请求映射到具体的处理器以及一系列前处理器和后处理器（即处理器拦截器）上。具体的规则视 **HandlerMapping** 类的实现不同而有所不同。其最常用的一个实现支持你在控制器上添加注解，配置请求路径。当然，也存在其他的实现。 |
| **HandlerAdapter**                            | **处理器适配器。**拿到请求所对应的处理器后，适配器将负责去调用该处理器，这使得 **DispatcherServlet** 无需关心具体的调用细节。比方说，要调用的是一个基于注解配置的控制器，那么调用前还需要从许多注解中解析出一些相应的信息。因此， **HandlerAdapter** 的主要任务就是对 **DispatcherServlet** 屏蔽这些具体的细节。 |
| **HandlerExceptionResolver**                  | **处理器异常解析器。**它负责将捕获的异常映射到不同的视图上去，此外还支持更复杂的异常处理代码。 |
| **ViewResolver**                              | **视图解析器**。它负责将一个代表逻辑视图名的字符串（**String**）映射到实际的视图类型 **View** 上。 |
| **LocaleResolver** &**LocaleContextResolver** | **地区解析器** 和 **地区上下文解析器**。它们负责解析客户端所在的地区信息甚至时区信息，为国际化的视图定制提供了支持。 |
| **ThemeResolver**                             | **主题解析器**。它负责解析你web应用中可用的主题，比如，提供一些个性化定制的布局等。 |
| **MultipartResolver**                         | 解析**multi-part**的传输请求，比如支持通过**HTML**表单进行的文件上传等。 |
| **FlashMapManager**                           | **FlashMap**管理器。它能够存储并取回两次请求之间的 **FlashMap** 对象。后者可用于在请求之间传递数据，通常是在请求重定向的情境下使用。 |

## 3.2 默认的DispatcherServlet配置

上一小节讲到， **DispatcherServlet** 维护了一个列表，其中保存了其所依赖的所有**bean**的默认
实现。这个列表保存在包 **org.springframework.web.servlet** 下的 **DispatcherServlet.properties** 文件中。

## 3.3 DispatcherServlet的处理流程

配置好 **DispatcherServlet** 以后，开始有请求会经过这个 **DispatcherServlet** 。此时， **DispatcherServlet** 会依照以下的次序对请求进行处理：

- 首先，搜索应用的上下文对象 **WebApplicationContext** 并把它作为一个属性（**attribute**）
  绑定到该请求上，以便控制器和其他组件能够使用它。属性的键名默认
  为 **DispatcherServlet.WEB_APPLICATION_CONTEXT_ATTRIBUTE**
- 将地区（**locale**）解析器绑定到请求上，以便其他组件在处理请求（渲染视图、准备数据
  等）时可以获取区域相关的信息。如果你的应用不需要解析区域相关的信息，忽略它即
  可
- 将主题（**theme**）解析器绑定到请求上，以便其他组件（比如视图等）能够了解要渲染哪
  个主题文件。同样，如果你不需要使用主题相关的特性，忽略它即可
- 如果你配置了**multipart**文件处理器，那么框架将查找该文件是不是**multipart**（分为多个部
  分连续上传）的。若是，则将该请求包装成一个 **MultipartHttpServletRequest** 对象，以
  便处理链中的其他组件对它做进一步的处理。
- 为该请求查找一个合适的处理器。如果可以找到对应的处理器，则与该处理器关联的整
  条执行链（前处理器、后处理器、控制器等）都会被执行，以完成相应模型的准备或视
  图的渲染
- 如果处理器返回的是一个模型（**model**），那么框架将渲染相应的视图。若没有返回任何
  模型（可能是因为前后的处理器出于某些原因拦截了请求等，比如，安全问题），则框
  架不会渲染任何视图，此时认为对请求的处理可能已经由处理链完成了

如果在处理请求的过程中抛出了异常，那么上下文 **WebApplicationContext** 对象中所定义的异常处理器将会负责捕获这些异常。通过配置你自己的异常处理器，你可以定制自己处理异常的方式

**Spring**的 **DispatcherServlet** 也允许处理器返回一个**Servlet API**规范中定义的 最后修改时间戳（last-modification-date） 值。决定请求最后修改时间的方式很直接： **DispatcherServlet** 会先查找合适的处理器映射来找到请求对应的处理器，然后检测它是否实现了 **LastModified** 接口。若是，则调用接口的 **long getLastModified(request)** 方法，并将该返回值返回给客户端。

你可以定制 **DispatcherServlet** 的配置，具体的做法，是在 **web.xml** 文件中，**Servlet**的声明元素上添加一些**Servlet**的初始化参数（通过 **init-param** 元素）。该元素可选的参数列表如下：

| 可选参数                  | 解释                                                         |
| ------------------------- | ------------------------------------------------------------ |
| **contextClass**          | 任意实现了 **WebApplicationContext** 接口的类。这个类会初始化该**servlet**所需要用到的上下文对象。默认情况下，框架会使用一个 **XmlWebApplicationContext** 对象。 |
| **contextConfigLocation** | 一个指定了上下文配置文件路径的字符串，该值会被传入给 **contextClass** 所指定的上下文实例对象。该字符串内可以包含多个字符串，字符串之间以逗号分隔，以此支持你进行多个上下文的配置。在多个上下文中重复定义的**bean**，以最后加载的**bean**定义为准 |
| **namespace**             | **WebApplicationContext** 的命名空间。默认是 **[servlet-name]-servlet** |

# 4 控制器(Controller)的实现

控制器作为应用程序逻辑的处理入口，它会负责去调用你已经实现的一些服务。通常，一个控制器会接收并解析用户的请求，然后把它转换成一个模型交给视图，由视图渲染出页面最终呈现给用户。**Spring**对控制器的定义非常宽松，这意味着你在实现控制器时非常自由。

**Spring 2.5**以后引入了基于注解的编程模型，你可以在你的控制器实现上添加 **@RequestMapping** 、 **@RequestParam 、 @ModelAttribute** 等注解。注解特性既支持基于**Servlet**的**MVC**，也可支持基于**Portlet**的**MVC**。通过此种方式实现的控制器既无需继承某个特定的基类，也无需实现某些特定的接口。而且，它通常也不会直接依赖于**Servlet**或**Portlet**的**API**来进行编程，不过你仍然可以很容易地获取**Servlet**或**Portlet**相关的变量、特性和设施等。

```java
@Controller
public class HelloWorldController {
    @RequestMapping("/helloWorld")	
    public String helloWorld(Model model) {
        model.addAttribute("message", "Hello World!");
        return "helloWorld";
    }
}
```

你可以看到， **@Controller** 注解和 **@RequestMapping** 注解支持多样的方法名和方法签名。在上面这个例子中，方法接受一个 **Model** 类型的参数并返回一个字符串 **String** 类型的视图名。但事实上，方法所支持的参数和返回值有非常多的选择。 **@Controller** 和 **@RequestMapping** 及其他的一些注解，共同构成了**Spring MVC**框架的基本实现。

## 4.1 使用@Controller注解定义一个控制器

**@Controller** 注解表明了一个类是作为控制器的角色而存在的。**Spring**不要求你去继承任何控
制器基类，也不要求你去实现**Servlet**的那套**API**。当然，如果你需要的话也可以去使用任何与
**Servlet**相关的特性和设施。

**@Controller** 注解可以认为是被标注类的原型（**stereotype**），表明了这个类所承担的角色。
分派器（ **DispatcherServlet** ）会扫描所有注解了 **@Controller** 的类，检测其中通
过 **@RequestMapping** 注解配置的方法。

当然，你也可以不使用 **@Controller** 注解而显式地去定义被注解的**bean**，这点通过标准的**Spring bean**的定义方式，在**dispather**的上下文属性下配置即可做到。但是 **@Controller** 原型是可以被框架自动检测的，**Spring**支持**classpath**路径下组件类的自动检测，以及对已定义**bean**的自动注册。

你需要在配置中加入组件扫描的配置代码来开启框架对注解控制器的自动检测。请使用下面
**XML**代码所示的**spring-context schema**：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xmlns:p="http://www.springframework.org/schema/p"
xmlns:context="http://www.springframework.org/schema/context"
xsi:schemaLocation="
http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans.xsd
http://www.springframework.org/schema/context
http://www.springframework.org/schema/context/spring-context.xsd">
<context:component-scan base-package="org.springframework.samples.petclinic.web"/>
<!-- ... -->
</beans>
```



## 4.2 使用@RequestMapping注解映射请求路径

你可以使用 **@RequestMapping** 注解来将请求**URL**，如 **/appointments** 等，映射到整个类上或某个特定的处理器方法上。

一般来说，**类级别的注解**负责将一个特定（或符合某种模式）的请求路径映射到一个控制器上，同时通过**方法级别**的注解来细化映射，即根据特定的HTTP请求方法（“GET”“POST”方法等）、HTTP请求中是否携带特定参数等条件，将请求映射到匹配的方法上。

```java
@Controller
@RequestMapping("/appointments")
public class AppointmentsController {

    private final AppointmentBook appointmentBook;

    @Autowired
    public AppointmentsController(AppointmentBook appointmentBook) {
    	this.appointmentBook = appointmentBook;
    }

    @RequestMapping(method = RequestMethod.GET)
    public Map<String, Appointment> get() {
   	 return appointmentBook.getAppointmentsForToday();
    }
    @RequestMapping(path = "/{day}", method = RequestMethod.GET)
    public Map<String, Appointment> getForDay(@PathVariable @DateTimeFormat(iso=ISO.DA
    TE) Date day, Model model) {
    	return appointmentBook.getAppointmentsForDay(day);
    }
    @RequestMapping(path = "/new", method = RequestMethod.GET)
    public AppointmentForm getNewForm() {
   		 return new AppointmentForm();
    }
    @RequestMapping(method = RequestMethod.POST)
    public String add(@Valid AppointmentForm appointment, BindingResult result) {
        if (result.hasErrors()) {
        	return "appointments/new";
        }
        appointmentBook.addAppointment(appointment);
        return "redirect:/appointments";
    }
}
```

在上面的示例中，许多地方都使用到了 **@RequestMapping** 注解。第一次使用点是作用于**类级别**的，它指示了所有 **/appointments** 开头的路径都会被映射到控制器下。 **get()** 方法上的 **@RequestMapping** 注解对请求路径进行了进一步细化：它仅接受**GET**方法的请求。这样，一个请求路径为 **/appointments** 、**HTTP**方法为**GET**的请求，将会最终进入到这个方法被处理。 **add()** 方法也做了类似的细化，而 **getNewForm()** 方法则同时注解了能够接受的请求的**HTTP**方法和路径。这种情况下，一个路径为 a**ppointments/new** 、**HTTP**方法为**GET**的请求将会被这个方法所处理。

**getForDay()** 方法则展示了使用 **@RequestMapping** 注解的另一个技巧：**URI模板**。

类级别的 **@RequestMapping** 注解并不是必须的。**不配置的话则所有的路径都是绝对路径，而非**
**相对路径。**以下的代码示例展示了一个具有多个处理器方法的控制器：

```java
@Controller
public class ClinicController {
    private final Clinic clinic;
    
    @Autowired
    public ClinicController(Clinic clinic) {
    	this.clinic = clinic;
    }
    
    @RequestMapping("/")
    public void welcomeHandler() {
    }
    
    @RequestMapping("/vets")
    public ModelMap vetsHandler() {
    	return new ModelMap(this.clinic.getVets());
    }
}
```

以上代码没有指定请求必须是**GET**方法还是**PUT/POST**或其他方法， **@RequestMapping** 注解默认会映射所有的**HTTP**请求方法。如果仅想接收某种请求方法，请在注解中指定之 **@RequestMapping(method=GET)** 以缩小范围。

### 4.2.1 @Controller和面向切面（AOP）代理

有时，我们希望在运行时使用**AOP**代理来装饰控制器，比如当你直接在控制器上使用 **@Transactional** 注解时。这种情况下，我们推荐使用类级别（在控制器上使用）的代理方式。这一般是代理控制器的默认做法。如果控制器必须实现一些接口，而该接口又不支持**Spring Context**的回调（比如 InitializingBean , *Aware 等接口），那要配置类级别的代理就必须手动配置了。比如，原来的配置文件 <tx:annotation-driven/> 需要显式配置为 

<tx:annotation-driven proxy-target-class="true"/> 。

### 4.2.2 Spring MVC 3.1中新增支持@RequestMapping的一些类

**Spring 3.1**中新增了一组类用以增强 **@RequestMapping** ，分别是 **RequestMappingHandlerMapping** 和 **RequestMappingHandlerAdapter** 。有部分**Spring MVC 3.1**之后新增的特性，这两个注解甚至是必须的。在**MVC**命名空间和**MVCJava**编程配置方式下，这组类及其新特性默认是开启的。但若你使用其他配置方式，则该特性必须手动配置才能使用。

在**Spring 3.1**之前，框架会在两个不同的阶段分别检查类级别和方法级别的请求映射——首
先， **DefaultAnnotationHanlderMapping** 会先在类级别上选中一个控制器，然后再通
过 **AnnotationMethodHandlerAdapter** 定位到具体要调用的方法。

现在有了**Spring 3.1**后引入的这组新类， **RequestMappingHandlerMapping** 成为了这两个决策实际发生的唯一一个地方。你可以把控制器中的一系列处理方法当成是一系列独立的服务节点，每个从类级别和方法级别的 **@RequestMapping** 注解中获取到足够请求路径映射信息。

这种新的处理方式带来了新的可能性。之前的 **HandlerInterceptor** 或 **HandlerExceptionResolver** 现在可以确定拿到的这个处理器肯定是一个 **HandlerMethod** 类型，因此它能够精确地了解这个方法的所有信息，包括它的参数、应用于其上的注解等。这样，内部对于一个**URL**的处理流程再也不需要分隔到不同的控制器里面去执行了。同时，也有其他的一些变化，比如有些事情就没法这么玩儿了：

- 先通过 **SimpleUrlHandlerMapping** 或 **BeanNameUrlHandlerMapping** 来拿到负责处理请求的控制器，然后通过 **@RequestMapping** 注解配置的信息来定位到具体的处理方法；
- 依靠方法名称来作为选择处理方法的标准。比如说，两个注解了 **@RequestMapping** 的方法除了方法名称拥有完全相同的**URL**映射和**HTTP**请求方法。在新版本下， **@RequestMapping** 注解的方法必须具有唯一的请求映射；
- 定义一个默认方法（即没有声明路径映射），在请求路径无法被映射到控制器下更精确的方法上去时，为该请求提供默认处理。在新版本中，如果无法为一个请求找到合适的处理方法，那么一个404错误将被抛出；

如果使用原来的类，以上的功能还是可以做到。但是，如果要享受**Spring MVC 3.1**版本带来的方便特性，你就需要去使用新的类.

### 4.2.3 URI模板

**URI**模板可以为快速访问 **@RequestMapping** 中指定的**URL**的一个特定的部分提供很大的便利。

**URI**模板是一个类似于**URI**的字符串，只不过其中包含了一个或多个的变量名。当你使用实际的值去填充这些变量名的时候，模板就退化成了一个**URI**。在**URI**模板的**RFC**提议中定义了一个**URI**是如何进行参数化的。比如说，一个这个**URI**模板 http://www.example.com/users/{userId} 就包含了一个变量名**userId**。将值**fred**赋给这个变量名后，它就变成了一个URI： http://www.example.com/users/fred 。

在**Spring MVC**中你可以在方法参数上使用 **@PathVariable** 注解，将其与**URI**模板中的参数绑定起来：

```java
@RequestMapping(path="/owners/{ownerId}", method=RequestMethod.GET)
public String findOwner(@PathVariable String ownerId, Model model) {
    Owner owner = ownerService.findOwner(ownerId);
    model.addAttribute("owner", owner);
    return "displayOwner";
}
```

## 4.3 定义@RequestMapping注解的处理方法(handler method)

使用 **@RequestMapping** 注解的处理方法可以拥有非常灵活的方法签名，它支持的方法参数及返回值类型将在接下来的小节讲述。

### 4.3.1支持的方法参数类型:

- 请求或响应对象（**Servlet API**）。可以是任何具体的请求或响应类型的对象，比如， **ServletRequest** 或 **HttpServletRequest** 对象等。

- **HttpSession** 类型的会话对象（**Servlet API**）。使用该类型的参数将要求这样一个**session**的存在，因此这样的参数永不为 **null** 

  [^session]: 存取session可能不是线程安全的，特别是在一个Servlet的运行环境中。如果应用可能有多个请求同时并发存取一个session场景，请考虑将RequestMappingHandlerAdapter类中的"synchronizeOnSession"标志设置为"true"。

- **org.springframework.web.context.request.WebRequest** 或 **org.springframework.web.context**
  **.request.NativeWebRequest** 。允许存取一般的请求参数和请求/会话范围的属性（**attribute**），同时无需绑定使用**Servlet/Portlet**的**API**

- 当前请求的地区信息 **java.util.Locale** ，由已配置的最相关的地区解析器解析得到。就是应用中配置的**LocaleResolver** 或 **LocaleContextResolver**

- 与当前请求绑定的时区信息 **java.util.TimeZone** （java 6以上的版本）/ **java.time.ZoneId** （java 8），由 **LocaleContextResolver** 解析得到

- 用于存取请求正文的 **java.io.InputStream** 或 **java.io.Reader** 。该对象与通过**Servlet API**拿到的输入流/**Reader**是一样的

- 用于生成响应正文的 **java.io.OutputStream** 或 **java.io.Writer** 。该对象与通过**ServletAPI**拿到的输出流/**Writer**是一样的

- **org.springframework.http.HttpMethod** 。可以拿到**HTTP**请求方法

- 包装了当前被认证用户信息的 **java.security.Principal**

- 带 **@PathVariable** 注解的方法参数，其存放了**URI**模板变量中的值。

- 带 **@MatrixVariable** 注解的方法参数，其存放了**URI**路径段中的键值对。

- 带 **@RequestParam** 注解的方法参数，其存放了**Servlet**请求中所指定的参数。参数的值会被转换成方法参数所声明的类型。

- 带 **@RequestHeader** 注解的方法参数，其存放了**Servlet**请求中所指定的**HTTP**请求头的值。参数的值会被转换成方法参数所声明的类型。

- 带 **@RequestBody** 注解的参数，提供了对**HTTP**请求体的存取。参数的值通过 **HttpMessageConverter** 被转换成方法参数所声明的类型。

- **HttpEntity<?>** 类型的参数，其提供了对**HTTP**请求头和请求内容的存取。请求流是通过 **HttpMessageConverter** 被转换成**entity**对象的。

- **java.util.Map** / **org.springframework.io.Model** / **org.springframework.ui.ModelMap** 类型的参数，用以增强默认暴露给视图层的模型(**model**)的功能

- 还有很多....记不过来了

  ------

### 4.3.1支持的方法返回类型:

  以下是**handler**方法允许的所有返回类型：

- **ModelAndView** 对象，其中**model**隐含填充了命令对象，以及注解了 **@ModelAttribute** 字段的存取器被调用所返回的值。

- **Model** 对象，其中视图名称默认由 **RequestToViewNameTranslator** 决定，**model**隐含填充了命令对象以及注解了 **@ModelAttribute** 字段的存取器被调用所返回的值

- **Map** 对象，用于暴露model，其中视图名称默认由 **RequestToViewNameTranslator** 决定，**model**隐含填充了命令对象以及注解了 **@ModelAttribute** 字段的存取器被调用所返回的值

- **View** 对象。其中**model**隐含填充了命令对象，以及注解了 **@ModelAttribute** 字段的存取器被调用所返回的值。**handler**方法也可以增加一个 **Model** 类型的方法参数来增强**model**

- **String** 对象，其值会被解析成一个逻辑视图名。其中，**model**将默认填充了命令对象以及注解了 **@ModelAttribute** 字段的存取器被调用所返回的值。**handler**方法也可以增加一个 **Model** 类型的方法参数来增强**model**

- **void** 。如果处理器方法中已经对**response**响应数据进行了处理（比如在方法参数中定义一个 **ServletResponse** 或 **HttpServletResponse** 类型的参数并直接向其响应体中写东西），那么方法可以返回**void**。**handler**方法也可以增加一个 **Model** 类型的方法参数来增强**model**

- 如果处理器方法注解了 **ResponseBody** ，那么返回类型将被写到**HTTP**的响应体中，而返回值会被**HttpMessageConverters** 转换成所方法声明的参数类型。

### 4.3.3 使用@RequestParam将请求参数绑定至方法参数

可以使用 **@RequestParam** 注解将请求参数绑定到控制器的方法参数上。

```java
@Controller
@RequestMapping("/pets")
@SessionAttributes("pet")
public class EditPetForm {
    // ...
    @RequestMapping(method = RequestMapping.GET)
    public String setupForm(@RequestParam("petId") int petId, ModelMap model) {
        Pet pet = this.clinic.loadPet(petId);
        model.addAttribute("pet", pet);
        return "petForm";
    }
    // ,..
}
```

若参数使用了该注解，则该参数默认是必须提供的，但你也可以把该参数标注为非必须的：只需要将 **@RequestParam** 注解的 **required** 属性设置为 **false** 即可（比如， **@RequestParam(path="id", required=false)** ）。

若所注解的方法参数类型不是 **String** ，则类型转换会自动地发生。
若 **@RequestParam** 注解的参数类型是 **Map<String, String>** 或者 **MultiValueMap<String,String>** ，则该**Map**中会自动填充所有的请求参数。

### 4.3.4使用@RequestBody注解映射请求体

方法参数中的 **@RequestBody** 注解暗示了方法参数应该被绑定了**HTTP**请求体的值。

```java
@RequestMapping(path = "/something", method = RequestMethod.PUT)
public void handle(@RequestBody String body, Writer writer) throws IOException {
	writer.write(body);
}
```

请求体到方法参数的转换是由 **HttpMessageConverter** 完成的。 **HttpMessageConverter** 负责将**HTTP**请求信息转换成对象，以及将对象转换回一个**HTTP**响应体。

对于 **@RequestBody** 注解， **RequestMappingHandlerAdapter** 提供了以下几种默认的 **HttpMessageConverter** 支持：

- **ByteArrayHttpMessageConverter** 用以转换字节数组
- **StringHttpMessageConverter** 用以转换字符串
- **FormHttpMessageConverter** 用以将表格数据转换成 **MultiValueMap<String, String>** 或从 **MultiValueMap<String, String>** 中转换出表格数据
- **SourceHttpMessageConverter** 用于 **javax.xml.transform.Source** 类的互相转换

### 4.3.5使用@ResponseBody注解映射响应体

**@ResponseBody** 注解与 **@RequestBody** 注解类似。 **@ResponseBody** 注解可被应用于方法上，标志该方法的返回值（更正，原文是**return type**，看起来应该是返回值）应该被直接写回到**HTTP**响应体中去（而不会被被放置到**Model**中或被解释为一个视图名）。

```java
@RequestMapping(path = "/something", method = RequestMethod.PUT)
@ResponseBody
public String helloWorld() {
	return "Hello World"
}
```

上面的代码结果是文本 **Hello World** 将被写入HTTP的响应流中。
与 **@RequestBody** 注解类似，**Spring**使用了一个 **HttpMessageConverter** 来将返回对象转换到响应体中。

### 4.3.6使用@RestController注解创建REST控制器

当今让控制器实现一个**REST API**是非常常见的，这种场景下控制器只需要提供**JSON**、**XML**或其他自定义的媒体类型内容即可。

你不需要在每个 **@RequestMapping** 方法上都增加一个 **@ResponseBody** 注解，更简明的做法是，给你的控制器加上一个 **@RestController** 的注解。

**@RestController** 是一个原生内置的注解，它结合了 **@ResponseBody** 与 **@Controller** 注解的功能。不仅如此，它也让你的控制器更表义，而且在框架未来的发布版本中，它也可能承载更多的意义。

### 4.3.7使用HTTP实体HttpEntity

**HttpEntity** 与 **@RequestBody** 和 **@ResponseBody** 很相似。除了能获得请求体和响应体中的内容之外， **HttpEntity** （以及专门负责处理响应的 **ResponseEntity** 子类）还可以存取请求头和响应头，像下面这样：

```java
@RequestMapping("/something")
public ResponseEntity<String> handle(HttpEntity<byte[]> requestEntity) throws 
    UnsupportedEncodingException {
    String requestHeader = requestEntity.getHeaders().getFirst("MyRequestHeader");
    byte[] requestBody = requestEntity.getBody();
    // do something with request header and body
    HttpHeaders responseHeaders = new HttpHeaders();
    responseHeaders.set("MyResponseHeader", "MyValue");
    return new ResponseEntity<String>("Hello World", responseHeaders, 
                                      HttpStatus.CREATED);
}
```

上面这段示例代码先是获取了 **MyRequestHeader** 请求头的值，然后读取请求体的主体内容。读
完以后往影响头中添加了一个自己的响应头 **MyResponseHeader** ，然后向响应流中写了字符
串 **Hello World** ，最后把响应状态码设置为201（创建成功）。

与 **@RequestBody** 与 **@ResponseBody** 注解一样，**Spring**使用了 **HttpMessageConverter** 来对请求流
和响应流进行转换。

### 4.3.8 对方法使用@ModelAttribute注解

**@ModelAttribute** 注解可被应用在方法或方法参数上。

注解在方法上的 **@ModelAttribute** 说明了方法的作用是用于添加一个或多个属性到**model**上。这样的方法能接受与 **@RequestMapping** 注解相同的参数类型，只不过不能直接被映射到具体的请求上。在同一个控制器中，注解了 **@ModelAttribute** 的方法实际上会在 **@RequestMapping** 方法之前被调用。

```java
// Add one attribute
// The return value of the method is added to the model under the name "account"
// You can customize the name via @ModelAttribute("myAccount")
@ModelAttribute
public Account addAccount(@RequestParam String number) {
	return accountManager.findAccount(number);
}
// Add multiple attributes
@ModelAttribute
public void populateModel(@RequestParam String number, Model model) {
	model.addAttribute(accountManager.findAccount(number));
	// add more ...
}

```

**@ModelAttribute** 方法通常被用来填充一些公共需要的属性或数据，比如一个下拉列表所预设的几种状态，或者宠物的几种类型，或者去取得一个**HTML**表单渲染所需要的命令对象，比如 **Account** 等。

留意 **@ModelAttribute** 方法的两种风格。在第一种写法中，方法通过返回值的方式默认地将添加一个属性；在第二种写法中，方法接收一个 **Model** 对象，然后可以向其中添加任意数量的属性。你可以在根据需要，在两种风格中选择合适的一种。

一个控制器可以拥有数量不限的 **@ModelAttribute** 方法。同个控制器内的所有这些方法，都会在 **@RequestMapping** 方法之前被调用。

### 4.3.9在请求之间使用@SessionAttributes注解，使用HTTP会话保存模型数据

类型级别的 **@SessionAttributes** 注解声明了某个特定处理器所使用的会话属性。通常它会列出该类型希望存储到**session**或**converstaion**中的**model**属性名或**model**的类型名，一般是用于在请求之间保存一些表单数据的**bean**。

以下的代码段演示了该注解的用法，它指定了模型属性的名称

```java
@Controller
@RequestMapping("/editPet.do")
@SessionAttributes("pet")
public class EditPetForm {
	// ...
}
```

## 5.异步请求的处理

**Spring MVC 3.2**开始引入了基于**Servlet 3**的异步请求处理。相比以前，控制器方法已经不一定需要返回一个值，而是可以返回一个 **java.util.concurrent.Callable** 的对象，并通过**SpringMVC**所管理的线程来产生返回值。与此同时，**Servlet**容器的主线程则可以退出并释放其资源了，同时也允许容器去处理其他的请求。通过一个 **TaskExecutor** ，**Spring MVC**可以在另外的线程中调用 **Callable** 。当 **Callable** 返回时，请求再携带 **Callable** 返回的值，再次被分配到**Servlet**容器中恢复处理流程。以下代码给出了一个这样的控制器方法作为例子：

```java
@RequestMapping(method=RequestMethod.POST)
public Callable<String> processUpload(final MultipartFile file) {
	return new Callable<String>() {
        public String call() throws Exception {	
            // ...
            return "someView";
        }
	};
}
```

另一个选择，是让控制器方法返回一个 **DeferredResult** 的实例。这种场景下，返回值可以由任何一个线程产生，也包括那些不是由**Spring MVC**管理的线程。举个例子，返回值可能是为了响应某些外部事件所产生的，比如一条**JMS**的消息，一个计划任务，等等。以下代码给出了一个这样的控制器作为例子：

```java
@RequestMapping("/quotes")
@ResponseBody
public DeferredResult<String> quotes() {
DeferredResult<String> deferredResult = new DeferredResult<String>();
    // Save the deferredResult somewhere..
    return deferredResult;
}
// In some other thread...
deferredResult.setResult(data);
```

