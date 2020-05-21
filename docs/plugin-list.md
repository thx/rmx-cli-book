
###  clear

##### 简介：
一键清除chrome的hsts及dns缓存，使用说明详见：https://thx.github.io/magix-cli-book/#/clearDnsHsts

##### 用法：
`rmx clear` 

##### 参数：
- `-a, --add <hosts>` 增加域名配置，多域名以逗号分隔（如：dmp.taobao.com）
- `-c, --clear` 清除掉所有域名配置
- `-l, --list` 列出当前配置过的所有域名

##### 说明
`rmx clear` 首次执行需要用户输入域名配置，然后再执行清除的动作，输入的域名会保存在本地配置中


---


###  createDaily

##### 简介：
一键快速创建开发分支的命令

##### 用法：
`rmx createDaily` 

##### 短命令：
`rmx cd`

##### 参数：
- `-n, --name <分支名>` 创建分支的名称
- `-t, --timestamp` 是否时间戳形式

---

###  magixBuild

##### 简介：
magix框架代码的纯本地构建插件

##### 用法：
`rmx magixBuild` 

##### 参数：
- 无

##### 说明：

与`rmx build`(云端构建)不同的是，此命令为纯本地构建，即构建逻辑由插件内置，而不是走DEF平台

---

###  cert

##### 简介：
一键生成自签名证书并添加到macOS钥匙串

##### 用法：
`rmx cert` 

##### 参数：
- `--install` 生成ssl密钥和自签名证书，在系统钥匙串里添加和信任自签名证书
- `--info` 查看自签名证书信息
- `--trust` 信任自签名证书
- `-a, --add <host>` 添加要支持的域名，多个以,分隔
- `--uninstall`  删除生成的ssl密钥和自签名证书
- `-h, --help` output usage information
