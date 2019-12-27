#### 为什么有rmx-cli?
`magix-cli` 主要是面向magix体系的命令行开发工具，其本身已经足够完善，但是对覆盖其他技术体系(如react)的框架来说不够友善，只能使用配置转发命令的形式，所以就有了`rmx-cli`（`rmx-cli`提供了webui可视操作界面，详见[webui](webui)）

#### 架构说明
rmx-cli主要是基于magix-cli做了更高一层抽象，抽象出三个层级的命令
 - 系统命令 (提供基础的功能，如用户登录，对接webui，管理套件/插件)
 - 套件命令 (套件是针对框架体系一系列的命令集成，magix-cli会以套件形式接入rmx-cli，针对react体系也会有另一个套件)
 - 插件命令 (一些与项目无关的通用的全局功能以插件形式接入，如之前的`mm createDaily`, `mm clear`)

#### 全局安装
请先安装Xcode，执行命令
```
xcode-select --install
```

xcode安装完毕后执行

```node
tnpm i -g @ali/rmx-cli
```


#### rmx系统命令
 
 - `rmx login` 输入域账号密码登录gitlab, 以获取相关用户信息，保存在本地
 - `rmx logout` 退出登录
 - `rmx web` 打开webui
 - `rmx install` 安装(或更新)套件/插件，会列出所有可用套件/插件，让用户选择安装
 - `rmx uninstall` 卸载本地已安装的套件/插件
 - `rmx list` 列出本地安装过的套件/插件列表


#### magix-cli用户升级说明
原来的magix-cli会做为套件接入rmx-cli，安装完rmx-cli后，使用的命令与原来保持一致，（如`mm dev` => `rmx dev`，也可以继续使用`mm`命令入口），初次使用会提示安装`magix`套件，确定安装后即可正常使用（ magix套件命令说明请移步https://thx.github.io/magix-cli-book ）


#### 套件/插件升级提示

- `套件` 用户在项目内执行套件相关的命令时，系统会给出该套件的升级提示（如果该套件有新版本）
- `插件` 用户在执行某插件命令时，给出该插件的升级提示
