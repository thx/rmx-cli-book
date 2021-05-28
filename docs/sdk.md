### 辅助开发套件(插件)的SDK

mm-cli实现了一个专门用来辅助开发套件(插件)的套件 `dev`

#### 初始化套件/插件

执行 `mm init dev` 选择开发类型(套件/插件)

<img src="https://img.alicdn.com/tfs/TB1lV8uAhD1gK0jSZFsXXbldVXa-698-172.png" width="40%">

随后填写资料即可初始化好一个套件(插件)
> `【请输入应用名称】` 在插件情形下为插件的命令名，套件情形下为套件的名称

<img src="https://img.alicdn.com/imgextra/i1/O1CN01tixAGz1lqVWLCG9Ez_!!6000000004870-2-tps-1424-852.png" width="80%">


#### 本地开发套件/插件

1. 找 `@崇志` 或 `@墨智` 将当前开发套件/插件添加到 mm-cli 的alp配置里 （需要提供套件/插件名称、说明，如果是套件需要提供脚手架仓库）
2. 当前项目根目录执行 `mm dev` 即可进行本地开发测试了 (会将包进行本地link)
 
##### 套件调试开发：
  - 运行 `mm init [你的套件名]`，会读取你之前提交的套件对应的脚手架仓库列表，选择一个完成初始化项目
  - 在你初始化好的脚手架项目里测试你的各个命令
  - 默认套件包含以下命令：<br><br>
    <img src="https://img.alicdn.com/imgextra/i3/O1CN01R7VSSk285Y4qB0rSu_!!6000000007881-2-tps-1416-434.png" width="80%">
  - `app/commands.ts` 文件为所有命令定义入口
  - 一般来说，`init` `daily` `publish` 不需要套件单独实现，mm-cli 内置了，套件只需实现 `dev` 以及其他套件所需的命令即可

##### 插件调试开发：
  - 运行 `mm [你的插件名]`，即可调试开发当前插件里的代码
  - 插件主逻辑代码在 `src/index.ts` 里


#### 如何发布

在项目根目录下执行 `mm publish` 即可将包发布到 tnpm 上（会自动提交代码，更新 version 版本，然后再发布到 tnpm）


