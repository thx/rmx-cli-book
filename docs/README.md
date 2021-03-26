#### 为什么有mm-cli?
`magix-cli` 主要是面向magix体系的命令行开发工具，其本身已经足够完善，但是对覆盖其他技术体系(如react)的框架来说不够友善，只能使用配置转发命令的形式，所以就有了`mm-cli`（`mm-cli`另外提供了webui可视操作界面，详见[webui](webui)）
> 欢迎加入mm-cli交流群，群号：32665864


#### 架构说明
mm-cli主要是基于magix-cli做了更高一层抽象，抽象出三个层级的命令
 - 系统命令 (提供基础的功能，如用户登录，对接webui，管理套件/插件)
 - 套件命令 (套件是针对框架体系一系列的命令集成，magix-cli会以套件形式接入mm-cli，针对react体系也会有另一个套件)
 - 插件命令 (一些与项目无关的通用的全局功能以插件形式接入，如之前的`mm createDaily`, `mm clear`)

#### 如何安装

本工具依赖tnpm，请先安装tnpm，终端执行以下命令即可安装

```node
npm install --registry=https://registry.npm.alibaba-inc.com -g tnpm
```

tnpm安装好后在终端执行以下命令即可安装 `mm-cli` 命令行工具

```node
tnpm i -g @ali/mm-cli
```


#### mm系统命令
 
 - `mm login` 输入域账号密码登录gitlab, 以获取相关用户信息，保存在本地
 - `mm logout` 退出登录
 - `mm web` 打开webui
 - `mm install` 安装(或更新)套件/插件，会列出所有可用套件/插件，让用户选择安装
 - `mm uninstall` 卸载本地已安装的套件/插件
 - `mm list` 列出本地安装过的套件/插件列表


#### magix-cli用户升级说明
原来的magix-cli会做为套件接入mm-cli，安装完mm-cli后，使用的命令与原来保持一致，但是命令主体统一为 `mm`，如 `mm dev`，初次使用会提示安装`magix`套件，确定安装后即可正常使用（ magix套件命令说明请移步https://thx.github.io/magix-cli-book ）


#### 套件/插件升级提示

- `套件` 用户在项目内执行套件相关的命令时，系统会给出该套件的升级提示（如果该套件有新版本）
- `插件` 用户在执行某插件命令时，给出该插件的升级提示

