# 从maven运行docker

## 需要插件

* spring-boot-maven-plugin
* docker-maven-plugin
* gmavenplus-plugin



* pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>

	<groupId>guru.springframework</groupId>
	<artifactId>spring-boot-docker</artifactId>
	<version>0.0.3-SNAPSHOT</version>
	<packaging>jar</packaging>

	<name>spring-boot-docker</name>
	<description>Spring Framework Guru Docker Course</description>

	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>1.5.1.RELEASE</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>

	<properties>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
		<java.version>1.8</java.version>

		<!--设置镜像前缀名-->
		<docker.image.prefix>springframeworkguru</docker.image.prefix>

		<!--设置镜像名-->
		<docker.image.name>springbootdocker</docker.image.name>

	</properties>

	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-thymeleaf</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-devtools</artifactId>
		</dependency>

		<!--testing deps-->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
	</dependencies>
	<build>
    <plugins>
        <!-- docker的maven插件，官网：https://github.com/spotify/docker-maven-plugin -->
        <plugin>
            <groupId>com.spotify</groupId>
            <artifactId>docker-maven-plugin</artifactId>
            <version>1.1.1</version>
            <configuration>
                <!-- 注意imageName一定要是符合正则[a-z0-9-_.]的，否则构建不会成功 -->
                <!-- 详见：https://github.com/spotify/docker-maven-plugin    Invalid repository name ... only [a-z0-9-_.] are allowed-->
				<!-- 镜像名 -->
                <imageName>${docker.image.prefix}-${project.artifactId}:${project.version}</imageName>
                <!-- 指定Dockerfile所在的路径 -->
                <dockerDirectory>${basedir}/src/main/resources</dockerDirectory>
                <resources>
				    <!--指定jar包所在的路径，docker maven插件会把dockerfile复制到jar包坐在路径进行解析构建镜像 -->
                    <resource>
				     	<!--项目根目录-->
                        <targetPath>/</targetPath>
                        <!--默认是target-->
                        <directory>${project.build.directory}</directory>
                        <!--默认是${project.artifactId}-${project.version}，这就要求我们打包的时候必须是指定打包的jar包名字是这个，不然会找不到-->
                        <include>${project.build.finalName}.jar</include>
                    </resource>
                </resources>
            </configuration>
        </plugin>
    </plugins>
</build>

</project>
```

