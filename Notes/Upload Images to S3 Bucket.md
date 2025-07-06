---
category: note
type: tip
tags:
  - aws
  - s3
  - form_data
  - image
  - file
  - cloud
description: 上傳圖片至 AWS S3 Bucket
resource_1: https://www.youtube.com/watch?v=yGYeYJpRWPM
resource_2: https://docs.aws.amazon.com/AmazonS3/latest/userguide/PresignedUrlUploadObject.html
resource_3: https://aws.amazon.com/tw/blogs/compute/uploading-to-amazon-s3-directly-from-a-web-or-mobile-application/
---
S3 全名為 Simple Storage Service， 它是 AWS 提供的其中一種服務，可以用來儲存各種 object，例如檔案、影片、圖片等等...，而儲存這些 object 的容器就稱為 bucket，我們可以透過 AWS 提供的 SDK 來完成這些行為。

以往儲存檔案的方式如下：
1. 使用者上傳檔案至伺服器
2. 伺服器將檔案暫存在某個空間並進行處理
3. 伺服器將檔案永久儲存到資料庫、檔案伺服器或者物件儲存庫
![[s3-1.png]]

這個方式很消耗伺服器的資源及效能，因此將處理檔案這件事直接交給第三方的物件儲存庫逐漸成為主流，以下為 AWS S3 的流程：
1. 使用者呼叫 Amazon 的 API API Gateway endpoint，它會觸發 getSignedURL 這個 Lambda 函式，從 S3 bucket 拿到一個短暫的 signed URL
2. 使用者可以直接透過這個 signed URL 將檔案傳到 S3 bucket
![[s3-2.png]]

實務上的流程可能如下：
1. 於 AWS Console 設定好各種關於 S3 的設定
2. 後端實作取得 preSigned URL 的邏輯
3. 前端發送 API 向後端取得 preSigned URL
4. 前端直接將檔案上傳至 S3 Bucket
5. 前端將上傳成功後的 S3 檔案 URL 透過 API 發送至後端
6. 後端將檔案 URL 儲存至資料庫
7. 前端拿取資料時，後端回傳此 URL

可能關於 S3 的各種設定？：
1. 使用者可操作的 Method - PUT、GET、DELETE
2. 