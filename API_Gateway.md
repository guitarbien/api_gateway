autoscale: true
build-lists: true
slidenumbers: true
theme: Next

# Serverless Architectures on AWS
## Chapter 7 API Gateway

^ 自我介紹，說明今天分享的流程

---

### Previously
## Ch1. Going serverless

Principles :

- Use compute service
- Write single-purpose stateless functions
- Event-driven
- More powerful frontend
- Embrace 3rd party services

^
雲端服務的興起對 IT 基礎建設和軟體開發有很大的衝擊。
本章介紹了 serverless 和傳統架構的比較，並非都沒有缺點，

---

### Previously
## Ch2. Architechures and patterns

- Use cases
- Architectures
    - Compute as back end
    - Compute as glue
- Patterns
    - Messaging pattern
    - Fan-out pattern

^
介紹 use case、
架構、
pattern，
compute as backend => 使用 serverless 的 Lambda/function 和 3rd 做後端開發
Compute as backend 的目標之一是不需要把所有事情都藏在後端，前端在考慮到安全性之下也可以直接跟 service db 溝通。
compute as glue 在講 pipeline workflow，大隊接力。
在真的開始做產品前可以多參考現實的已知範例。
使用 SQS SNS 做 message pattern 或 fan-out pattern
(實際成功案例的經驗十分值得參考)

---

### Previously
## Ch3. Building a serverless application

- IAM
- S3
- Elastic Transcoder
- Lambda
- SNS & multiple subscriber

^
正式進入 AWS console
24Hour Video
(先講 list 再講 24h 應用)
上傳影片、轉檔、轉成功發通知信、產生影片 meta data、將影片設定為 public read


---

### Previously
## Ch4. Setting up your cloud

- Security
- Log
- Alert
- Billing

^
IAM 有 group, role, policy, permission
用 cloudWatch 看 log，S3 也可以自動記 log
billing 成本控管成本預估、監控

---

### Previously
## Ch5. Authentication and authorization

- JWT
- Auth0
- **24Hour Video** website
- Delegation token
- Custom authorizer

^
認證和授權
會員才可以享用 24Hour Video 的功能
Login -> API Gateway -> Lambda -> get-user-info

---

<!-- [.autoscale: false] -->

### Previously
## Ch6. Lambda the orchestrator

啟動模式：

- Event ( push & pull )
- Request

^ Event (aws)
Request (api gateway、console、cli)

---

### Previously
## Ch6. Lambda the orchestrator

運行模式 :

- Waterfall
- Series
- Parallel

^ npm Async

---

### Previously
## Ch6. Lambda the orchestrator

Lambda 設定 :

- Version
- Alias
- Environment variable

^ Alias => Dev, Stg, Prod

---

### Previously
## Ch6. Lambda the orchestrator

- AWS CLI
- Local testing

^ npm : Mocha、Chai、Sinon、Rewire
for Testing、TDD、Mock

---

## This chapter covers

- Creation and management API Gateway resources and methods
- Lambda Proxy integration
- API Gateway caching, throttling, and logging

^
Resource & Method 後面會補充
以最簡單的方式串接 API Gateway 和 Lambda

---

## 回顧到目前的實作

1. (Uploading to S3)
2. Elastic Transcoder, output to another S3
3. Send notification
4. Generate meta data
5. Set permission
6. Auth0
7. Get video list

---

## Demo

^
目前省略了書中
使用 SES 發送 email
測試 API Gateway 存取限制是否有效
get-video-list
Auth0 JWT
user-profile

---
​
​![fit](./images/get-video-list.png)

---

​![fit](./images/auth0-login.png)

---

​![fit](./images/get-user-profile.png)

^ Proxy resource & CORS
Proxy integration V.S. manual mapping

---

## Integration with AWS services

1. Lambda function
2. HTTP Proxy
3. AWS Service Proxy
4. Mock Intergration

^ 可在 request forward 到 endpoint 之前加料
把 request 直接 forward 到 aws services，http method 可以直接對應到 service 的行為，例如在 DynamoDB 新增資料
不必再串接其他服務，讓 API Gateway 直接回傳定義好的 response

---

## Features of the API Gateway

1. Caching
2. Throttling (節流)
3. Logging
4. Staging (環境別)
5. Versioning
6. Scripting

^ 還在想

---

## Caching

- 減少 latency 等待時間
- 分擔後端 loading

---

![fit](./images/7-14.jpg)

^ 可以設定 0.5 GB ~ 237 GB (每小時計費，0.5 GB: \$0.020 / hour，237GB: \$3.800 / hour)
^ 可以設定 TTL (time-to-live) 秒

---

## Throttling (節流)

- 可限制後端每秒被呼叫的次數
- 可預防阻斷式攻擊 (denial-of-service attacks)

---

![fit](./images/7-11.jpg)

^ 可以設定 rate and burst limit
^ rate: API Gateway 允許一個 method 每秒被呼叫的平均次數
^ burst limit: API Gateway 允許一個 method 被呼叫的最大次數

^ 書: 一個 AWS 帳戶可以設定 API Gateway 的 steady-state request rate to 1000 requests per second
(rps) and allows bursts of up to 2000 rps across all APIs, stages, and methods，想要增加 default 值可以跟 AWS 要

^ 官網: By default, API Gateway limits the steady-state request rate to 10,000 requests per second (rps). It limits the burst (that is, the maximum bucket size) to 5,000 requests across all APIs within an AWS account.

---

## Logging

- 可以使用 CloudWatch 紀錄 request and response，如 cache hits & misses

---

![fit](./images/7-13.jpg)

^ API Gateway 建議都設置 CloudWatch Logs and CloudWatch Metrics

^ 設置剛提到的那兩項還需要設定 IAM 權限

^ log levels: error and info, metrics: API calls, latency, and errors

---

## Staging

- 建立環境別，例如 development、UAT、production
- 每個 API 可以有 10 個環境別、每個帳號可以有 60 個 API
- API 可同時佈署不同的環境別，各自有自己的 URL
- 可以設定 stage variable，就像環境變數
- 設定好以後這樣用 `${stageVariables.<variable_name>}`

^ API gateway 根據不同環境設定的 stage variable 去呼叫不同的 lambda function 或 http endpoint

---

![fit](./images/7-18.jpg)

---

![fit](./images/7-19.jpg)

---

## Versioning

- 每次佈署 API 都會產生一個新的 version
- 可以設定不同 stage 對應到不同 version 的 API

---

![fit](./images/7-21.jpg)

^ 透過 Stage Editor 裡的 Deployment History tab 來 rollback

---

## Scripting

- 使用 Swagger to script API (export / import)

- 官網: [swagger.io](https://app.swaggerhub.com/apis/guitarbien6/ooxx/1.0.0)

- 官網教學: https://swagger.io/getting-started-with-the-amazon-swagger-importer/

^ 使用 Swagger 把 API 變成 script，除了更容易佈署，還可以產生文件

---