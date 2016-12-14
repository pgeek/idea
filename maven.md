## 依赖的继承

### 怎么覆盖parent中dependency的scope

例如在parent的<dependencyManagement>中声音的依赖如下：

```
<dependencyManagement>
	<dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>${javax.servlet-api.version}</version>
            <scope>provided</scope>
    </dependency>
```

servlet-api的scope为 **provided**

但是有的项目需要这个jar包（比如spark程序），我们需要修改scope为runtime， 该怎么办呢？

在module中声明dependency为：

```
<dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>javax.servlet-api</artifactId>
        <scope>runtime</scope>
</dependency>

```
直接修改scope即可
