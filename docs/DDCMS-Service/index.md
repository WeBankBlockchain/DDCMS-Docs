# DDCMS-Service

[![License](https://img.shields.io/badge/license-Apache%202-4EB1BA.svg)](https://www.apache.org/licenses/LICENSE-2.0.html)


DDCMS-Service由系统运营方部署，它负责接收前端传来的请求。从功能而言，它承担了用户管理、业务管理、数据目录管理等功能。


## 快速开始
### 克隆代码
```
git clone https://github.com/WeBankBlockchain/DDCMS-Service.git
cd DDCMS-Service
git checkout origin/dev
```

### 编译

在DDCMS-Service项目根目录下执行
```
bash ./gradlew bootJar
```
这条指令会将代码编译成springboot jar包，该jar包位于dist目录。

### 链配置
由于后端服务需要访问区块链，因此需要先配置链信息，包括证书、sdk配置，这些信息可以直接从sdk里拷贝。

首先拷贝控制台里的证书到DDCMS-Service工程的conf目录下:

```
cp [控制台目录]/conf/* dist/conf
```


### 数据库初始化
进入数据库，依次执行resources目录下的数据库脚本：
- scripts/db-script.sql


### 服务配置
编辑dist/application.yml，按照下面的模板进行配置，其中打了${}的需要自己配置。
```
server:
  port: 10880

spring:
  mvc:
    async:
      request-timeout: 500
  servlet:
    multipart:
      max-file-size: 10MB
      max-request-size: 10MB
  datasource:
    url: ${数据库连接}
    username: ${数据库用户名}
    password: ${数据库密码}
    driverClassName: com.mysql.cj.jdbc.Driver
    type: com.alibaba.druid.pool.DruidDataSource
    defaultAutoCommit: false
    initialSize: 25
    maxActive: 128
    maxIdle: 50
    minIdle: 25
    testOnBorrow: false
    testOnReturn: false
    testWhileIdle: true
    validationQuery: SELECT 1
    timeBetweenEvictionRunsMillis: 34000
    minEvictableIdleTimeMillis: 34000
    removeAbandoned: false
    logAbandoned: true
    removeAbandonedTimeout: 54
    maxWait: 10000
    poolPreparedStatements: false
    connectionProperties: druid.stat.mergeSql=true;druid.stat.slowSqlMillis=5000

mybatis:
  typeAliasesPackage: com.webank.databrain.dao.entity
  configuration:
    map-underscore-to-camel-case: true

jwt:
  secret: ${jwt secret}
  expiration: 8640000

system:
  bcos-cfg: "config/config.toml"
  bcos-group-id: "group0"
  crypto-type: 0
  admin-account: ${系统运营方账户}
  admin-password: ${系统运营方密码}
  admin-private-key: ${见证方私钥，例如0x11afa82f974469792aa0172931b813d4fc7dd9177f3211779efc5f955d5e480f}
  admin-company: '系统运营方'
  contractConfig:
    account-contract: ${账户合约地址}
    product-contract: ${业务合约地址}
    dataSchema-contract: ${数据目录合约地址}
  fileConfig:
    file-dir: "tmp"
swagger:
  enabled: true



auth:
  permitAllApiList:
    - /api/account/register
    - /api/account/pageQueryCompany
    - /api/account/queryCompanyByUsername
    - /api/account/queryPersonByUsername
    - /api/account/queryCompanyByAccountId
    - /api/account/getHotCompanies
    - /api/account/searchCompany
    - /api/account/searchPerson
    - /api/schema/pageQuerySchema
    - /api/schema/querySchemaById
    - /api/schema/querySchemaAccessById
    - /api/file/download
    - /api/file/upload
    - /api/product/getHotProducts
    - /api/product/pageQueryProduct
    - /api/product/queryProductById
    - /api/tag/getHotTags
    - /v2/api-docs
    - /configuration/**
    - /swagger*/**
    - /webjars/**
    - /csrf
    - /
  anonymousApi: /api/account/login
  roleAuth:
    companyAuth:
      - /api/product/createProduct
      - /api/schema/createSchema
      - /api/product/updateProduct
    witnessAuth:
      - /api/product/approveProduct
    adminAuth:
      - /api/account/approveAccount

```
需要自己配置的属性包括：

- spring.datasource.url: 数据库连接url。示例： jdbc:mysql://localhost:3306/databrain?serverTimezone=GMT%2B8&useUnicode=true&characterEncoding=utf-8&zeroDateTimeBehavior=convertToNull&allowMultiQueries=true
- spring.datasource.username：数据库用户名。
- spring.datasource.password：数据库密码
- system.admin-account：系统运营方登陆账户名。示例：admin
- system.admin-password：系统运营方登陆密码。示例：admin 
- system.admin-private-key：系统运营方私钥 。注意带上双引号，示例"0x11afa82f974469792aa0172931b813d4fc7dd9177f3211779efc5f955d5e480f"。
- system.contractConfig.account-contract：账户合约地址。注意带上双引号，例如"0x1e0171e2f59d00a1851c5f8ca8af1b208f52627b"。
- system.contractConfig.product-contract：业务合约地址。注意带上双引号，例如"0x2f45c1aac14531d8bd3269d7d2c9ffc342798dcb"。
- system.contractConfig.dataSchema-contract：业务合约地址。注意带上双引号，注意带上双引号，例如"0xb426ac2d4436c051ae71a149621c76620733ede3"。
- jwt.secret: 用于支持jwt，是一个base64的字符串，例如“Ok2Q0AZiRTT1N6CJ3q8ZvQOk2Q0AZiRTT1N6CJ3q8ZvQOk2Q0AZiRTT1N6CJ3q8ZvQOk2Q0AZiRTT1N6CJ3q8ZvQOk2Q0AZiRTT1N6CJ3q8ZvQ”

### 启动
在DDCMS-Service项目根目录下执行
```
cd dist
bash start.sh
```
这条指令会编译代码成springboot jar包，并执行。

创建好后，可以通过http://localhost:10880/swagger-ui.html 访问其swagger页面。