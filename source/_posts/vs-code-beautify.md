---
title: vs code 美化
date: 2018-12-10 13:23:34
tags: [vs code, editor]
---
写代码时优秀的字体和高亮会让人很舒服，使用vscode的时候非常喜欢一款主题`One Dark Pro`，高亮，颜色以及背景色都很舒服。
还有一个代码格式化插件也非常棒`Prettier`，一键格式化，并且支持众多文件类型，最重要的是有对应的es-lint插件，简直完美。
还有推荐三款编程字体
+ Operator Mono：非常好看的编程字体
+ Source Code Pro：adobe公司出品的等宽编程字体（在没使用Operator Mono之前，一直使用这个）
+ Microsoft YaHei UI：这个不多说，经典，window自带

字体使用的时候需要把字体文件都放到`C:\Windows\Fonts\`文件夹下，系统会默认归档同一个字体的字体文件。
`editor.fontFamily`的值设置为`Operator Mono, Source Code Pro, Menlo, Consolas, 'Courier New', monospace, Microsoft YaHei UI`，找到`C:\Users\用户名\.vscode\extensions\zhuangtongfa.material-theme-2.17.7\themes\OneDark-Pro.json`，
在最后加上一段，配置斜体（Operator Mono的斜体真好看）
``` json
{
  "name": "Italics",
  "scope": [
    "emphasis", 
    "storage", 
    "comment", 
    "entity.other.attribute-name", 
    "entity.other.attribute-name.html", 
    "entity.other.attribute-name.tag.jade", 
    "entity.other.attribute-name.tag.pug", 
    "markup.italic", 
    "keyword.control", 
    "variable.language"
  ],
  "settings": {
    "fontStyle": "italic"
  }
}
```

这样你就拥有了`One Dark Pro`舒服的配色，`Operator Mono`好看的字体，省心的代码格式化`Prettier`

字体[下载](https://github.com/russellmars/russellmars.github.io/releases/download/1.0/fonts.zip)