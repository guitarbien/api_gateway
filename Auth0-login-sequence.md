Auth0 登入

```sequence
Browser->Auth0:給我登入畫面
Auth0-->Browser:UI
Browser->Auth0:決定要用 Google 登入
Auth0->Google:這個人是你的會員嗎
Note right of Google:判斷中...
Google-->Auth0:yes
Auth0-->Browser:JWT
Note left of Browser:已登入(toggle UI)
```

