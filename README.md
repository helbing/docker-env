### 介绍

这个`docker-env`项目主要是为了帮助新同事快速搭建项目开发环境而写的。每当新同事加入的时候，往往需要花费不少时间教新同事如何安装环境和一些注意事项，即使有完善的项目文档，也可能存在新同事对后端搭建环境不是很了解的情况，依旧还是要花时间帮他搭建开发环境。利用`docker-env`就能极大的改善这种情况，开发者只需要在`docker-env`项目中写好相关的配置并上传公司的私有代码仓库中。以后每当有新同事加入的时候，只需要让他安装`docker`和下载`docker-env`和相关的项目代码，通过几条简单的命令就能够启动，关闭开发环境等。

### 开发环境

`docker-env`主要参考`laradock`这个项目，之前在公司中采用了`laradock`来搭建项目，然后由于种种原因，最终只能写了`docker-env`。

现在的`docker-env`包含的环境有

- nginx
- maraidb
- php
- redis

### 注意事项

##### 安装php拓展

可以在`docker-compose.yml`中`php`的开启相应的拓展，如果里面没有你需要的拓展的话，你可以在`/php/dockerfile`中写入，如需要安装`zip`包

```shell
RUN pecl install zip && docker-php-ext-enable zip 
```
如果不知道要安装的包的具体名字，可以到[https://pecl.php.net](https://pecl.php.net)去搜索，如果`pecl`上没有这个拓展，你就得通过其他的方式安装拓展了。

##### 目录的问题

在`application`的`volumes`中进行相应目录的挂载，其他`service`通过`volumes_from`可以将挂载同步过去。以`php`为例，当启动`php`启动后，容器中的目录为`php`本身的目录+`application`中`volumns`的目录。
