---
title: "Fetch vs Axios: gọi API hiệu quả"
date: 2025-10-09
tags: ["JavaScript", "HTTP", "Axios"]
categories: ["JavaScript"]
---

`fetch` là native, `axios` tiện alias & interceptor. Best-practices:
- Tạo **HTTP client** dùng chung (baseURL, timeout, headers).
- Interceptor xử lý token & lỗi 401/403.
- Chuẩn hoá response (success/data/error), có retry/backoff cho lỗi mạng tạm thời.
