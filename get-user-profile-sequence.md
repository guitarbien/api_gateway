 Auth0 取得 User Profile (with custom authorizer)

```sequence
Browser->API Gateway: JWT in header
API Gateway->Lambda A: verify JWT
Note right of Lambda A: verify.......
Lambda A-->API Gateway: OK
API Gateway->Lambda B: JWK
Lambda B->Auth0: JWK
Auth0-->Lambda B: JSON
Note over API Gateway, Lambda B:Lambda Proxy Integration
Lambda B-->API Gateway: JSON
API Gateway-->Browser: JSON
```

