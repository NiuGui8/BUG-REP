&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;项目使用的 maven 构建工具，有一个需求需要用到第三方jar包，在导入 jar 包后，本地运行正常，打包时也正常，但是运行打好的 JAR 包时，程序报错： `java.lang.NoClassDefFoundError.png`

![截图](https://github.com/NiuGui8/BUG-REP/blob/master/IDE/img/NoClassDefFoundError.png)

---

百度，Google 一顿搜，网上的回答大体分为几类：


#### 方式1：dependency 本地jar包
    
	<dependency>
        <groupId>com.hope.cloud</groupId>  <!--自定义-->
        <artifactId>cloud</artifactId>    <!--自定义-->
        <version>1.0</version> <!--自定义-->
        <scope>system</scope> <!--system，类似provided，需要显式提供依赖的jar以后，Maven就不会在Repository中查找它-->
        <systemPath>${basedir}/lib/cloud.jar</systemPath> <!--项目根目录下的lib文件夹下-->
    </dependency> 
 

#### 方式2：编译阶段指定外部lib
     
	<plugin>
     <artifactId>maven-compiler-plugin</artifactId>
     <version>2.3.2</version>
     <configuration>
     <source>1.8</source>
     <target>1.8</target>
     <encoding>UTF-8</encoding>
     <compilerArguments>
     <extdirs>lib</extdirs><!--指定外部lib-->
     </compilerArguments>
     </configuration>
     </plugin>
 

#### 方式3：将外部jar打入本地maven仓库

cmd 进入jar包所在路径，执行以下命令

    mvn install:install-file -Dfile=cloud.jar -DgroupId=com.hope.cloud -DartifactId=cloud -Dversion=1.0 -Dpackaging=jar
 

引入依赖

    <dependency>
    <groupId>com.hope.cloud</groupId>
    <artifactId>cloud</artifactId>
    <version>1.0</version>
    </dependency>

但以上几种对我的项目都不起效果（第三种没有尝试，每次需要这样操作，太麻烦了），我的项目是一个提供服务的 java 项目，以上解决方法可能 web 项目是没问题的（尚未尝试），最后在 Stack Overflow 上找到了解决方案：相当于第一种去掉 `<version></version> <scope></scope>` 两项，
	
	<dependency>
        <groupId>com.hope.cloud</groupId>  <!--自定义-->
        <artifactId>cloud</artifactId>    <!--自定义-->
        <version>1.0</version> <!--自定义-->
    </dependency> 

在这基础上再加上：

		<plugin>
	    <groupId>org.apache.maven.plugins</groupId>
	    <artifactId>maven-install-plugin</artifactId>
	    <version>2.5.2</version>
	    <executions>
	        <execution>
	            <id>install-external</id>
	            <phase>clean</phase>
	            <configuration>
	                <file>${basedir}/lib/mylib-core-0.0.1.jar</file>
	                <repositoryLayout>default</repositoryLayout>
	                <groupId>com.mylib</groupId>
	                <artifactId>mylib-core</artifactId>
	                <version>0.0.1</version>
	                <packaging>jar</packaging>
	                <generatePom>true</generatePom>
	            </configuration>
	            <goals>
	                <goal>install-file</goal>
	            </goals>
	        </execution>
	    </executions>
	</plugin>

加上以上配置后， clean package ，生成的 JAR 包可以正常运行了。
