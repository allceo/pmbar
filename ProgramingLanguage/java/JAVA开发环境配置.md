JAVA开发环境配置
====================
----------
程序准备
--------------
 - 下载Eclipse IDE for Java EE Developers
   https://www.eclipse.org/downloads/ 
 - 下载Apache Maven（Maven 3.2.1 (Binary zip)） 
   http://maven.apache.org/download.cgi
 - 下载JDK(jdk-7u51-windows-i586.exe)
   http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html
 - 下载Gaea
   https://github.com/58code/Gaea

环境配置
--------------
配置环境变量

 - 在"系统变量"中新建 JAVA_HOME
JAVA_HOME= C:\Program Files\Java\jdk1.7.0_51(JDK安装位置)

 - 在"系统变量"中添加 JAVA_JRE_HOME
JAVA_JRE_HOME=%JAVA_HOME%\jre

 - 在"系统变量"中添加 JRE_HOME
JRE_HOME= C:\Program Files\Java\jre7(你的安装位置)

 - 在"系统变量"中添加 CLASSPATH
CLASSPATH=.;%JAVA_HOME%\lib;%JAVA_HOME%\lib\tools.jar;%JAVA_HOME%\lib\dt.jar;%JRE_HOME%\lib;%JRE_HOME%\lib\rt.jar;%JAVA_JRE_HOME%\lib;%JAVA_JRE_HOME%\lib\rt.jar

 - 在"系统变量"中添加 Path（！注意，如果原来有的话只打开即可，然后在原来的内容后面加上分号并复制在后面即可，否则可能会造成系统不稳定）
%JAVA_HOME%\bin;%JRE_HOME%\bin;%JAVA_JRE_HOME%\bin;%MAVEN_HOME%\bin;

 - 在"系统变量"中添加MAVEN_HOME
MAVEN_HOME=D:\Program Files\eclipse\apache-maven-3.2.1

Eclipse配置

保存D:\Personal\.m2目录下settings.xml（有些人可能是在C:\Users\Administrator目录下）

    <settings>
    <servers>
      <server>
        <id>releases</id>
        <username>hofantech</username>
        <password>hofantech</password>
      </server>
      <server>
        <id>Snapshots</id>
        <username>hofantech</username>
        <password>hofantech</password>
      </server>
    </servers>
     
      <mirrors>
        <mirror>
          <!--This sends everything else to /public -->
          <id>nexus</id>
          <mirrorOf>*</mirrorOf>
          <url>http://build.hofan-sz.com:8081/nexus/content/groups/public/</url>
        </mirror>
    	<mirror>
          <!--This is used to direct the public snapshots repo in the 
              profile below over to a different nexus group -->
          <id>nexus-public-snapshots</id>
          <mirrorOf>public-snapshots</mirrorOf>
          <!--<url>http://10.58.120.19:8058/nexus/content/groups/public-snapshots</url>-->
          <url>http://build.hofan-sz.com:8081/nexus/content/repositories/snapshots/</url>
        </mirror>
      </mirrors>
     
      <profiles>
        <profile>
          <id>development</id>
          <repositories>
            <repository>
              <id>central</id>
              <url>http://central</url>
              <releases><enabled>true</enabled></releases>
              <snapshots><enabled>true</enabled></snapshots>
            </repository>
          </repositories>
         <pluginRepositories>
            <pluginRepository>
              <id>central</id>
              <url>http://central</url>
              <releases><enabled>true</enabled></releases>
              <snapshots><enabled>true</enabled></snapshots>
            </pluginRepository>
          </pluginRepositories>
        </profile>
        <profile>
          <!--this profile will allow snapshots to be searched when activated-->
          <id>public-snapshots</id>
          <repositories>
            <repository>
              <id>public-snapshots</id>
              <url>http://public-snapshots</url>
              <releases><enabled>false</enabled></releases>
              <snapshots><enabled>true</enabled></snapshots>
            </repository>
          </repositories>
         <pluginRepositories>
            <pluginRepository>
              <id>public-snapshots</id>
              <url>http://public-snapshots</url>
              <releases><enabled>false</enabled></releases>
              <snapshots><enabled>true</enabled></snapshots>
            </pluginRepository>
          </pluginRepositories>
        </profile>
      </profiles>
      <activeProfiles>
        <activeProfile>development</activeProfile>
        <activeProfile>public-snapshots</activeProfile>
      </activeProfiles>
     
    </settings>

在库中搜索gaea
http://build.hofan-sz.com:8081/nexus/index.html#welcome

    <dependency>
      <groupId>fakepath</groupId>
      <artifactId>com.bj58.spat.gaea.client</artifactId>
      <version>1.0.0</version>
    </dependency>

将上述内容加入到项目根目录下的pom.xml

修改Maven的Local Repository目录
--------------
直接修改配置
D:\Program Files\eclipse\apache-maven-3.2.1\conf\settings.xml
添加配置为：

    <localRepository>D:\Personal\.m2\repository</localRepository>
在Eclipse的配置界面修改
首选项》Maven》User Settings

让项目编译时自动打包生成的Jar文件
--------------
 在项目的根目录下增加deploy-all.xml文件，内容如下：
    <?xml version="1.0" encoding="UTF-8"?>
    
    <assembly
    	xmlns="http://maven.apache.org/plugins/maven-assembly-plugin/assembly/1.1.0"
    	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    	xsi:schemaLocation="http://maven.apache.org/plugins/maven-assembly-plugin/assembly/1.1.0 http://maven.apache.org/xsd/assembly-1.1.0.xsd">
    
    	<id>release</id>
    	<formats>
    		<format>zip</format>
    	</formats>
    	<baseDirectory>/</baseDirectory>
    	<fileSets>
    		<fileSet>
    			<directory>pricesystem-common-dal/target
    			</directory>
    			<outputDirectory>/</outputDirectory>
    		</fileSet>
    		<fileSet>
    			<directory>pricesystem-common-util/target
    			</directory>
    			<outputDirectory>/</outputDirectory>
    		</fileSet>
    		<fileSet>
    			<directory>pricesystem-core-service/target
    			</directory>
    			<outputDirectory>/</outputDirectory>
    		</fileSet>
    		<fileSet>
    			<directory>pricesystem-service-facade/target
    			</directory>
    			<outputDirectory>/</outputDirectory>
    		</fileSet>
    		<fileSet>
    			<directory>pricesystem-service-impl/target
    			</directory>
    			<outputDirectory>/</outputDirectory>
    		</fileSet>
    		
    		
    	</fileSets>
    	<dependencySets>
    		<dependencySet>
    			<scope>runtime</scope> 
    		</dependencySet>
    	</dependencySets>
    </assembly>
    
修改根目录下的pom.xml文件，在build的plugins节点下增加项目如下

    <plugin>
    	<artifactId>maven-assembly-plugin</artifactId>
    	<version>2.2-beta-5</version>
    	<executions>
    		<execution>
    			<id>release</id>
    			<phase>package</phase>
    			<goals>
    				<goal>single</goal>
    			</goals>
    			<configuration>
    				<descriptors>
    					<descriptor>deploy-all.xml</descriptor>
    				</descriptors>
    			</configuration>
    		</execution>
    	</executions>
    </plugin>