#### 套件需要对照实现的开发命令
 - `add` 添加页面 
 - `dev` 本地开发服务 
 - `test` 测试

> `init`命令一般不需要实现，cli内置了，只需要提供脚手架即可(@崇志)
> 
> `build` `daily` `publish`三个命令也无需实现，统一由cli调用def来进行构建与发布，只需要确保项目根目录下有abc.json配置即可(详见https://builder.alibaba-inc.com/document?slug=config)


#### 套件代码示例

> 套件模块包含一个rmx实例，提供一些实用方法，如对接平台的接口创建项目，保存配置等，详见 [rmx实例api](rmx-api)

```javascript
module.exports = async (rmx) => {
    const Kit = {
        commands: {}
    }

    /**
     * 配置遵循commander的配置
     * commander文档：https://github.com/tj/commander.js/blob/HEAD/Readme_zh-CN.md
     * 多参数的以二维数组形式
     * rmx-cli内置了init, dev, add, build, daily, publish, test 七个命令
     * 套件可自行覆盖实现这些命令，也可增加额外的这七个固定命令以外的命令来辅助套件开发
     * 所有命令均支持配置before, after钩子函数来进行执行命令前后的一些自定义处理
    */

    //初始化项目，cli内置实现了，只需要提供脚手架地址即可
    //同时提供参数标识是否自动创建gitlab,iconfont,chartpar,rap2,def等平台项目
    Kit.commands.init = {
        command: 'init',
        description: 'init的描述',
        params: {
            //提供一个可以对项目名称做校验的方法
            nameValidate(value, answer) {
                //value: 项目名称
                //answer: 包含脚手架url等信息
                //特定脚手架需要限制项目名称
                if (answer.scaffoldGitUrl.includes(`boom-template-scaffold`)) {
                    if (!/^cellex\-.+/.test(value)) {
                        return `项目名称必须以cellex-开头`
                    }
                } else {
                    return true
                }
        },
        //init由cli内置实现了，如果没有特殊需求，不需要实现action，只需要提供脚手架地址即可
        //可以提供 _before, _after钩子函数来进行一些自定义的处理
        async _before() {},
        async _after(info, log) {
            //info 为项目初始完成后创建的各平台的项目信息(如id)
            //log 为自定义的打印信息
        }
      
    }

    //本地开发服务
    Kit.commands.dev = {
        params: {
            enableSudo: true //标识当前命令是否允许sudo执行，默认cli所有命令不可sudo运行，以免污染文件权限
        },
        command: 'dev',
        description: 'dev的描述',
        options: [] //命令的参数，多个参数以二维数组形式
        //命令主逻辑函数，必须为异步方法
        async action(options) {
            //dev here...
        }
    }

    //添加模板页面
    Kit.commands.add = {
        command: 'add',
        description: 'add的描述',
        options: [] //命令的参数，多个参数以二维数组形式
        //命令主逻辑函数，必须为异步方法
        async action(options) {
            //add here...
        }
    }

    //可以设置某个内置命令为false，则在该套件内忽略该内置命令
    //共有七个内置命令：init、dev、add、build、daily、publish、test
    Kit.commands.build = false  

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

#### 注意事项
- 套件提供的脚手架的package.json里必须包含标识
    ```json
    {
        "rmxConfig": {
            "kit": "[你的套件名]"
        }
    }
    ```
- 套件可以任意定义自己的命令，但如果与系统命令(如`login`, `logout`)重名，则无效
- 如果套件命令与插件命令重名，则在该套件的项目内执行该命令时，执行的是套件的该命令，忽略插件命令