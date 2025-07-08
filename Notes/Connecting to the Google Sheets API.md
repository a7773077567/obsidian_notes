---
category: note
type: tip
tags:
  - google
  - sheet
  - api
  - gcp
  - sdk
description: 如何連接 Google Sheets API
resource_1: https://www.youtube.com/watch?v=PFJNJQCU_lo
resource_2: https://www.youtube.com/watch?v=K6Vcfm7TA5U
resource_3: https://developers.google.com/workspace/sheets/api/guides/concepts
---

> [!WARNING] Frontend vs.  Backend
> 要先確立好要前前端還是後端使用 Google Sheets API，這會影響到 GCP 的設定、授權流程及使用 Library

如果由後端存取 Sheets，使用 Service Accounts 的方式驗證及使用 googleapis 套件；如果是直接在前端存取的話則會使用 OAuth2 驗證及使用 gapi.client + gsi 套件。

文件可以參考 -  `= this.resource_3`
使用 Express 示範的 YT - `= this.resource_3`
