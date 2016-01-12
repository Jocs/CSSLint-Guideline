#### CSSLint 基本使用说明

#### 一、命令行中使用CSSlint

1. 全局安装csslint

> npm install -g csslint

1. 将目录中的`.csslintrc` 配置文件拷贝到你项目的根目录下
2. 使CSSLint运行起来

命令行中运行如下命令

> csslint [option]\[file|dir]*

举个栗子：

> csslint file1.css directory

可选的option请参见https://github.com/CSSLint/csslint/wiki/Command-line-interface

#### 二、其他环境中使用CSSLint

在构建工具中使用CSSLint请参见：

https://github.com/CSSLint/csslint/wiki/Build-System-Integration

在IDE中使用CSSLint请参见

https://github.com/CSSLint/csslint/wiki/IDE-integration
