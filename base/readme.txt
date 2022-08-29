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




