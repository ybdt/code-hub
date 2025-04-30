# 拿到一个PHP系统，如何审计出0day
跟把大象装冰箱里步骤是一样的，路由分析 -> 鉴权审计 -> 危险函数 -> 从危险函数到用户输入
## 0x01 路由分析
```
# ThinkPHP路由学习
具体见 -> ThinkPHP路由学习.md
```
## 0x02 鉴权审计
```
# 思路1
cookie是否可以构造

参考链接：
https://mp.weixin.qq.com/s/47vfZ1v3j6ML22T9tJd4VA
```
## 0x03 危险函数
### 01 文件上传漏洞
```
文件上传函数或变量：
move_uploaded_file、$_FILES

参考：
https://www.php.net/manual/zh/function.move-uploaded-file.php
https://www.php.net/manual/zh/reserved.variables.files.php
https://www.php.net/manual/zh/features.file-upload.post-method.php

漏洞演化：
从2005年左右的无过滤，到2015年左右后缀黑名单过滤，再到2025左右后缀白名单过滤，越来越安全了，意味着漏洞越来越难挖了（以上时间是我估计瞎说的，不必太当真）

过滤演化：
无任何验证 -> 前端JS验证 -> 验证文件头、验证Content-Type-> 黑名单验证 -> 白名单验证
```
### 02 命令执行漏洞
```
命令执行函数：
system

Windows下命令拼接符：
& -> command1 & command2 -> command1执行成功或失败，都会执行command2
&& -> command1 && command2 -> command1执行成功，才执行command2
|| -> command1 || command2 -> command1执行失败，才执行command2
| -> command1 | command2 -> 将command1执行结果传递给command2，经测试，command1执行成功，才执行command2

Linux下命令拼接符：
& -> command1 & command2 -> 将command1放入后台执行，然后执行command2
&& -> command1 && command2 -> command1执行成功，才执行command2
|| -> command1 || command2 -> command1执行失败，才执行command2
| -> command1 | command2 -> 将command1执行结果传递给command2，经测试，command1执行成功或失败，都会执行command2
; -> command1 ; command2 -> command1执行成功或失败，都会执行command2

Windows下反弹Shell：

Linux下反弹Shell：

Windows下写入Webshell：

Linux下写入Webshell：

参考：
https://www.php.net/manual/zh/function.system.php
https://blog.csdn.net/Thunderclap_/article/details/129177629
```

### 03 代码执行漏洞
```
代码执行函数：
eval
asset
```

### 04 文件读取漏洞
```
文件读取函数：
fopen 

参考：
https://www.php.net/manual/zh/function.fopen.php
```

### 05 文件包含漏洞
```
文件包含函数：
include、include_once、require、require_once
```
## 0x04 从危险函数到用户输入
```
就是从危险函数一点一点回溯审查代码
```