# Spring Cloud Config Sample

本示例是通过阅读Spring官网上的 [ Spring Cloud Config ](http://cloud.spring.io/spring-cloud-config/) 版本为 [1.3.0 M1](http://cloud.spring.io/spring-cloud-static/spring-cloud-config/1.3.0.M1/) 的指南而创建的示例

将通过创建里程碑的方式记录每一个阶段的学习及应用

###运行示例

__服务端__

1. 运行cloud-config-server下的ConfigServerApplication
1. 通过server服务查看client的配置信息[默认配置](http://localhost:8888/config-client/default)或[开发环境配置](http://localhost:8888/config-client/dev)

__客户端__

1. 运行cloud-config-client下的ConfigClientApplication
1. 访问[HTTP message](http://localhost:8080/message)地址进行查看

#版本

##v1.0 创建示例项目

- 创建cloud config sample主项目，并创建cloud-config-client及cloud-config-server子项目
- cloud-config-server提供配置服务，cloud-config-client通过server从[config-repo](https://github.com/wzz-code/config-repo)获取配置

##v1.1 环境资源配置

##v1.1.1 search-paths配置

属性配置`search-paths`指定在uri资源下search-paths的子目录查找配置文件，示例如下
```yaml
spring:
  cloud:
    config:
      server:
        git:
          uri: https://github.com/wzz-code/cloud-config-sample
          search-paths: config-repo
```
如上，将在 https://github.com/wzz-code/cloud-config-sample 的config-repo下查找配置文件，也可直接指定uri为 https://github.com/wzz-code/cloud-config-sample/config-repo 。
`search-paths`也可以配置为 `search-path: config-repo, config-repo/dev*`

##v1.1.2 资源文件匹配和多个资源的配置

应用程序名称和profile可以支持更复杂的需求，多种模式进行匹配。此配置格式为 ``{application}/{profile}``
，当有多个值时，可以用逗号进行分隔，值也可以进行模糊匹配。示例如下：
```yaml
spring:
  cloud:
    config:
      server:
        git:
          uri: https://github.com/wzz-code/cloud-config-sample
          search-paths: config-repo
          repos:
            client:
              pattern: config-client/dev, test/dev*, test*/dev
              uri: https://github.com/wzz-code/cloud-config-sample
              search-paths: config-repo/config-client-multiple
            #也可以本地配置
            #local: 
            #  pattern: local*
            #  uri: file:/xxx/xxx/repo-config 或 file:/F:/xxx/xxx/repo-config 的git仓库
```
此示例中 config-client的dev环境会被``pattern: config-client/dev`` 匹配到，并读取指定的``uri/search-paths``中的``config-client-dev.properites配置文件``

