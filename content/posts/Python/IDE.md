---
title: Unit Test
tags:
  - python
categories:
  - Develop
date: 2025-04-21
draft: true
---

## Pycharm

在 Linux 環境執行
```
cd to pycharm/bin
sh pycharm.sh
```

Ubuntu 下顯示中文
https://cdmana.com/2022/02/202202161357032613.html


## VSCode

### settings.json

```
    "workbench.colorTheme": "PyCharm Dark+ Theme",
    "files.autoSave": "afterDelay",
    "editor.fontSize": 16,
    "workbench.editor.autoLockGroups": {
        "jupyter-notebook": true
    },
    "flake8.args": [
        "--max-line-length=119",
        "--ignore=E402,F841,F401,E302,E305,W503"
    ],
    "python.analysis.autoFormatStrings": true,
    "python.analysis.inlayHints.callArgumentNames": "partial",
    "python.analysis.typeCheckingMode": "basic",
    "python.analysis.inlayHints.functionReturnTypes": true,
    "python.analysis.inlayHints.variableTypes": true,
    "terminal.integrated.fontSize": 14,
    "editor.minimap.enabled": false,
    "python.languageServer": "Pylance",
    "explorer.confirmDelete": false,
    "explorer.confirmDragAndDrop": false,
	"editor.renderWhitespace": "all",
	"editor.indentSize": "tabSize",
	"editor.tabSize": 4,
	"workbench.settings.applyToAllProfiles": [
		"editor.tabSize",
		"editor.insertSpaces"
    ],
```