取得影片資訊

```sequence
Browser->API Gateway:我要 720P 的影片
API Gateway->Lambda:我要 720P 的影片
Note right of Lambda:找出 S3 的所有 720P 的影片
Lambda-->API Gateway:JSON
API Gateway-->Browser:JSON
Note left of Browser:用 JSON 內的影片網址產生出 <video>
```

