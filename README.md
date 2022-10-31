# maven_settings
[toc]

maven global settings for closed source and open source projects

### 0、settings.xml

参考：https://maven.apache.org/settings.html

完整配置：[settings.xml](settings.xml)

settings.xml配置文件路径：

```
~/.m2/settings.xml
```

缺省本地仓库路径：

```
~/.m2/repository/
```

查看生效的settings.xml文件：

```shell
$ mvn -X clean | grep "settings"
[DEBUG]   Imported: org.apache.maven.settings < plexus.core
[DEBUG] Reading global settings from /usr/local/bin/apache-maven-3.6.3/conf/settings.xml
[DEBUG] Reading user settings from /Users/gongpengjun/.m2/settings.xml
```

查看最终生效的配置内容：

```shell
$ mvn help:effective-settings
```

### 1、profiles and activeProfiles

```xml
<settings>
  <profiles>
    <profile>
      <id>privatemaven</id>
    </profile>
    <profile>
      <id>alimaven</id>
    </profile>
  </profiles>
  <activeProfiles>
    <activeProfile>privatemaven</activeProfile>
    <activeProfile>alimaven</activeProfile>
  </activeProfiles>
</settings>
```

定义两个profile：

- 一个profile的id为 privatemaven
- 一个profile的id为 alimaven

activeProfiles激活两个profile：privatemaven和alimaven

### 2、profile - privatemaven

#### 2.1、repositories and pluginRepositories

```xml
<profile>
  <id>privatemaven</id>
  <repositories>
    <repository>
    </repository>
  </repositories>
  <pluginRepositories>
    <pluginRepository>
    </pluginRepository>
  </pluginRepositories>
</profile>
```

profile内可以定义多个普通仓库repositories和多个插件仓库pluginRepositories

普通仓库分为两个：一个release版仓库、一个snapshot版仓库

```xml
<repositories>
  <repository>
    <id>central</id>
    <name>libs-release</name>
    <url>http://artifactory.${PRIVATE_MAVEN_HOST}/artifactory/libs-release</url>
    <snapshots><enabled>true</enabled></snapshots>
  </repository>
  <repository>
    <snapshots />
    <id>snapshots</id>
    <name>libs-snapshot</name>
    <url>http://artifactory.${PRIVATE_MAVEN_HOST}/artifactory/libs-snapshot</url>
  </repository>
</repositories>
```

插件仓库分为两个：一个release版插件仓库、一个snapshot版插件仓库

```xml
<pluginRepositories>
  <pluginRepository>
    <id>central</id>
    <name>plugins-release</name>
    <url>http://artifactory.${PRIVATE_MAVEN_HOST}/artifactory/plugins-release</url>
    <snapshots><enabled>true</enabled></snapshots>
  </pluginRepository>
  <pluginRepository>
    <snapshots />
    <id>snapshots</id>
    <name>plugins-snapshot</name>
    <url>http://artifactory.${PRIVATE_MAVEN_HOST}/artifactory/plugins-snapshot</url>
  </pluginRepository>
</pluginRepositories>
```

#### 2.2、private repositories username and password

顶级字段servers配置私有仓库的用户名和密码，具体账号密码咨询私有maven仓库管理员

```xml
<settings>
  <servers>
    <server>
      <username>${PRIVATE_MAVEN_USERNAME}</username>
      <password>${PRIVATE_MAVEN_PASSWORD}</password>
      <id>central</id>
    </server>
    <server>
      <username>${PRIVATE_MAVEN_USERNAME}</username>
      <password>${PRIVATE_MAVEN_PASSWORD}</password>
      <id>snapshots</id>
    </server>
  </servers>
  <profiles>
    <profile>
      <id>privatemaven</id>
    </profile>
    <profile>
      <id>alimaven</id>
    </profile>
  </profiles>
  <activeProfiles>
    <activeProfile>privatemaven</activeProfile>
    <activeProfile>alimaven</activeProfile>
  </activeProfiles>
</settings>
```

### 3、profile - alimaven

参考：https://developer.aliyun.com/mvn/guide

```xml
<profile>
  <id>alimaven</id>
  <repositories>
      <repository>
          <id>public</id>
          <url>https://maven.aliyun.com/repository/public</url>
          <releases><enabled>true</enabled></releases>
          <snapshots><enabled>true</enabled></snapshots>
      </repository>
  </repositories>
  <pluginRepositories>
      <pluginRepository>
          <id>public</id>
          <url>https://maven.aliyun.com/repository/public</url>
          <releases><enabled>true</enabled></releases>
          <snapshots><enabled>true</enabled></snapshots>
      </pluginRepository>
  </pluginRepositories>
</profile>
```

