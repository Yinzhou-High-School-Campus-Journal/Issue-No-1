# 贡献说明

本仓库主要供我刊创刊号归档用，本文档为具体操作指南。

### 归档工作流程

- 导出 Craft 中的稿件；
- 写上元数据并 Commit（贡献）到对应板块的文件夹；

### 目前稿件目录 

- 文章/
    - 正文之外/
    - 人文社科/
    - 创意写作/
    - 其他/

### 元数据

#### 格式（YAML）

```
---
title: "文章标题"
author: "班级 作者"
author_display: "作者实际署名"
received_date: "yyyy-mm-dd"
status: "文章状态"
note: "额外备注"
---
```

#### status（状态）选项

- 校审未通过
- 已见刊

### 提交规则

1. 稿件必须使用 Markdown 格式提交；
2. 稿件开头必须包含规定元数据（不得删除 "）；
3. 禁止提交作者隐私内容（如私人联系方式）、未获授权的稿件以及与校刊无关的文件。

### Markdown 稿件注意事项

- 带空格的井号（# ）后加文字代表标题（层级）
    - 「# 」对应 Craft 中的大标题或文件名，一般出现在文首，现已被元数据替代
    - 「## 」对应副标题，较少见
    - 「### 」对应中标题，一般用于正文以外部分，如后记、注释等
    - 「#### 」对应小标题，一般用于正文之内部分，如「一、」「二、」「三、」
- 如果你对其余 Markdown 写法有疑问
    - 可以参考 GitHub Docs 官方的 [*Basic Writing and Formatting Syntax*](https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax)（《基本写作与格式语法》）。注意，请直接通过如 Chrome 的自动翻译等功能阅读英文版，而不要被「诱骗」到那个不堪入目的机翻简中版
    - 也可以参考更（最）进阶、权威的 *GitHub Flavored Markdown Spec*（《GitHub 风格的 Markdown 规范》），这是[第三方中文版](https://gfm.docschina.org/zh-hans/)，而这是[官方英文版](https://github.github.com/gfm/#insecure-characters)
- 所有双层蝌蚪引号（“”）、傻瓜引号（""）应改为单层直角引号（「」），单层蝌蚪引号（‘’）应改为双层直角引号（『』）
- 不进入 InDesign 排版的文件（如 `README.md`），为保持美观，汉字与西文字母、数字间应加入一个空格
- 进入排版流程的文件则不用添加任何空格，InDesign 的排版引擎会自动添加
- 对其他排版细则有疑问的可参考 W3C（万维网联盟，World Wide Web Consortium）的[中文排版需求](https://www.w3.org/TR/clreq/)
