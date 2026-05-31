---
share: "true"
sharepath: doc
title: lobehub定时任务
sharetitle: "202605312119"
---
定时任务的配置中很多在官方文档找不到的信息，从github的讨论或issue中摘取记录一些解决办法，办法待考证，仅为提供参考，以备不时之需

# 配置1
```

# 如没有特殊需要不用更改
LOBE_PORT=3210
APP_URL='https://lobe.xxxxxx.com'
# 内部应用URL，用于容器内部服务间通信
# Docker Compose 部署时必须配置，否则当 APP_URL 为宿主机 IP 时 AI 生图等功能会失败
# INTERNAL_APP_URL=https://localhost:3210
INTERNAL_APP_URL='https://lobe.xxxxxx.com'
UPSTASH_WORKFLOW_URL='https://lobe.xxxxxx.com'

# QStash 配置
QSTASH_URL="https://qstash-us-east-1.upstash.io"
QSTASH_TOKEN="xxxxxxDEzYTQyMDU4MzQwZDY2ODZhOTllMDY0In0="
QSTASH_CURRENT_SIGNING_KEY="xxxxxx"
QSTASH_NEXT_SIGNING_KEY="xxxxxx"

# --- 记忆 ---
# 过滤器，Gate Keeper，强制指定过滤器的语言
MEMORY_USER_MEMORY_GATEKEEPER_LANGUAGE='简体中文'
# 强制指定模型，优先级高于提供商和模型偏好设置
MEMORY_USER_MEMORY_GATEKEEPER_MODEL='deepseek-v4-flash'
MEMORY_USER_MEMORY_GATEKEEPER_API_KEY='sk-xxxxxx'
MEMORY_USER_MEMORY_GATEKEEPER_BASE_URL='https://opencode.ai/zen/go/v1'
MEMORY_USER_MEMORY_GATEKEEPER_PROVIDER='openai'

# （可选）
# 如果使用记忆的模块的用户配置了多个提供商，这里可以选择配置如何
# 匹配 提供商，比如 openai,deepseek 这样，可以只写 openai
MEMORY_USER_MEMORY_GATEKEEPER_PREFERRED_PROVIDERS='openai'

# （可选）
# 如果使用记忆的模块的用户配置了多个提供商然后激活了多个模型，这里
# 可以选择配置如何 匹配 模型，如果没有配置就会跳到下一个 model id
# 比如 OpenRouter 的 OpenAI 模型叫 openai/gpt-5.4-mini 的话，这里就写 openai/gpt-5.4-mini 就好了
MEMORY_USER_MEMORY_GATEKEEPER_PREFERRED_MODELS='deepseek-v4-flash'

# 提取器，Layer Extractor
# 强制指定模型，优先级高于提供商和模型偏好设置
# 上下文限制，比如 27200 的 token 限制的话，推荐设置成 60%~75% 的，剩下的部分可以给生成的内容用
MEMORY_USER_MEMORY_LAYER_EXTRACTOR_CONTEXT_LIMIT='500000'
MEMORY_USER_MEMORY_LAYER_EXTRACTOR_MODEL='deepseek-v4-flash'
MEMORY_USER_MEMORY_LAYER_EXTRACTOR_API_KEY='sk-xxxxxx'
MEMORY_USER_MEMORY_LAYER_EXTRACTOR_BASE_URL='https://opencode.ai/zen/go/v1'
MEMORY_USER_MEMORY_LAYER_EXTRACTOR_PROVIDER='openai'
MEMORY_USER_MEMORY_LAYER_EXTRACTOR_PREFERRED_PROVIDERS='openai'
MEMORY_USER_MEMORY_LAYER_EXTRACTOR_PREFERRED_MODELS='deepseek-v4-flash'

# 向量化，Embedding
# token 限制，比如 OpenAI 家这个 embedding 模型只有 8192 的话，推荐设置成 75%~90% 的
MEMORY_USER_MEMORY_EMBEDDING_CONTEXT_LIMIT='20000'
MEMORY_USER_MEMORY_EMBEDDING_API_KEY='sk-xxxxxxpqz'
MEMORY_USER_MEMORY_EMBEDDING_BASE_URL='https://api.siliconflow.cn/v1'
# 这里以 OpenAI 的 text-embedding-3-large 模型为例，实际使用时请替换成你要使用的模型
MEMORY_USER_MEMORY_EMBEDDING_MODEL='Qwen/Qwen3-Embedding-0.6B'
MEMORY_USER_MEMORY_EMBEDDING_PROVIDER='openai'
MEMORY_USER_MEMORY_EMBEDDING_PREFERRED_PROVIDERS='openai'
MEMORY_USER_MEMORY_EMBEDDING_PREFERRED_MODELS='Qwen/Qwen3-Embedding-0.6B'

# # ============ Webhook（QStash 回调地址，必须公网可达）============
MEMORY_USER_MEMORY_WEBHOOK_BASE_URL='https://lobe.xxxxxx.com'

```

# 配置2
**对于自部署用户，目前有以下选项**：

1. **接入 QStash（目前唯一的生产可用方案）**：注册 [Upstash QStash](https://upstash.com/)（有免费额度），然后配置以下环境变量 [[8]](https://github.com/lobehub/lobehub/blob/2eb7ee824f99abae72ccd1af3dabf3b4f49737c0/src/libs/qstash/index.ts)：
    
    - `QSTASH_TOKEN`
    - `QSTASH_CURRENT_SIGNING_KEY`
    - `QSTASH_NEXT_SIGNING_KEY`
    - `AGENT_RUNTIME_MODE=queue`
    - `APP_URL`（你的 LobeHub 实例地址）
2. **等待官方支持独立的自托管调度方案**：目前没有公开的时间线。

# 配置3 据说是有效的方式

```
 cron-dispatcher:
    image: alpine/curl:latest
    container_name: lobe-cron-dispatcher
    restart: always
    volumes:
      - /etc/localtime:/etc/localtime:ro
    environment:
      - CRON_SCHEDULE=0,30 * * * *
      - TARGET_URL=http://lobe:${LOBE_PORT:-3210}/api/workflows/task/schedule-dispatch
      - DRY_RUN=false
    entrypoint: sh -c
    command:
      - >
        echo "$$CRON_SCHEDULE curl -fsSL -X POST -H 'Content-Type: application/json' -d \"{\\\"dryRun\\\":$$DRY_RUN}\" $$TARGET_URL" > /var/spool/cron/crontabs/root &&
        crond -f -L /dev/stdout
    networks:
      - lobe-network
        
```