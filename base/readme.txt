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
