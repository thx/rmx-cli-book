
#### 插件代码示例
> 插件模块包含一个rmx实例，提供一些实用方法，如对接平台的接口创建项目，保存配置等，详见 [rmx实例api](rmx-api)

```javascript
module.exports = async (rmx) => {
    /**
     * 配置遵循commander的配置
     * commander文档：https://github.com/tj/commander.js/blob/HEAD/Readme_zh-CN.md
     */
    const plugin = {
        command: 'createDaily', //命令名称，如: rmx createDaily
        description: '自动创建daily分支，可以选择时间戳或指定版本形式，避免多人协作分支冲突问题',
        alias: 'cd',
        options: [ //多参数的以二维数组形式
            ['-n, --branchName <branchName>', '设置语义化的分支名称'],
            ['-t, --timestamp', '是否是时间戳形式分支']
        ],
        //必须异步方法
        async action(options) {
            require('./createDaily')(options, rmx)
        }
    }

    return plugin
}

```


