# 如何打包一个springboot项目
## jar包部署
### 1.打包时将配置文件和静态资源文件排除在外
### 1.1如果没有排除在外，那么配置文件是不可以更改的，每次打包都会被打包进入jar中
![](./assets/2018-08-20-16-52-42.png)
### 1.2很显然配置文件是需要灵活调整的，打包时候最好不要进入war包
* pom配置文件示例
```
<build>
        <resources>
            <resource>
                <directory>src/main/resources</directory>
                <excludes>
                    <!--打包时排除resource下的配置文件排除在jar外-->
                    <exclude>**/*.properties</exclude>
                    <exclude>**/*.yml</exclude>
                    <exclude>**/*.xml</exclude>
                </excludes>
            </resource>
        </resources>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
```
* 效果：
![](./assets/2018-08-20-17-01-11.png)
### 2.配置文件如何从外部获取
## war包部署
* 第一步，jar包改成war包
```
<packaging>war</packaging>
```
* 第二步，引入tomcat依赖
```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-tomcat</artifactId>
    <scope>provided</scope>
</dependency>
```
* 第三步，排除内置的tomcat
```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
    <exclusions>
        <exclusion>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-tomcat</artifactId>
        </exclusion>
    </exclusions>
</dependency>
```
* 第四步，启动类修改,继承SpringBootServletInitializer
```
public class Application extends SpringBootServletInitializer {

    @Override
    protected SpringApplicationBuilder configure(SpringApplicationBuilder builder) {
        return builder.sources(Application.class);
    }
    
    public static void main(String[] args) {
        new SpringApplicationBuilder(Application.class).web(true).run(args);
    }
}
```
