## MAVEN

### Maven是什么

 Maven是一个**项目管理工具**，它包含了：

* 一个项目对象模型 (Project Object Model)
* 一组标准集合
* 一个项目生命周期(ProjectLifecycle)
* 一个依赖管理系统(Dependency Management System)
* 和用来运行定义在生命周期阶段(phase)中插件(plugin)目标(goal)的逻辑。

当你使用Maven的时候，你用一个明确定义的项目对象模型来描述你的项目，然后 Maven 可以应用横切的逻辑，
这些逻辑来自一组共享的(或者自定义的)插件。



### Maven的核心概念

#### 约定大于配置

在maven中提供了一些默认的选项，当用户不进行配置的时候，maven将会使用约定好的默认配置对项目进行构建。例如默认的源代码目录在src/main/java下。



#### 插件和目标

一个Maven插件是一个**单个或者多个目标的集合**。

一个目标是**一个明确的任务**，它可以作为单独的目标运行，或者作为一个大的构建的一部分和其它目标一起运行。

> ```
> mvn archetype:create -DgroupId=org.sonatype.mavenbook.ch03 \
>                      -DartifactId=simple \
> ```



#### Maven生命周期

>  mvn package

一个阶段是在被Maven称为“构建生命周期”中的一个步骤。生命周期是包含在一个项目构建中的一系列有序的阶段。



Maven可以支持许多不同的生命周期，但是最常用的生命周期是默认的Maven生命周期，这个生命周期中一开始的一个阶段是验证项目的基本完整性，最后的一个阶段是把一个项目发布成产品。



**插件目标可以附着在生命周期阶段上**。随着Maven沿着生命周期的阶段移动，它会执行附着在特定阶段上的目标。



例如：**当我们运行mvn package，Maven运行到打包为止的所有阶段**，在Maven沿着生命周期一步步向前的过程中，它运行绑定在每个阶段上的所有目标。也可以像下面这样显式的指定一系列插件目标，以得到同样的结果：

```shell
mvn resources:resources \
    compiler:compile \
    resources:testResources \
    compiler:testCompile \
    surefire:test \
	jar:jar
```



maven生命周期给了所有使用maven构建项目的开发者一个**统一的构建标准**，使开发者在不同的项目之间能够基于同一套构建理念对不同的项目进行同样的操作就可以达到构建项目的目的，这种统一的标准无疑节省了大量的开发者的时间。



#### Maven坐标

Maven坐标定义了一组标识，它们可以用来唯一标识一个项目，一个依赖，或者MavenPOM里的一个插件。Maven坐标通常用冒号来作为分隔符来书写，像这样的格式:**groupId:artifactId:packaging:version**。



#### Maven仓库

在Maven中，构件和插件是在它们被需要的时候从远程的仓库取来的。默认的远程仓库可以被替换，或者增加一个你组织维护的自定义Maven仓库的引用。有许多现成的项目允许组织管理和维护公共Maven仓库的镜像。



#### Maven依赖管理

##### 传递性依赖

假如你的项目依赖于一个库，而这个库又依赖于五个或者十个其它的库(就像Spring或者Hibernate那样)。你不必找出所有这些依赖然后把它们写在你的pom.xml里，你只需要加上你直接依赖的那些库，Maven会隐式的把这些库间接依赖的库也加入到你的项目中。Maven也会处理这些依赖中的冲突，同时能让你自定义默认行为，或者排除一些特定的传递性依赖。



### 深入Maven

#### 项目对象模型（POM）



