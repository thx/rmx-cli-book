
###  clear

##### 简介：
一键清除chrome的hsts及dns缓存

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