### 辅助开发套件(插件)的SDK

rmx-cli实现了一个专门用来辅助开发套件(插件)的套件 `rmx-dev`

### 用法

#### 初始化

执行 `rmx init rmx-dev` 选择开发类型(套件/插件)

<img src="https://img.alicdn.com/tfs/TB1fiF2oKH2gK0jSZFEXXcqMpXa-538-164.png" width="36%">

随后填写资料即可初始化好一个套件(插件)

<img src="https://img.alicdn.com/tfs/TB1M7VZoFT7gK0jSZFpXXaTkpXa-1088-678.png" width="70%">


#### 本地开发

执行 `rmx dev` 会将当前套件(插件)link到rmx-cli的套件(插件)安装目录下，接下来就可以直接调试你的套件(插件)了

<img src="https://img.alicdn.com/tfs/TB1CxJ2oHr1gK0jSZR0XXbP8XXa-1170-910.png" width="70%">

套件调试开发：
  - 运行 `rmx init [你的套件名]`，即可执行当前套件里的init命令
  - 在你init完的项目里，必须有配置rmxConfig: { "kit": "[你的套件名]"}
  - 其他套件必要的命令逐一实现调试即可 (init, dev, add, test等等)

插件调试开发：
  - 运行 `rmx [你的插件名]`，即可执行当前插件里的代码

退出开发模式：
  - 运行 `rmx dev --exit` 即可取消link，退出本地开发模式



#### 发布

本地开发调试完毕，执行 `rmx publish` 即可发布当前套件(插件)到tnpm上 

<img src="https://img.alicdn.com/tfs/TB1ZwF5oQL0gK0jSZFAXXcA9pXa-1164-1218.png" width="70%">


> ps: 不需要手动修改package.json里的version，系统会自动在原来的版本上+1

发布完毕，就可以将你开发好的套件(插件)信息提交给 `@崇志`，加入到rmx-cli的可用套件(插件)里了。



