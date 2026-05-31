---
share: "true"
sharepath: doc
title: lobehub定时任务
sharetitle: "202605312119"
---
定时任务的配置中很多在官方文档找不到的信息，从github的讨论或issue中摘取记录一些解决办法，办法待考证，仅为提供参考，以备不时之需

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