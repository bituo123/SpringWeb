Maven 仓库，依赖管理，POM，插件，生命周期
POM定义项目类型，名字，依赖关系，定制插件
<groupId>com.youkeda.course</groupId>
<artifactId>app</artifactId>
<packaging>jar</packaging>
<version>1.0-SNAPSHOT</version>
以上是Maven坐标
打包格式，jar，war，ear，pom
版本不稳定用SNAPSHOT，稳定RELEASE
版本号第一位主版本号，第二位新增功能，第三位bug修改版本
Annotation注解，一般放在类前，方法前，属性前，参数前
Retention生命周期
@Retention(RetentionPolicy.RUNTIME)运行期间有效
Documented把注解元素包含到JavaDoc种，
interface声明当前Java是Annotation
Bean是对象实例后的结果
Service，Component，Controller，Repository
一般调用Service服务都需要加上继承类实例化，并且用到哪个服务需要set，但是给初始服务继承类加上Service,再给使用的服务属性加上Autowired就不需要实例化和set了
java文件的jar包文件读取
InputStream in = Test.class.getClassLoader().getResourceAsStream("data.json");
从当前运行的类加载器实例中查找文件，
在Spring中通过
@Autowired
private ResourceLoader loader;
来提供任意文件的读写，下面是方法实现，方法名称getContent(name)
InputStream in = loader.getResource(name).getInputStream();
            return IOUtils.toString(in,"utf-8");
下面是调用
fileService.getContent("classpath:data/urls.txt");
参数可以是classpath:data/urls.txt，file:mywork/readme.md，https://www.zhihu.com/question/34786516/answer/822686390
@PostConstruct这个注释下面的方法会在bean启动后自动执行
MVC
在类上面加上@Controller
路由在方法上加上
@RequestMapping
get参数
@RequestMapping("/songlist")
    public String index( @RequestParam("id") String id)
多个get参数
@RequestMapping("/songlist")
  public String index(@RequestParam("id") String id,  @RequestParam("pageNum") int pageNum)
只限定get请求
@GetMapping("/songlist")
public String index(@RequestParam("id") String id,@RequestParam("pageNum") int pageNum){
非必须传递参数
@GetMapping("/songlist")
public String index(@RequestParam(name="pageNum",required = false) int pageNum,@RequestParam("id") String id){
输出JSON数据或者字符串
方法前面加上@ResponseBody
也可以返回Java对象转化为JSON的形式return users.get(id);
Thymeleaf
数据，模板，引擎渲染
添加依赖
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
引入Model
import org.springframework.ui.Model;
传递对象model.addAttribute("songList",songList);
模板文件路径
src/main/resources/templates
<html lang="en" xmlns:th="http://www.thymeleaf.org">
html使用<h1 th:text="${songList.name}"></h1>
Thymeleaf循环
<ul th:each="song : ${songs}">
  <li th:text="${song.name}">歌曲名称</li>
</ul>
新的参数
th:each="song,it: ${songs}"
it.index，索引，从0开始
it.count从1开始
it.size获取长度
it.current获取当前迭代的变量
it.even/odd布尔值，当前循环是否是偶数/奇数，从0开始
it.first是否是第一个
it.last是否是最后一个
字符串处理
"'00:00/'+${totalTime}"拼接，用''包裹是把他转化为字符串
拼接优化
"|00:00/${totalTime}|"
数据优化
处理LocalDate类添加依赖
<dependency>
  <groupId>org.thymeleaf.extras</groupId>
  <artifactId>thymeleaf-extras-java8time</artifactId>
  <version>3.0.4.RELEASE</version>
</dependency>
添加一个新的工具类temporal
工具类的使用，下面是Date类
<p th:text="${#dates.format(dateVar, 'yyyy-MM-dd')}"></p>
<p th:text="${#dates.format(dateVar, 'yyyy年MM月dd日')}"></p>
<p th:text="${#dates.format(dateVar, 'yyyy-MM-dd HH:mm:ss')}"></p>
<p th:text="${#dates.format(dateVar, 'yyyy年MM月dd日 HH时mm分ss秒')}"></p>
用LocalDate/LocalDateTime就需要把dates替换为temporals
Strings字符处理
${#strings.toUpperCase(name)}字符全部改成大写
${#strings.toLowerCase(name)}小写
${#strings.arrayJoin(array,',')}字符数组合并，用，为间隔
${#strings.arraySplit(str,',')}字符串分割为数组用，为分隔符，没有，就作为整体一个长度的数组
${#strings.trim(str)}去空格
${#strings.length(str)}得到字符串长度，支持集合类的长度
${#strings.equals(str1,str2)}比较两个字符串是否相等
${#strings.equalsIgnoreCase(str1,str2)}忽略大小写后比较是否相等
${#strings.isEmpty(name)}是否为空
${#strings.arrayIsEmpty(name)}
${#strings.listIsEmpty(name)}
${#strings.contains(name,'abc')}是否包含
${#strings.containsIgnoreCase(name,'abc')}
${#strings.startsWith(name,'abc')}
${#strings.endsWith(name,'abc')}
更多使用技巧
#messages
#urls/uris
#conversions
#dates
#calendars
#numbers
#strings
#objects
#bools
#arrays
#lists
#sets
#maps
#aggregates
#ids
https://www.jianshu.com/p/d8bc610c4400
https://www.thymeleaf.org/apidocs/thymeleaf/3.0.11.RELEASE/org/thymeleaf/expression/Arrays.html
内联表达式
<span>Hello [[${msg}]]</span>[[]]只是用来替换text的
条件语句
<span th:if="${user.sex == 'male'}">男</span>
<span th:unless="${user.sex == 'male'}">女</span>
以下也为true
值非空，值非0的数字，值字符串，但是不是false，off，no
值不是boolean值，数字，character或者字符串
Thymeleaf表单
添加书的请求
用到了post请求，前端的模板为label+input，其中input加上不同的name
<form action="/book/save" method="POST">
后端用
@PostMapping("/book/save")
  public String saveBook(Book book){
    books.add(book);
    return "saveBookSuccess";
  }
Validation验证
JSR 380
<dependency>
  <groupId>jakarta.validation</groupId>
  <artifactId>jakarta.validation-api</artifactId>
  <version>2.0.1</version>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-validation</artifactId>
</dependency>
JSR可以直接在类的属性上面添加注解来进行校验
NotNull
AssertTrue是否为true
Size约定大小
Min最小长度字符串
Max最大
Email是否为邮箱
NotEmpty不允许为null或者空，字符串集合，Map数组List
NotBlank不允许为null和空格
使用规范Min(value = 18, message = "你的年龄必须大于等于18岁")
执行校验
@PostMapping("/user/save")
    public String saveUser(@Valid User user, BindingResult errors) {
        if (errors.hasErrors()) {
            // 如果校验不通过，返回用户编辑页面
            return "user/addUser";
        }
        // 校验通过，返回成功页面
        return "user/addUserSuccess";
    }
显示错误信息
后端添加
User user = new User();
model.addAttribute("user",user);
前端添加
<form action="/user/save" th:object="${user}" method="POST">
css添加
.error {
  color: red;
}
单独字段属性添加，如果这个属性有错误则就加上这个错误类
<div th:classappend="${#fields.hasErrors('name')} ? 'error' : ''">
加上错误信息
<p th:if="${#fields.hasErrors('age')}" th:errors="*{age}"></p>
th:object="${user}"设置过了，所以可以用*{age}代替
一般错的时候，我们还希望显示上一次输入的内容
<div th:classappend="${#fields.hasErrors('age')} ? 'error' : ''">
  <label>年龄:</label>
  <input type="text" th:field="*{age}" />
  <p th:if="${#fields.hasErrors('age')}" th:errors="*{age}"></p>
</div>
页面布局
布局组件，使用th:include + th:replace
首先创建一个组件模板layout，然后加上header和container和footer
给container我们加上
<div class="container" th:include="::content">页面正文内容</div>
这样当我们套用的时候只有container不一样，剩下的组件都一样
现在我们要在list里面使用
<html lang="en" xmlns:th="http://www.thymeleaf.org" th:replace="layout">
开头加上replace，后面的layout是组件文件
<div th:fragment="content">这里是正文部分，也就是不套用组件部分
使用Logger
在application.properties文件中加上
logging.level.root=info
也可以为不同的包定义不同类别
一般日志优先级类别有error，warn，info，debug
设置高级优先级后更低的就不再输出了
private static final Logger LOG = LoggerFactory.getLogger(SongListControl.class);
LOG.info("SongListControl 启动啦");
info和级别一一对应
自定义配置application.properties
song.name=God is a girl
在对象类里的属性添加
@Value("${song.name}")
private String songName;
Cookie读取
@RequestMapping("/songlist")
public Map index(HttpServletRequest request) {
  Map returnData = new HashMap();
  returnData.put("result", "this is song list");
  returnData.put("author", songAuthor);
  Cookie[] cookies = request.getCookies();
  returnData.put("cookies", cookies);
  return returnData;
}
使用注解读取Cookie
@RequestMapping("/songlist")
public Map index(@CookieValue("JSESSIONID") String jSessionId) {
  Map returnData = new HashMap();
  returnData.put("result", "this is song list");
  returnData.put("author", songAuthor);
  returnData.put("JSESSIONID", jSessionId);
  return returnData;
}
写Cookie
@RequestMapping("/songlist")
public Map index(HttpServletResponse response) {
  Map returnData = new HashMap();
  returnData.put("result", "this is song list");
  returnData.put("name", songName);
  Cookie cookie = new Cookie("sessionId","CookieTestInfo");
  // 设置的是 cookie 的域名，就是会在哪个域名下生成 cookie 值
  cookie.setDomain("youkeda.com");
  // 是 cookie 的路径，一般就是写到 / ，不会写其他路径的
  cookie.setPath("/");
  // 设置cookie 的最大存活时间，-1 代表随浏览器的有效期，也就是浏览器关闭掉，这个 cookie 就失效了。
  cookie.setMaxAge(-1);
  // 设置是否只能服务器修改，浏览器端不能修改，安全有保障
  cookie.setHttpOnly(false);
  response.addCookie(cookie);
  returnData.put("message", "add cookie successful");
  return returnData;
}
Session读取
首先登陆的信息类
public class UserLoginInfo implements Serializable {
  private String userId;
  private String userName;
}
读取
@RequestMapping("/songlist")
public Map index(HttpServletRequest request, HttpServletResponse response) {
  Map returnData = new HashMap();
  returnData.put("result", "this is song list");
  // 取得 HttpSession 对象
  HttpSession session = request.getSession();
  // 读取登录信息
  UserLoginInfo userLoginInfo = (UserLoginInfo)session.getAttribute("userLoginInfo");
  if (userLoginInfo == null) {
    // 未登录
    returnData.put("loginInfo", "not login");
  } else {
    // 已登录
    returnData.put("loginInfo", "already login");
  }
  return returnData;
}
写操作
@RequestMapping("/loginmock")
public Map loginMock(HttpServletRequest request, HttpServletResponse response) {
  Map returnData = new HashMap();
  // 假设对比用户名和密码成功
  // 仅演示的登录信息对象
  UserLoginInfo userLoginInfo = new UserLoginInfo();
  userLoginInfo.setUserId("12334445576788");
  userLoginInfo.setUserName("ZhangSan");
  // 取得 HttpSession 对象
  HttpSession session = request.getSession();
  // 写入登录信息
  session.setAttribute("userLoginInfo", userLoginInfo);
  returnData.put("message", "login successful");
  return returnData;
}
Session配置
@Configuration
public class SpringHttpSessionConfig {
  @Bean
  public TestBean testBean() {
    return new TestBean();
  }
}
Configuration表示这是配置类
Bean会把方法返回的对象实例注册成Bean
添加依赖
<dependency>
    <groupId>org.springframework.session</groupId>
    <artifactId>spring-session-core</artifactId>
</dependency>
配置类
@Configuration
@EnableSpringHttpSession
public class SpringHttpSessionConfig {
  @Bean
  public CookieSerializer cookieSerializer() {
    DefaultCookieSerializer serializer = new DefaultCookieSerializer();
    serializer.setCookieName("JSESSIONID");
    // 用正则表达式配置匹配的域名，可以兼容 localhost、127.0.0.1 等各种场景
    serializer.setDomainNamePattern("^.+?\\.(\\w+\\.[a-z]+)$");
    serializer.setCookiePath("/");
    serializer.setUseHttpOnlyCookie(false);
    // 最大生命周期的单位是秒
    serializer.setCookieMaxAge(24 * 60 * 60);
    return serializer;
  }

  // 当前存在内存中
  @Bean
  public MapSessionRepository sessionRepository() {
    return new MapSessionRepository(new ConcurrentHashMap<>());
  }
}
其中EnableSpringHttpSession开启Session
CookieSerializer读写Cookies的SessionId信息
MapSessionRepository是Session信息存储在服务器的仓库
Request拦截器
HandlerInterceptor拦截器，主要用于拦截没有登陆的请求
创建拦截器
1 Controller方法执行之前，是否登陆的验证放在preHandle里面处理
2 Controller执行后，比如记录日志，统计方法执行，就在postHandle中处理
3 整个请求完成后，不常用，在afterCompletion中处理
public class InterceptorDemo implements HandlerInterceptor {

  // Controller方法执行之前
  @Override
  public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
    // 只有返回true才会继续向下执行，返回false取消当前请求
    return true;
  }
  //Controller方法执行之后
  @Override
  public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler,
      ModelAndView modelAndView) throws Exception {
  }
  // 整个请求完成后（包括Thymeleaf渲染完毕）
  @Override
  public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
  }
}
实现WebMvcConfigurer，实现addInterceptors方法用于管理拦截器
设置拦截器范围addPathPatterns("/**")
也可以用排除excludePathPatterns()
@Configuration
public class WebAppConfigurerDemo implements WebMvcConfigurer {
  @Override
  public void addInterceptors(InterceptorRegistry registry) {
    // 多个拦截器组成一个拦截器链
    // 仅演示，设置所有 url 都拦截
    registry.addInterceptor(new UserInterceptor()).addPathPatterns("/**");
  }
}
通常拦截器会放在一个包比如interceptor用于管理拦截器放在config
实战用户登陆
@PostMapping(path = "/authenticate")
public String loginAction(@RequestParam String name, @RequestParam String password) {
  return user.toString();
}
<form action="" method="post">
页面跳转
