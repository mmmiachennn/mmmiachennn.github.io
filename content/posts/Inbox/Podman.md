---
title: ClickHouse
tags:
  - Seedling
categories:
  - Develop
date: 2025-04-21
draft: true
---

## 簡單 build 自己的 image

1. 撰寫一個 Dockerfile 或 Containerfile 的檔案

```
# 使用輕量級的 Python 映像檔
FROM python:3.11-slim

# 設定工作目錄
WORKDIR /app

# 安裝 Gemini CLI 相關套件
# 這裡以官方 SDK 為例，如果你有特定的開源 CLI 工具可自行更換
RUN pip install -U google-generativeai

# 設定環境變數（選擇性）
ENV API_KEY="你的_GEMINI_API_KEY"

# 保持容器啟動
CMD ["/bin/bash"]
```

2. 建構 image:名稱爲 gemini-sandbox
```
podman build -t gemini-sandbox .
```

3. 建立並啓動container
```
podman run -it --name my-gemini-sandbox \
  --env-file .env \
  -v D:\gemini_project:/app:Z \
  gemini-sandbox
```
