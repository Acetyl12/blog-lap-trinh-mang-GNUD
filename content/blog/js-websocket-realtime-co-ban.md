---
title: "WebSocket cơ bản: realtime trong JS/Node"
date: 2025-10-09
tags: ["JavaScript", "WebSocket", "Realtime"]
categories: ["JavaScript"]
---

WebSocket cho kết nối **2 chiều** thời gian thực (chat, dashboard).  
Frontend: `new WebSocket(url)`; Backend: `ws` (Node) hoặc Socket.IO (có fallback & rooms).  
Lưu ý: xác thực (JWT qua query/header), ping/pong, auto-reconnect, phân kênh (rooms), và hạn mức message size.
