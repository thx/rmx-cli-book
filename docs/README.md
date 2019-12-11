### 为什么有rmx-cli?
magix-cli 主要是面向magix体系的命令行开发工具，其本身已经足够完善，但是对覆盖其他技术体系的框架来说不够方便，只能使用配置转发命令的形式，所以就有了rmx-cli

### 架构说明
rmx-cli主要是基于magix-cli做了更高一层抽象，抽象出三个层级的命令
 - 系统命令 (提供基础的功能，如用户登录，对接webui，管理套件/插件)
 - 套件命令 (套件是针对框架体系一系列的命令集成，magix-cli会以套件形式接入rmx-cli，针对react体系也会有另一个套件)
 - 插件命令 (一些与项目无关的通用的全局功能以插件形式接入，如之前的`mm createDaily`, `mm clear`)

### 全局安装
```node
tnpm i -g @ali/rmx-cli
```

### rmx系统命令
 
 - `rmx login` 输入账号密码登录gitlab, 以获取相关用户信息，保存在本地config.json中
 - `rmx logout` 退出登录
 - `rmx web` 打开webui
 - `rmx install` 安装(更新)套件/插件，列出所有可用套件/插件，让用户选择安装
 - `rmx uninstall` 卸载套件/插件，列出本地安装的套件/插件
 - `rmx list` 列出本地安装过的套件/插件列表



### 套件
#### 套件需要对照实现的统一的开发命令
 - `init` 初始化 
 - `add` 添加页面 
 - `dev` 本地开发服务 
 - `build` 本地构建(依赖def的abc.json配置) (cli内置了，一般不需要套件实现) 
 - `daily` 日常发布 (cli内置了，一般不需要套件实现) 
 - `publish` 正式发布 (cli内置了，一般不需要套件实现) 
 - `test` 测试

#### 套件安装地址
/[用户目录]/.rmx/kit/[套件名]

#### 升级提示
用户在项目内执行套件相关的命令时间，给出该套件的升级提示

#### 套件代码示例

```javascript
module.exports = async (rmx) => {
    //rmx提供一些实用方法，如对接平台的接口创建项目，保存配置等
    //rmx.util提供一些常用的工具方法，如spawn

    const Kit = {
        commands: {}
    }

    //配置遵循commander工具的配置
    //多参数的以数组形式
    //rmx-cli内置了init, dev, add, build, daily, publish, test 七个命令
    //套件可自行覆盖实现这些命令，也可增加额外的这七个固定命令以外的命令来辅助套件开发

    //初始化项目，cli内置实现了，只需要提供脚手架地址即可
    //同时提供参数标识是否自动创建gitlab,iconfont,chartpar,rap2,def等平台项目
    //提供before, after钩子来进行执行命令前后的一些杂事
    Kit.commands.init = {
        command: 'init',
        description: 'init的描述',
        options: [] //命令的参数
        //命令主逻辑函数，必须为异步方法
        async action(options) {
            //init here...

            /**
            rmx实例提供setConfig/getConfig方法用来保存一些全局配置
            rmx.setConfig会固定将配置存在[用户目录]/.rmx/.config.js中
            套件的配置会固定保存在kit.[kitName].[kitConfig]命名空间下，避免污染其他全局配置
            */

            rmx.setConfig(`kitConfig`, 'some configs...')
            rmx.getConfig('kitConfig')

            //rmx同时提供对接各平台的接口方法，如gitlab平台的创建项目(iconfont, chartpark, def, rap2等等)
            rmx.gitlab.createProject(name, group, options)
            rmx.iconfont.createProject(name, options)
        }
    }

    //本地开发服务
    Kit.commands.dev = {
        enableSudo: true, //标识当前命令是否允许sudo执行，默认cli所有命令不可sudo运行，以免污染文件权限
        command: 'dev',
        description: 'dev的描述',
        options: [] //命令的参数
        //命令主逻辑函数，必须为异步方法
        async action(options) {
            //dev here...
        }
    }

    //添加模板页面
    Kit.commands.add = {
        command: 'add',
        description: 'add的描述',
        options: [] //命令的参数
        //命令主逻辑函数，必须为异步方法
        async action(options) {
            //add here...
        }
    }
    //Kit.commands.add = false, 设置某个内置命令为false，则在该套件内忽略该内置命令

    //build命令一般不需要定义，cli内置的build命令结合了DEF的云构建可以涵盖大部分场景
    //DEF云构建依赖项目中的abc.json，详见DEF的文档
    //可以定义before，after钩子函数，在rmx daily前后做一些自定义事情 
    Kit.commands.build = {
        command: 'build',
        description: 'build的描述',
        options: [] //命令的参数
        //命令主逻辑函数，必须为异步方法
        async action(options) {
            //build here...
        }
    }


    //daily命令一般不需要定义，cli内置的daily命令结合DEF发布系统可以涵盖多数发布场景
    //DEF云构建依赖项目中的abc.json，详见DEF的文档
    //可以定义before，after钩子函数，在rmx daily前后做一些自定义事情 
    Kit.commands.daily = {

        //执行命令前的勾子函数
        async before(options) {
            console.log(`hook before...`)
            // throw new Error('执行错误'); // 抛出异常，将不再执行相关命令动作，包括 after hook 也不会执行
            // return false; // 返回 false，将不再执行相关命令动作，但是会执行 after hook

        },

        //执行命令后的勾子函数
        async after(options) {
            console.log(`hook afeter...`)
        }
    }

    Kit.commands.publish = {}

    //测试命令
    Kit.commands.test = {
        command: 'test',
        description: 'test的描述',
        options: [] //命令的参数
        //命令主逻辑函数，必须为异步方法
        async action(options) {
            //test here...
        }
    }

    return Kit
}
```


### 插件
全局通用的命令

#### 插件安装地址
/[用户目录]/.rmx/plugin/[插件名]

#### 升级提示
用户在执行某插件命令时，给出该插件的升级提示

#### 插件代码示例
```javascript
module.exports = async (rmx) => {
    //rmx实例提供一些实用方法，如对接平台的接口创建项目，保存配置等
    //插件的rmx.setConfig会固定保存在用户config.json的plugin命令空间下，避免污染全局配置
    //rmx.util提供一些常用的工具方法，如spawn
    const plugin = {
        command: 'createDaily',
        description: '自动创建daily分支，可以选择时间戳或指定版本形式，避免多人协作分支冲突问题',
        alias: 'cd',
        options: [
            ['-n, --branchName <branchName>', '设置语义化的分支名称'],
            ['-t, --timestamp', '是否是时间戳形式分支']
        ],
        async action(options) {
            require('./createDaily')(options, rmx)
        }
    }

    return plugin
}

```



### 问题
- 套件与插件定义问题？
  - 套件提供一系列项目开发相关的命令的集成
  - 插件提供一些与项目无关的全局的功能

- 插件里的命令如果与套件里定义的命令重复了，怎么办
  - 套件里的命令优先覆盖掉插件的定义
  - 系统自带的命令优先于套件里定义的

- 项目package.json里需要加个参数标识当前项目用的套件是哪个，以决定 rmx dev时用的是哪个套件的dev命令
  - package.json增加配置 
    ```json
    {
        "rmxConfig": {
            "kit": "magix"
        }
    }
    ```

- 插件/套件会接收一个rmx实例，包含rmx提供的一些实用方法，如对接平台接口创建项目，保存配置等
- 插件/套件的rmx.setConfig会固定保存到config.json的kit(或plugin)命名空间下，避免污染全局配置
- cli版本与套件版本问题？
  - cli本身rmx实例提供的基础功能应该相对稳定

- 套件/插件开发sdk (开发一个套件专门用来开发套件/插件)
  - 已完成，`rmx init` 选择套件 `dev`即可开始开发一个你自己的套件或插件

