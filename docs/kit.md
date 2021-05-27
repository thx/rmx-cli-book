
#### 初始化一个套件项目
执行 `mm init dev` 选择 `套件`，即可初始化一个套件项目，项目目录如下：

``` 
[套件项目目录]
├── app
    ├── commands.ts //入口文件
    ├── routes.json
    └── controller
        └── hello.ts
```

#### 套件一般情况下需要对照实现的开发命令
 - `add` 添加页面 
 - `dev` 本地开发服务 
 - `test` 测试

> `init`命令一般不需要实现，cli内置了，只需要提供脚手架即可(@崇志)
> 
> `build` `daily` `publish`三个命令也无需实现，统一由cli调用def来进行构建与发布，只需要确保项目根目录下有abc.json配置即可(详见https://builder.alibaba-inc.com/document?slug=config)


`app/commands.ts` 入口文件代码示例：

```javascript
import { ICommandList, ICreateAppInfo } from '@ali/mm-cli-core/types'
import { blueBright, cyanBright, grey } from 'chalk'
import { spawn } from 'child_process'
import { CommanderStatic } from 'commander'
import { EventEmitter } from 'events'

export default async function () {
  const commands: ICommandList = []

   /**
     * 配置遵循commander的配置
     * commander文档：https://github.com/tj/commander.js/blob/HEAD/Readme_zh-CN.md
     * 多参数的以二维数组形式
    */

  commands.push({
    params: {
      iconfont: false,
      rap: false,
      chartpark: false,
      spma: false,
      clue: false
    },
    command: 'init',
    description: '应用初始化',
    // 内置 init (初始化套件配置的脚手架) 完毕后，执行套件特有的后续操作
    // after 方法 cli 会自动执行，__after__/__before__ 是给 init 特殊提供, cli 与 webui 共用逻辑
    async __after__ (appInfo: ICreateAppInfo, emitter: EventEmitter) {
      console.log(`\n应用 ${cyanBright(appInfo.app)} 创建成功！你可以执行以下命令：\n`)
      console.log(`  ${grey('●')} ${cyanBright(`cd ${appInfo.app}`)}`)
      console.log(`  ${grey('●')} ${cyanBright('mm dev')}      ${grey('启动本地开发服务')}`)
      console.log(`  ${grey('●')} ${cyanBright('mm -h')}       ${grey('查看所有命令帮助')}`)
      console.log('')
    },
    on: [
      ['--help', () => {
        console.log('\nExamples:')
        console.log(`  ${grey('$')} ${blueBright('mm init react')}`)
        console.log()
      }]
    ]
  })

  commands.push({
    command: 'hello',
    description: '命令示例',
    async action (command: CommanderStatic) {
      console.log('hello', command)
    }
  })
  commands.push({
    command: 'check',
    options: [],
    description: '依赖检查 Checks for outdated/unused/missing package dependencies',
    async action (command: CommanderStatic) {
      const tasks: Array<any> = [
        ['yarn', ['check', '--verify-tree']],
        ['yarn', ['outdated']],
        ['npx', ['depcheck']]
      ]
      async function doit (command: string, args: Array<string>) {
        console.log(grey(`$ ${command} ${args.join('')}`))
        const subprocess = spawn(command, args, {
          stdio: 'pipe',
          env: { ...process.env, FORCE_COLOR: '1' }
        })
        subprocess.stdout.on('data', chunk => console.log(chunk.toString()))
        subprocess.stdout.on('error', error => console.error(error))
        await new Promise((resolve, reject) => {
          subprocess.on('close', code => resolve(code))
        })
      }
      for (const task of tasks) {
        const [command, args] = task
        await doit(command, args)
      }
    },
    on: [
      ['--help', () => {
        console.log('\nExamples:')
        console.log(`  ${grey('$')} ${blueBright('mm check')}`)
        console.log()
      }]
    ]
  })

  return commands
}
```


#### 注意事项
- 套件可以任意定义自己的命令，但如果与系统命令(如`login`, `logout`)重名，则无效
- 如果套件命令与插件命令重名，则在该套件的项目内执行该命令时，执行的是套件的该命令，忽略插件命令