# Java 和 Scala

参考资料:

1. []

# 打包（IDEA、Maven）和命令行操作

## 打包

### IDEA 打包 

IDEA 打包有两个选项，可以把依赖的`Jar`看成源代码打进去`Jar`包，另一个是empty的，然后自定义的`Jar`。

![image](https://user-images.githubusercontent.com/18718050/161362498-44bec339-3009-467c-99a4-81e03fee1923.png)


### Maven 打包

- Maven Dependency Plugin

  Dependency插件，可以将程序所依赖的`Jar`包单独copy到指定文件夹内，使得打包出来源程序`Jar`文件很小,一般`KB`级别。可以将依赖的`Jar`包，放到服务器端，每次只更新源代码的`Jar`包就可以
  
  参考 [maven-dependency-plugin](https://maven.apache.org/plugins/maven-dependency-plugin/index.html#apache-maven-dependency-plugin)，
  最下面有使用的例子。
  
  使用方法：
  ```xml
  <build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-dependency-plugin</artifactId>
            <version>3.3.0</version>
            <executions>
                <execution>
                    <id>copy-dependencies</id>
                    <phase>package</phase>
                    <goals>
                        <goal>copy-dependencies</goal>
                    </goals>
                    <configuration>
                        <outputDirectory>${project.build.directory}/alternateLocation</outputDirectory>
                        <overWriteReleases>false</overWriteReleases>
                        <overWriteSnapshots>false</overWriteSnapshots>
                        <overWriteIfNewer>true</overWriteIfNewer>
                    </configuration>
                </execution>
            </executions>
        </plugin>
      </plugins>
  </build>
  ```
  
  或者
  
  ```xml
  <build>
    <plugins>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-dependency-plugin</artifactId>
        <version>3.3.0</version>
        <executions>
          <execution>
            <id>copy</id>
            <phase>package</phase>
            <goals>
              <goal>copy</goal>
            </goals>
          </execution>
        </executions>
        <configuration>
          <artifactItems>
            <artifactItem>
              <groupId>junit</groupId>
              <artifactId>junit</artifactId>
              <version>3.8.1</version>
              <type>jar</type>
              <overWrite>false</overWrite>
              <outputDirectory>${project.build.directory}/alternateLocation</outputDirectory>
              <destFileName>optional-new-name.jar</destFileName>
            </artifactItem>
          </artifactItems>
          <outputDirectory>${project.build.directory}/wars</outputDirectory>
          <overWriteReleases>false</overWriteReleases>
          <overWriteSnapshots>true</overWriteSnapshots>
        </configuration>
      </plugin>
    </plugins>
  </build>
  ```
  
  这两个区别在于`<goal>`标签不同，`copy-dependencies`不需要`<artifactItem>`这个标签，而`copy`需要，否则会报错。
  
  `Failed to execute goal org.apache.maven.plugins:maven-dependency-plugin:3.3.0:copy (copy) on project jdq4_demo: Either artifact or artifactItems is required `
  
  运行方式：`java -classpath $name:./jdq4_demo.jar jdq.ProducerOffLine `，其中`$name`是引用的`Jar`包，用冒号隔开(Linux上)。
  
  注意： 最好不要在maven中指定main函数，然后使用 `java -jar xxx.jar`的方式运行`jar`包，因为没有办法再加载外部`Jar`了。[参照详情](https://blog.csdn.net/w47_csdn/article/details/80254459)
  ![image](https://user-images.githubusercontent.com/18718050/161362288-91990f3e-7539-4136-be46-be601fb52632.png)

  
- Maven Assembly Plugin

```xml

            <plugin>
                <artifactId>maven-assembly-plugin</artifactId>
                <configuration>
                    <descriptorRefs>
                        <descriptorRef>jar-with-dependencies</descriptorRef>
                    </descriptorRefs>
                </configuration>

                <executions>
                    <execution>
                        <id>make-assembly</id>
                        <phase>package</phase>
                        <goals>
                            <goal>assembly</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
```

Assembly Plugin 可以将所有的依赖打进作为源代码进行打包，打包出来的效果就是`xxx-1.0-SNAPSHOT.jar`.`xxx-1.0-SNAPSHOT-jar-with-dependencies.jar`

- Maven shade Plugin

> The Shade Plugin has a single goal:
> 
> shade:shade is bound to the package phase and is used to create a shaded jar.

```xml

            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-shade-plugin</artifactId>
                <version>3.3.0</version>
                <executions>
                    <execution>
                        <phase>package</phase>
                        <goals>
                            <goal>shade</goal>
                        </goals>
                        <configuration>
                            <shadedArtifactAttached>true</shadedArtifactAttached>
                            <!-- 这里 出来的jar包名称会是 xxx-1.0-SNAPSHOT-jackofall.jar 的形式 -->
                            <shadedClassifierName>jackofall</shadedClassifierName> <!-- Any name that makes sense -->
                        </configuration>
                    </execution>
                </executions>
            </plugin>
  ```
  
  会出来两个两个`Jar`包，一个是带有`xxx-1.0-SNAPSHOT.jar`，一个是`xxx-1.0-SNAPSHOT-jackofall.jar`。
  
  运行： `java -classpath ./xxx-1.0-SNAPSHOT-jackofall.jar jdq.ProducerOffLine ` 就可以。

# 多线程

# 集合类

# 醍醐灌顶的思考

# 遇到的问题

1. ` Unsupported major.minor version 52.0` ？

高JDK版本编译，然后低版本运行。

`java.lang.UnsupportedClassVersionError` happens because of a higher JDK during compile time and lower JDK during runtime.

