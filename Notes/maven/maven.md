# maven

依赖管理，项目构建

## 一、配置环境变量

- MAVEN_HOME：安装目录
- path中添加：%MAVEN_HOME%\bin
- 使用maven要求jdk环境变量配置了%JAVA_HOME%

**仓库**：中央仓库 --> 私服 --> 本地仓库

**坐标**：

- groupId：定义当前Maven项目力时组织名称（通常是域名反着写，如：org.mybatis）
- artifactId：定义maven项目名称（通常为模块名称，例如：CRM、SMS）
- version：对应的版本号
- packaging：定义该项目打包方式

## 二、Maven配置

### 1. 本地仓库配置

打开apache-maven-3.5.4\conf目录下的 setting.xml

```xml
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 http://maven.apache.org/xsd/settings-1.0.0.xsd">
  <localRepository>E:\Repository\MavenRepo</localRepository>	//添加该行，中间路径为自定义的maven仓库路径
```

### 2. 镜像仓库配置

在setting.xml中添加阿里镜像

```xml
<mirrors>
	 <!--从上往下按顺序使用的-->
	<mirror>
        <!--镜像id，唯一标识-->
        <id>nexus-aliyun</id>
        <!--对哪个仓库进行镜像，即替代哪个仓库-->
        <mirrorOf>central</mirrorOf>
        <!--镜像名，不重要-->
        <name>Nexus aliyun</name>
        <!--镜像url-->
        <url>http://maven.aliyun.com/nexus/content/groups/public</url> 
    </mirror>
	<mirror>
        <id>huaweicloud</id>
        <mirrorOf>*</mirrorOf>
        <url>https://mirrors.huaweicloud.com/repository/maven/</url>
     </mirror>
	<mirror>
		<id>aliyunmaven</id>
		<mirrorOf>*</mirrorOf>
		<name>阿里云公共仓库</name>
		<url>https://maven.aliyun.com/repository/public</url>
	</mirror>
  </mirrors>
```



## 三、pom文件说明

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" 
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd "> 

    <!-- 父项目的坐标。如果项目中没有规定某个元素的值，那么父项目中的对应值即为项目的默认值。
         坐标包括group ID，artifact ID和 version。 --> 
    <parent> 
        <!-- 被继承的父项目的构件标识符 --> 
        <artifactId>xxx</artifactId>
        <!-- 被继承的父项目的全球唯一标识符 -->
        <groupId>xxx</groupId> 
        <!-- 被继承的父项目的版本 --> 
        <version>xxx</version>
        <!-- 父项目的pom.xml文件的相对路径。相对路径允许你选择一个不同的路径。默认值是../pom.xml。
             Maven首先在构建当前项目的地方寻找父项目的pom，其次在文件系统的这个位置（relativePath位置），
             然后在本地仓库，最后在远程仓库寻找父项目的pom。 --> 
        <relativePath>xxx</relativePath> 
    </parent> 

    <!-- 声明项目描述符遵循哪一个POM模型版本。模型本身的版本很少改变，虽然如此，但它仍然是必不可少的，
         这是为了当Maven引入了新的特性或者其他模型变更的时候，确保稳定性。 --> 
    <modelVersion> 4.0.0 </modelVersion> 
    <!-- 项目的全球唯一标识符，通常使用全限定的包名区分该项目和其他项目。并且构建时生成的路径也是由此生成， 
         如com.mycompany.app生成的相对路径为：/com/mycompany/app --> 
    <groupId>xxx</groupId> 
    <!-- 构件的标识符，它和group ID一起唯一标识一个构件。换句话说，你不能有两个不同的项目拥有同样的artifact ID
         和groupID；在某个特定的group ID下，artifact ID也必须是唯一的。构件是项目产生的或使用的一个东西，Maven
         为项目产生的构件包括：JARs，源码，二进制发布和WARs等。 --> 
    <artifactId>xxx</artifactId> 
    <!-- 项目产生的构件类型，例如jar、war、ear、pom。插件可以创建他们自己的构件类型，所以前面列的不是全部构件类型 --> 
    <packaging> jar </packaging> 
    <!-- 项目当前版本，格式为:主版本.次版本.增量版本-限定版本号 --> 
    <version> 1.0-SNAPSHOT </version> 
    <!-- 项目的名称, Maven产生的文档用 --> 
    <name> xxx-maven </name> 
    <!-- 项目主页的URL, Maven产生的文档用 --> 
    <url> http://maven.apache.org </url> 
    <!-- 项目的详细描述, Maven 产生的文档用。 当这个元素能够用HTML格式描述时（例如，CDATA中的文本会被解析器忽略，
         就可以包含HTML标签）， 不鼓励使用纯文本描述。如果你需要修改产生的web站点的索引页面，你应该修改你自己的
         索引页文件，而不是调整这里的文档。 --> 
    <description> A maven project to study maven. </description> 
    
```

待续······