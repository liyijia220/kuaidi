# 快递管理系统

### 2024/2/8 
#### 基础开发环境：统一版本号。
- JDK: 1.8
- Maven: 3.5+
- MySql: 5.7+
- Redis: 3.2 +
- Node Js: 10.0 +
- Npm: 5.6.0+
- Yarn: 1.21.1+
### IDE插件 (lombok 插件必装)

# 介绍
基于web开发的快递管理系统，涵盖模块：用户管理、车辆管理、快递管理、仓库管理、库存管理、系统管理等模块组成

# 软件架构
- jeecg-boot-master 后台项目
- cable.sql 后台管理系统数据库脚本

# 所用技术
- 此系统基于 Jeecg-boot 为脚手架开发的PRD管理系统
- 后端技术：SpringBoot 2.1.3 + Shiro 1.4.0 + Redis + Mysql 5.7 + MyBatis-Plus 3.1.2 + Jwt 3.7.0 + Swagger-ui
- 前端技术：Vue + Ant-design-vue + Webpack
- 其他技术：Druid(数据库连接池)、Logback(日志工具)、poi(Excel工具)、Quartz(定时任务)、lombok(简化代码)
- 项目构建：Maven3.5+、JDK1.8+

# 项目所需软件下载路径及jeecg文档说明
- JeecgBoot官方文档 [http://jeecg-boot.mydoc.io/](http://jeecg-boot.mydoc.io/)

> # 启动教学
# 1. 数据库配置
- 首先在本地创建 cable 数据库，选择好字符集编码
- 然后在创建好的 cable 数据库下执行 cable.sql 脚本即可

# 2. 后端配置
- 进入 IDEA 工具后设置 Maven 依赖下载设置
- 更改自己的 Maven 安装路径，用来下载项目所需的 jar 包
- 选择后台项目的启动环境 application.yml-> dev[开发环境] 或者 prod[生产环境]
- 然后更改对应开发环境的配置文件，如 application-dev.yml 文件
- 配置项目启动端口号
- 配置数据库连接信息
- 配置 redis 连接信息
- 配置 jeecg 专用配置文件上传路径
- 找到 JeecgApplication 启动类启动项目即可
- 通过访问 `http://localhost:8080/jeecg-boot/` 可以查看后台 API 接口文档（开发者查看）

# 3. 前端配置
- 前端项目使用 VsCode 工具打开，在控制台执行 `npm install` 命令下载所需依赖
- 配置 index.html 页面的全局配置 -> 指定后台路径
- 配置项目根目录下的 vue.config.js 文件，指定后台路径,建立前后端对接
- 最后配置完成后，需要前端后端同时启动才能访问项目
- 前端通过 `npm run serve` 命令启动
