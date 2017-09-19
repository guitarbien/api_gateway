autoscale: true
build-lists: true
slidenumbers: true
footer: Senao RD3
theme: Next

# Serverless Architectures on AWS
## Chapter 7 API Gateway

---

## Previously

1. Going serverless
2. Architechures and patterns
3. Building a serverless application
4. Setting up your cloud
5. Authentication and authorization
6. Lambda the orchestrator

---

## This chapter covers

- Creation and management API Gateway resources and methods
- Lambda Proxy integration
- API Gateway caching, throttling, and logging

---

## 回顧之前的實作

1. Uploading to S3
2. Elastic Transcoder, output to another S3
3. Send notification
4. Generate meta data
5. Set permission
6. auth0

---

## Simple Demo

^ 此功能沒有照書本完全實作

---

^ 先看本章最後實作出的結果，傳統方式要做出這樣的功能如何達成
^ 帶出這個功能有什麼重點，要如何在 AWS 上實作

## 有什麼重點

- auth
- endpoint
- get the video

---

## RESTful style endpoint

---

## Resources & Method

^ Proxy resource & CORS

^ Proxy integration V.S. manual mapping

---

## API Gateway as the interface

Figure 7.1

---

## Integration with AWS services

1. Lambda function
2. HTTP Proxy

^ 可在 request forward 到 endpoint 之前加料

3. AWS Service Proxy

^ 把 request 直接 forward 到 aws services，http method 可以直接對應到 service 的行為，例如在 DynamoDB 新增資料

4. Mock Intergration

^ 不必再串接其他服務，讓 API Gateway 直接回傳定義好的 response

---

## Features of the API Gateway

1. Caching

^ 減少 latency 等待時間 以及 分擔後端 loading

2. Throttling (節流)

^ 可限制後端每秒被呼叫的次數

3. Logging

^  可以使用 CloudWatch 紀錄 request and response，如 cache hits & misses

4. Staging (環境別)

^ 建立環境別，例如 development、testing、production，每個 API 可以有十個環境別、每個帳號可以有 60 個 API，根據 stage variable 可以設定不同的環境去呼叫不同的 lambda 或 http endpoint

5. Versioning

^ 每次佈署 API 都會產生一個新的 version，並且可以設定不同 stage 對應到不同 version 的 API

6. Scripting

^ 使用 Swagger to scripting API (export/import)

---

## Caching

Figure 7.14

可以設定 0.5 GB ~ 237 GB
每小時計費，0.5 GB $0.020 / hour，237GB $3.800 / hour
可以設定 TTL (time-to-live) 秒

---

## Throttling

Figure 7.11

可以設定 rate and burst limit
rate: API Gateway 允許一個 method 每秒被呼叫的平均次數
burst limit: API Gateway 允許一個 method 被呼叫的最大次數
default: 一個 AWS 帳戶可以設定 API Gateway 的 steady-state request rate to 1000 requests per second (rps) and allows bursts of up to 2000 rps across all APIs, stages, and methods

^ 書: API Gateway sets the “steady-state request rate to 1000 requests per second

(rps) and allows bursts of up to 2000 rps across all APIs, stages, and methods within an
AWS account，想要增加 default 值可以跟 AWS 要

^ 官網: By default, API Gateway limits the steady-state request rate to 10,000 requests per second (rps). It limits the burst (that is, the maximum bucket size) to 5,000 requests across all APIs within an AWS account. In API Gateway, the burst limit corresponds to the maximum number of concurrent request submissions that API Gateway can fulfill at any moment without returning 429 Too Many Requests error responses.

---

## Logging

Figure 7.13

API Gateway 建議都設置 CloudWatch Logs and CloudWatch Metrics
需要設定 IAM 權限

^ log: error and info, metrics: API calls, latency, and errors

---

## Staging

每個 API 可以有十個環境別、每個帳號可以有 60 個 API
可設定 stage variable，就是環境變數

^ 用來 mapping templates, 傳給 Lambda functions, HTTP... 等等

${stageVariables.<variable_name>}

Figure 7.18
Figure 7.19
Figure 7.20

---

## Versioning

Figure 7.21

^ 透過 Stage Editor 裡的 Deployment History tab 來 rollback

---

## Scripting

---