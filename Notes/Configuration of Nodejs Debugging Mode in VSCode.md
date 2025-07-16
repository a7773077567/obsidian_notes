---
category: note
type: tip
tags:
  - node
  - debug
  - vscode
description: 在 VSCode 中設定 Nodejs debug 模式的 config
resource_1: https://code.visualstudio.com/docs/nodejs/nodejs-debugging
---
可以參考 VSCode 的說明文件看更詳細的資訊 - 
`= this.resource_1`

我們可以從 VSCode 的側邊欄 - Run and Debug 進入 debug 模式，我們可以從 run 按鈕旁的下拉選單選擇 add configuration 來設定 debug 模式的 config。當我們一進入設定便會產生一個被放在 .vscode 資料夾下的launch.json 檔案，這個檔案便是用來設置 debug 的配置。

我們會看到如下的配置結構：
```json
{
  // Use IntelliSense to learn about possible attributes.
  // Hover to view descriptions of existing attributes.
  // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
  "version": "0.2.0",
  "configurations": [
    {
      "type": "node",
      "request": "launch",
      "name": "Launch Program",
      "skipFiles": ["<node_internals>/**"],
      "program": "${workspaceFolder}/app.js",
      "restart": true, // 當我們更改程式碼時，debug 也會重新 run
      "runtimeExecutable": "nodemon", // 使用 nodemon 而非 node 來 run script
      "console": "integratedTerminal" // 會將 log 印在 VSCode 的 terminal 上
    }
  ]
}
```

