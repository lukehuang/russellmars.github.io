---
title: vs code 使用
date: 2018-06-27 11:21:48
tags: [vs code, editor]
---

vs code 开发优质插件推荐

<!--more-->

### vs code 插件推荐

#### Auto Close Tag
> 自动关闭html标签

#### Auto Rename Tag
> 重命名html标签

#### Bookmarks
> 给文件加书签/标记

#### Bracket Pair Colorizer
> 用不同的颜色标记嵌套括号

#### code Runner
> 运行不同语言的代码

#### Code Spell Checker
> 代码单词拼写检查

#### Dart
> Dart 语言支持

#### Debugger for Chrome
> 可在chrome里调试

#### Docker
> Docker相关支持插件

#### EditorConfig for VS Code
> editorConfig 插件

#### ESLint
> eslint 代码检查

#### Flutter
> Flutter开发

#### Git History
> git 历史纪录查询

#### gitignore
> .gitignore文件支持

#### Go
> Go 语言支持

#### HTML CSS Support
> html css 开发相关

#### HTML Snippets
> html 代码片段

#### JavaScript(ES6) code snippets
> es6 代码片段

#### Live Server
> 支持live reload 的本地开发服务器

#### Local History
> 本地文件的历史纪录，会在当前的项目目录下生成`.history`目录

#### Markdown All in One
> markdown 编辑预览相关

#### One Dark Pro
> 非常漂亮的主题

#### Path Intellisense
> 文件路径提示

#### Prettier - Code formatter
> prettier 格式化插件

#### Python
> python 语言支持

#### Settings Sync
> 设置同步

#### TODO Highlight
> 高亮注释里的TODO/FIXME

#### Vetur
> vue 开发插件

#### vscode wxml
> 小程序.wxml文件支持

### 用户设置
``` json
{
    "terminal.integrated.shell.windows": "D:\\app\\git\\bin\\bash.exe",
    "editor.tabSize": 2,
    "editor.fontSize": 18,
    "editor.fontFamily": "Source Code Pro, Menlo, Consolas, 'Courier New', monospace, Microsoft YaHei UI",
    "workbench.colorTheme": "One Dark Pro",
    "javascript.implicitProjectConfig.experimentalDecorators": true,
    "emmet.includeLanguages": {
        "javascript": "javascriptreact",
        "vue-html": "html"
    },
    "emmet.syntaxProfiles": {
        "jsx": {
            "self_closing_tag": true
        }
    },
    "files.associations": {
        "*.wxss": "css",
    },
    "git.enableSmartCommit": true,
    "git.confirmSync": false,
    "git.autofetch": true,
    "eslint.validate": [ "javascript", "javascriptreact", "vue", { "language": "html", "autoFix": true } ],
    "liveServer.settings.donotShowInfoMsg": true,
    "markdown.preview.fontFamily": "Source Code Pro, Menlo, Consolas, 'Courier New', monospace, 微软雅黑",
    "markdown.preview.breaks": true,
    "markdown.preview.fontSize": 18
}


```