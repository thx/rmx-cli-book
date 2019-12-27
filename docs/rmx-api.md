### 插件/套件配置方法
#### rmx.setConfig(path, value)
接口说明： 提供给套件、插件使用的保存配置的方法，套件的setConfig只会固定存在`kit.[套件名].[配置名]`的命名空间下，插件固定存在`plugin.[插件名].[配置名]`命名空间下，避免影响其他全局的配置，比如user用户信息

入参：
- path `[string]` path格式：'xx.yy.zz'
- value `[any]` value为标准json值

---

#### rmx.getConfig(path)
接口说明： 获取套件/插件下的由`rmx.setConfig`设置过的配置

入参：
- path `[string]` path格式：'xx.yy.zz'

返回 `json` 格式值

---


#### rmx.getRootConfig(path)
接口说明： 获取根级配置，如user信息

入参：
- path `[string]` path格式：'xx.yy.zz'

返回 `json` 格式值

示例：
```javascript
const user = rmx.getRootConfig('user')
```

---


### 工具函数

#### rmx.utils.spawn(command, args, options) `[event]`
接口说明：`child_process.spawn`的封装

入参：
- command `[string]` 命令名称
- args `[array]` 命令参数
- options `[object]` spawn的相关配置

返回 emitter `[event]` 事件实例，支持事件：
- `data` 执行命令的log输出
- `close` 命令执行完毕的回调

示例：
```javascript
    rmx.utils.spawn('tnpm', ['install', '--color'])
        .on('data', msg => {
            console.log(msg)
        })
        on('close', resp => {
            if(resp.error) {
                console.log(`命令运行失败: ${resp.error}`)
            } else {
                console.log(`命令运行成功`)
            }
        })
```

---

#### rmx.utils.getPrecentBranch(cwd) `[async]`
接口说明：获取当前分支名

入参：
- cwd `[string]` 工作目录，不传则为当前目录

返回分支名 `[string]`

示例：
```javascript
const branch = await rmx.utils.getPrecentBranch() //'daily/0.0.1'
```

---

#### rmx.utils.portIsOccupied(port) `[async]`
接口说明：检测端口是否被占用

入参：
- port `[number]` 端口号

如果端口被占用抛出错误

示例：
```javascript
    try {
        await rmx.utils.portIsOccupied(8080)
    } catch(error) {
        console.log('端口被占用')
    }

```


---

### 对接gitlab的接口

#### rmx.gitlab.login() `[async]`
接口说明：登录gitlab系统，登录成功会在本地保存gitlabToken, 域账号，工号等信息

示例：
```javascript
    await rmx.gitlab.login()
```

---

#### rmx.gitlab.getToken() `[async]`
接口说明：获取gitlab的token，如果没登录，会先提示登录

返回：gitlab的token `[string]`

示例：
```javascript
   const private_token = await rmx.gitlab.getToken()
```
---

#### rmx.gitlab.getGroups() `[async]`
接口说明：获取用户gitlab的分组信息，如果没登录，会先提示登录

返回：
- groups `[object]`

```json
{
    "groupsList": [{
        "name": "mm",
        "value": 3186
    }],
    "groupsListMap" : {
        "3186": "mm"
    },    
    "groupsListNameMap": {
        "mm": 3186
    }
}
```

示例：
```javascript
    const groups = await rmx.gitlab.getGroups()
```
---

#### rmx.gitlab.createProject(name, groupId, options) `[async]`
接口说明：创建gitlab项目

入参：
- name `[string]` 项目名称
- groupId `[number]` 分组id
- options `[object]` gitlab开放api的options配置

返回：
- gitlab项目信息 `[object]`


示例：
```javascript
    const projectInfo = await rmx.gitlab.createProject(name, groupId, options)
```
---


### 对接rap2的接口

#### rmx.rap.createProject(name, options) `[async]`
接口说明：创建RAP2项目，返回项目信息

入参：
- name `[string]` 项目名称
- options `[object]` 创建rap项目的options可选配置

返回项目信息 `[object]`

示例：
```javascript
    const rapProject = await rmx.rap.createProject(name, options)
```

---


#### rmx.rap.getRap2ModelsSingle(id) `[async]`
接口说明：获取RAP2项目信息，包含完整接口列表等

入参：
- id `[number]` 项目id

返回RAP项目信息，包含完整接口列表等 `[object]`

示例：
```javascript
    const rapProject = await rmx.rap.getRap2ModelsSingle(671)
```

---


#### rmx.rap.getRap2Action(id) `[async]`
接口说明：获取RAP项目指定接口的信息

入参：
- id `[number]` 接口id

返回RAP项目指定接口的信息 `[object]`

示例：
```javascript
    const apiInfo = await rmx.rap.getRap2Action(123456)
```

---



### 对接DEF的接口

#### rmx.def.createProject(name, group, options) `[async]`
接口说明：将项目接入DEF云构建发布系统，返回项目信息

入参：
- name `[string]` 项目名称
- group `[string]` 分组名称
- options `[object]` 接入DEF系统的options可选配置

返回项目信息 `[object]`

示例：
```javascript
    const project = await rmx.def.createProject('mmtest1', 'mm', {})
```

---

#### rmx.def.release(publishBranch, publishType, internationalCdn, isCheck, log, cwd) `[async]`
接口说明：DEF云构建发布

入参：
- publishBranch `[string]` 要发布的分支
- publishType `[string]` 发布类型，daily:日常，prod:线上
- internationalCdn `[boolean]` 是否需要同时发布到国际版，默认为false
- isCheck `[boolean]` webui使用请固定传true
- log `[function]` 发布log信息抛出
- cwd `[string]` 执行发布的目录，默认为当前目录


示例：
```javascript
    await rmx.def.release('daily/0.0.1', 'prod', true)
```

---


### 对接iconfont的接口

#### rmx.iconfont.createProject(name, options) `[async]`
接口说明：创建iconfont项目，返回项目信息

入参：
- name `[string]` 项目名称
- options `[object]` 创建iconfont项目的options可选配置

返回项目信息 `[object]`

示例：
```javascript
    const project = await rmx.iconfont.createProject('mmtest1', {})
```

---

#### rmx.iconfont.getProject(id) `[async]`
接口说明：创建iconfont项目，返回项目信息

入参：
- id `[number]` 项目id

返回项目信息 `[object]`

示例：
```javascript
    const project = await rmx.iconfont.getProject(12345)
```

---

#### rmx.iconfont.getToken() `[async]`

接口说明：获取iconfont的token信息，用来请求iconfont对外的开放api

示例：
```javascript
    const token = await rmx.iconfont.getToken()
```

---


### 对接chartpark的接口

#### rmx.chartpark.createProject(name, options) `[async]`
接口说明：创建chartpark项目，返回项目信息

入参：
- name `[string]` 项目名称
- options `[object]` 创建chartpark项目的options可选配置

返回项目信息 `[object]`

示例：
```javascript
    const project = await rmx.chartpark.createProject('mmtest1', {})
```

---


#### rmx.chartpark.getOptions(id) `[async]`
接口说明：获取指定chartpark项目的配置信息

入参：
- id `[number]` chartpark的项目id

返回：chartpark项目的配置信息 `[object]`

示例：
```javascript
    const chartOptions = await rmx.chartpark.getOptions(12345)
```

---


### 对接dataplus的接口

#### rmx.dataplus.createSpma(options) `[async]`
接口说明：自动创建一个spma字段

入参：
- options `[object]` 创建spma字段接口的options可选配置

返回：项目信息，包含spma字段 `[object]`

示例：
```javascript
    const { spma } = await rmx.dataplus.createSpma()
```

---

### 对接clue的接口

#### rmx.clue.createProject(name) `[async]`
接口说明：自动接入clue监控平台

入参：
- name `[string]` 名称

返回：clue项目信息 `[object]`

示例：
```javascript
    const project = await rmx.clue.createProject('mmtest1')
```

---


#### rmx.clue.addCustomMonitor(name) `[async]`
接口说明：添加一个自定义监控

入参：
- name `[string]` 自定义监控名称

返回：创建好的自定义监控信息 `[object]`

示例：
```javascript
    const monitor = await rmx.clue.addCustomMonitor('mmtest1-自定义监控')
```

---
