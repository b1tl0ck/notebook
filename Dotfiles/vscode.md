---
tags:
  - dotfile
create_time: 2024-05-31 21:09
modified_time: 2024-05-31 21:09
status: complete
---
# 插件

- code runner：运行代码
- error lens：在当前行显示报错信息
- better comment：注释增加颜色
- python：运行python
- github light theme
- Comment Translate

# setting.json

```json
{
    // theme
    "window.autoDetectColorScheme": true,
    "workbench.preferredLightColorTheme": "Quiet Light",
    "workbench.preferredDarkColorTheme": "Default Dark Modern",
    "workbench.iconTheme": "material-icon-theme",
    "editor.fontFamily": "FiraCode Nerd Font, 'Courier New', monospace",
    // file auto save
    "files.autoSave": "afterDelay",
    "files.autoSaveDelay": 1000,
    // file encoding
    "terminal.integrated.fontSize": 14,
    "editor.fontSize": 16,
    "editor.fontLigatures": true,
    "editor.lineHeight": 1.6,
    "debug.console.fontSize": 14,
    "editor.stickyScroll.enabled": false,
    "editor.mouseWheelZoom": true,
    "workbench.tree.indent": 24,
    "editor.cursorBlinking": "smooth",
    // title
    "window.title": "${activeEditorShort}${separator}",
    // activity bar
    "workbench.activityBar.location": "default",
    // scroll bar
    "editor.scrollbar.vertical": "auto",
    "editor.scrollbar.horizontal": "auto",
    // chinese
    "editor.unicodeHighlight.allowedLocales": {
        "zh-hans": true,
        "zh-hant": true
    },
    // well, I don't like indents
    "editor.guides.indentation": true,
    "window.titleBarStyle": "custom",

    // Better Comments
    "better-comments.tags": [
        {
            // TODO 表示有未完成的代码
            "tag": "TODO",
            "color": "#fa973a",
            "strikethrough": false,
            "underline": false,
            "backgroundColor": "transparent",
            "bold": false,
            "italic": false
        },
        {
            // HINT 用于提示协作者实现思路
            "tag": "HINT",
            "color": "#2faf64",
            "strikethrough": false,
            "underline": false,
            "backgroundColor": "transparent",
            "bold": false,
            "italic": false
        },
        {
            // NOTE 用于描述这段代码的作用
            "tag": "NOTE",
            "color": "#00b7e4",
            "strikethrough": false,
            "underline": false,
            "backgroundColor": "transparent",
            "bold": false,
            "italic": false
        },
        {
            // BUG 表示这里有问题，但还不知道怎么改
            "tag": "BUG",
            "color": "#ff0000",
            "strikethrough": false,
            "underline": false,
            "backgroundColor": "transparent",
            "bold": false,
            "italic": false
        },
    ],
    // Error Lens
    "workbench.colorCustomizations": {
        "errorLens.errorForeground": "#ff0000", // 对应 BUG
        "errorLens.warningForeground": "#fa973a", // 对应 TODO
        "errorLens.infoForeground": "#00b7e4", // 对应 NOTE
        "errorLens.hintForeground": "#2faf64", // 对应 HINT
    },
    // Code Runner
    "code-runner.saveAllFilesBeforeRun": true,
    "code-runner.executorMap": {
        "php": "php -f",
        "python": "python -u",
        "go": "go run",
    },
    // Comment Translate
    "commentTranslate.sourceLanguage": "auto",
    "commentTranslate.targetLanguage": "zh-CN",
    "commentTranslate.hover.content": false,
    "commentTranslate.multiLineMerge": true,
    "commentTranslate.hover.nearShow": false,
    "[json]": {
        "editor.defaultFormatter": "esbenp.prettier-vscode"
    },
    "[javascript]": {
        "editor.defaultFormatter": "esbenp.prettier-vscode"
    },
    "php.debug.executablePath": "D:\\CodeEnv\\php-8.3.9",
    "window.commandCenter": false,
    "security.workspace.trust.untrustedFiles": "open",
    "[python]": {
        "editor.defaultFormatter": "ms-python.black-formatter"
    },
    "terminal.external.windowsExec": "C:\\WINDOWS\\System32\\wt.exe",
    "code-runner.runInTerminal": true,
    "code-runner.terminalRoot": ""
}
```

# 额外字体（不用，容易bug）

C:\Users\zion\AppData\Local\Programs\Microsoft VS Code\resources\app\out\vs\workbench\workbench.desktop.main.css
找到.window中的font-famliy属性，改成自己想要的

原版
```css
.windows {
	font-family: Segoe WPC, Segoe UI, sans-serif
}

.windows:lang(zh-Hans) {
	font-family: Segoe WPC, Segoe UI, Microsoft YaHei, sans-serif
}

.windows:lang(zh-Hant) {
	font-family: Segoe WPC, Segoe UI, Microsoft Jhenghei, sans-serif
}

.windows:lang(ja) {
	font-family: Segoe WPC, Segoe UI, Yu Gothic UI, Meiryo UI, sans-serif
}

.windows:lang(ko) {
	font-family: Segoe WPC, Segoe UI, Malgun Gothic, Dotom, sans-serif
}
```