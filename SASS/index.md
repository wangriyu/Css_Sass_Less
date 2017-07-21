# SASS

### 安裝Sass和Compass

window下安裝SASS首先需要安裝Ruby，先從官網下載Ruby并安裝，mac跳过

安装前可以选择替换gem源(与替换npm源同理)
```
//1.删除原gem源
gem sources --remove https://rubygems.org/

//2.添加國内淘寶源
gem sources -a https://ruby.taobao.org/

//3.打印是否替換成功
gem sources -l

//4.更換成功後打印如下
*** CURRENT SOURCES ***
https://ruby.taobao.org/
```
开始安装
```
//安裝如下(如mac安裝遇到權限問題需加 sudo gem install sass)
gem install sass
gem install compass
```
安装完成后输入一下命令查看是否安装成功
```
$ sass -v

$ compass -v
```
其他命令
```
//更新sass
gem update sass

//查看sass版本
sass -v

//查看sass幫助
sass -h
```
### 编译转换

```
$ sass style.scss:style.css --style expanded

$ sass style.scss:style.css --style compressed // 压缩版本
```

[SASS入门](https://www.sass.hk/en/guide.htm)

[SASS教程](https://www.sass.hk/docs/)

[Sass:@mixin指令介紹](https://www.sass.hk/skill/sass142.html)