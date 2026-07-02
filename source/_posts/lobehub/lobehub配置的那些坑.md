---
share: "true"
sharepath: doc
title: 定时任务中redis异常问题
sharetitle: "定时任务中redis异常问题"
---

`[Agent Runtime Redis] Redis connection error: ReplyError: NOAUTH Authentication required.` 

`REDIS_URL`中需要配置密码才行

```
REDIS_URL=redis://:yourpassword@localhost:6379
```
