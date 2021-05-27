
#### 初始化一个插件项目

执行 `mm init dev` 选择 `插件`，然后输入项目名称 (项目名称即为该命令名称)，即可初始化一个插件项目，项目目录如下：

``` 
[插件项目目录]
└── src
    ├── handle.ts
    └── index.ts
```

`/src/index.ts` 入口文件代码示例：

```javascript
import { CommanderStatic } from 'commander'
import handler from './handler'
import { ICommandConfig } from '@ali/mm-cli-core/types'

export default async () => {
    /**
     * 配置遵循commander的配置
     * commander文档：https://github.com/tj/commander.js/blob/HEAD/Readme_zh-CN.md
     */
  const plugin: ICommandConfig = {
    name: 'plugin-name',
    command: 'plugin-name',
    description: 'plugin description ...',
    // options: [
    //     ['-n, --name <name>', 'name'],
    // ],
    async action (command: CommanderStatic) {
      await handler(command)
    }
  }

  return plugin
}

```


