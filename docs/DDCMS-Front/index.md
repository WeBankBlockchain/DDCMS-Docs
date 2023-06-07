# DDCMS-Front

[![License](https://img.shields.io/badge/license-Apache%202-4EB1BA.svg)](https://www.apache.org/licenses/LICENSE-2.0.html)


DDCMS-Front为前端操作页面，各个角色均需通过它来使用DDCMS。它包含两个核心功能：数据浏览、数据关联。

- 数据浏览：当进入首页后，或者点击左上角DDCMS图标，可以浏览已通过审核的数据目录和对应的产品。
- 数据管理：当登陆后，用户可以点击右上角的按钮，进入管理台页面，在里面将会根据不同的角色，展示不同的功能，例如：数据提供方可以创建业务、数据目录；系统运营方可以查看、审核注册请求；数据见证方可以查看、投票数据目录及归属业务。

## 快速开始

### 克隆代码
```
git clone https://github.com/WeBankBlockchain/DDCMS-Front.git
cd DDCMS-Front
git checkout origin/dev
```

### 修改配置
创建.env（可以参考.env.example）中，将REACT_APP_SERVER_API修改为DDCMS-Service启动后的api地址，例如：
```
REACT_APP_SERVER_API=http://localhost:10880/api/
```


更新依赖：
```
yarn
```

启动：
```
yarn start
```

然后用户可以访问[](http://localhost:3000) 进行体验。

