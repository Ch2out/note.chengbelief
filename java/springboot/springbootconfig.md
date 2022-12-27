# springbootcofig

## 配置springboot详解

``` xml

pom.xml
<!-- 将spring启动器设置成父依赖 -->
<parent>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-parent</artifactId>
	<version>2.3.4.RELEASE</version>
</parent>

<dependencies>
<!-- web开发依赖 -->
	<dependency>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-web</artifactId>
	</dependency>
	<!-- lombok用来给Bean类自动生成get/set有参无参构造器及toString方法  不推荐使用 需要额外安装插件并且可能有安全问题-->
	<dependency>
     	<groupId>org.projectlombok</groupId>
    	 <artifactId>lombok</artifactId>
	</dependency>
	<!-- 当重建项目的时候自动重启项目 适用性不大 不如适用reabl插件 -->
	  <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-devtools</artifactId>
        <optional>true</optional>
    </dependency>
	<!-- 我们在为我们自己的类如bean在Application.properites配置文件没有提示文件我们可以添加此依赖。然后重新运行便会有提示注意 -->
	<dependency>
    	<groupId>org.springframework.boot</groupId>
    	<artifactId>spring-boot-configuration-processor</artifactId>
    	<optional>true</optional>
	</dependency>
</dependencies>

<!-- 下面插件作用是工程打包时，不将spring-boot-configuration-processor打进包内，让其只在编码的时候有用 这样能够大大减少mavne打包后的程序-->
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
            <configuration>
                <excludes>
                    <exclude>
                        <groupId>org.springframework.boot</groupId>
                        <artifactId>spring-boot-configuration-processor</artifactId>
                    </exclude>
                </excludes>
            </configuration>
        </plugin>
    </plugins>
</build>
```

## application .yaml项目配置

``` yaml
spring:
  mvc:
    contentnegotiation:
      favor-parameter: true  #开启请求参数内容协商模式 通过在前端往后端递交参数的时候添加format属性 值一般为json或xml 这样能向服务端告诉前端需要返回什么数据。
```

## springboot配置数据校验


## springboot常见异常

### [SpringBoot报406错误，There was an unexpected error (type=Not Acceptable, status=406)](https://blog.csdn.net/qq_39530572/article/details/123087284)

### [Springboot拦截器中无法使用容器中的bean](https://blog.csdn.net/yuyu1067/article/details/110194983)

### Springboot在过滤器或Controller中，需要抛出异常后然后返回成JSON

采取@ControllerAdvice+@ExceptionHandler处理全局异常

1. 首先我们先自定义一个异常类（完成我们需要处理捕获的异常，只需要实现Exception即可。）
2. 选择一个controllerAdvice作为全局异常处理器（在controller和拦截器可用）@RestControllerAdvice则返回的是json | @ControllerAdvice则放回的是视图名称。
3. 然后在任意方法上添加@ExceptionHandler(异常类型)处理异常，来捕获你指定的异常。

```java
@RestControllerAdvice
public class ExceptionController {
    //    处理拦截器中用户没有登录的异常
    // 注意记得检查ExceptionHandler和方法中的参数要相同
    @ExceptionHandler(value = UserToeknException.class)
    public ResponseBean tokenUserLogin(UserToeknException exception) {
        return new ResponseBean(exception.getCode(), exception.getMessage(), exception.getSuccess(), exception.getToekn());
    }
}
```

### springboot给静态属性注入得到的值为null

般需要在static方法里调用注入进来的service，因为是静态方法，所以必须声明该service也必须是static的，这时候你会发现注入不进来，会报null指针。
>处理办法
通过set方法注入,类上需要用@Component
并且使用这个静态类注入要求此类必须是注入组件

``` java
//解决static方法 调用注入对象的方法
    private static UserService userService;

    @Autowired
    public void setUserService(UserService userService) {
        DubboAuthService.userService = userService;
    }
```

### [springboot拦截静态资源怎么处理](https://blog.csdn.net/weixin_44115555/article/details/94215581)

### springboot处理跨域异常（为了处理配置web过滤器遇到问题）

>在springboot中使用此方法。由注入过滤器的方式来处理跨域问题。在服务器中的tomcat8会出现无权限访问的问题(tomcat9中又无事发生) 但我们使用springboot项目就应该采取springboot的方式去处理跨域问题（如拦截器）

为什么postman等测试工具不会遇到跨域问题
>因为使用postman发送请求的时候，每个请求都是独立的
首先，回顾一下跨域的定义。根据MDN Web Docs 里的定义，跨域是指当一个资源从与该资源本身所在的服务器不同的域或端口不同的域或不同的端口请求一个资源时，资源会发起一个跨域 HTTP 请求。

即当一个请求url的协议、域名、端口三者之间任意一个与当前页面url不同即为跨域。
也就是说，正常的跨域情况，是你访问了一个A网站，然后这个网站返回的资源里面，请求了B网站/端口的资源，于是就跨域了。

所以，跨域这个情况只会出现在浏览器页面里，因为实际上是浏览器由于安全原因限制了这些请求的访问。

然而，在postman里面，实际上每发出一个请求，都是在独立请求一个资源，而不是在一个网站返回的页面里，再去请求另外一个网站/端口的资源。自然也就不会造成跨域了。

这是浏览器同源策略导致的，注意这是浏览器规范

出于浏览器的同源策略限制。同源策略（Sameoriginpolicy）是一种约定，它是浏览器最核心也最基本的安全功能，如果缺少了同源策略，则浏览器的正常功能可能都会受到影响。可以说Web是构建在同源策略基础之上的，浏览器只是针对同源策略的一种实现。同源策略会阻止一个域的。javascript脚本和另外一个域的内容进行交互。所谓同源（即指在同一个域）就是两个页面具有相同的协议（protocol），主机（host）和端口号（port)

并不代表 POSTMAN等API工具 需要遵守这个浏览器的策略。

1.springboot处理跨域的方式

``` java
@Configuration
public class CorsConfig implements WebMvcConfigurer {
     @Override
    public void addCorsMappings(CorsRegistry registry) {
        WebMvcConfigurer.super.addCorsMappings(registry);
        // addMapping表示加域的url射。
        // /**表示所有的面都支特辟域，一般会配业务处理的url，而非全部
        // allowedMethods表示许的http事件 (POST. GET. DELETE...)
        // allowedOrigins表示允许跨的域名，生产环境中一定会配成限定的域名
        // 比如前端名是huhuiyu.top, 而springboot后名api.huhuiyu.top
        // 那应该允许的域名应该配成huhuiyu.top,或者相对险的*.huhuiyu.top
        // allowCredentials表示是要开认证
        registry.addMapping("/**").allowedMethods("*").allowedOrigins("*").allowCredentials(false);
    }
  }   
```

2.springboot处理跨域方式（拦截器）

```java
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(userInterceptor).addPathPatterns("/**").excludePathPatterns("/", "/user/auth/login", "/user/auth/reg", "/tool/sendUserEmailCode", "/user/auth/findPwdByEmail", "/user/auth/findPwdByPhone");
        registry.addInterceptor(new HandlerInterceptor() {

            @Override
            public boolean preHandle(HttpServletRequest request, HttpServletResponse resp, Object handler) throws Exception {
                resp.setHeader("Access-Control-Allow-Credentials", "false");
                //设置受支持请求标头
                resp.setHeader("Access-Control-Allow-Headers", "*");
                //响应标头指定响应访问所述资源到时允许的一种或多种方法
                resp.setHeader("Access-Control-Allow-Methods", "*");
                // 响应标头指定 指定可以访问资源的URI路径
                resp.setHeader("Access-Control-Allow-Origin", "*");
                //设置 缓存可以生存的最大秒数
                resp.setHeader("Access-Control-Max-Age", "3600");
                return true;
            }
        }).addPathPatterns("/**");
        WebMvcConfigurer.super.addInterceptors(registry);
    }
```

3.配置了跨域处理器，任然ajax有跨域问题（问题原因一）

>此时会出现跨域异常:原因：在nginx配置服务器中tomcat的8080端口http://127.0.0.1:8080/message/会被配置为/message/所以我们访问服务器中的message项目不能是由https://chengbelief.top/message而是在访问地址加上https://chengbelief.top/message+接口地址。

```js
// 发送一个ajax请求
        axios({
            method: 'get',
            url: 'https://chengbelief.top/message',
            // data: Qs.stringify({
            //   username: this.username,
            //   age: this.age,
            //   dateTime:  new Date(this.dateTime).getTime()
            // })
          }).then(function (resp) {
            console.log(resp)
          })
```

### 如果springboot的打包方式为war包[多出现的ServletInitializer类详解](https://blog.csdn.net/qq_43799161/article/details/125315579?spm=1001.2101.3001.6650.9&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-9-125315579-blog-118605060.pc_relevant_multi_platform_whitelistv4&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-9-125315579-blog-118605060.pc_relevant_multi_platform_whitelistv4&utm_relevant_index=12)