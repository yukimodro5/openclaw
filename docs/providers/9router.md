---
summary: "Kết nối OpenClaw (Docker) tới API 9router chạy trên host"
read_when:
  - Bạn chạy OpenClaw bằng Docker
  - Bạn cần OpenClaw gọi API của 9router
  - Bạn cần cấu hình host.docker.internal trên Linux
title: "9router"
---

# 9router

## Lưu ý quan trọng

Khi chạy **OpenClaw trong Docker** và gọi API **9router** trên máy host, cần thêm cấu hình:

```yaml
extra_hosts:
  - "host.docker.internal:host-gateway"
```

Nếu thiếu dòng này, container có thể không resolve được `host.docker.internal` (đặc biệt trên Linux).

## Ví dụ docker-compose

```yaml
services:
  openclaw:
    image: ghcr.io/openclaw/openclaw:latest
    container_name: openclaw
    extra_hosts:
      - "host.docker.internal:host-gateway"
    environment:
      # Đổi port theo service 9router của bạn
      OPENAI_BASE_URL: "http://host.docker.internal:3000/v1"
      OPENAI_API_KEY: "<your-9router-api-key>"
```

## Gợi ý kiểm tra nhanh

Từ trong container OpenClaw, kiểm tra truy cập API 9router:

```bash
curl http://host.docker.internal:3000/v1/models
```

Nếu nhận được JSON hợp lệ, kết nối Docker -> 9router đã hoạt động.
