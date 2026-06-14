# Readiness Checklist – Lab 05

Đây là danh sách kiểm tra (checklist) để đảm bảo stack Docker Compose của bạn đã sẵn sàng trước khi gửi bài. Hãy tick vào mỗi mục sau khi hoàn thành.

- [x] **Database ready:** container DB đã chạy và phản hồi `pg_isready`. Kiểm tra bằng `docker exec -it fit4110-db-lab05 pg_isready -U $POSTGRES_USER`.
  - Status: ✅ /var/run/postgresql:5432 - accepting connections
  - Evidence: reports/db-logs.txt
- [x] **AI service ready:** container AI service trả về `200` cho endpoint `/health` và `/predict` hoạt động.
  - Health: ✅ {"status":"ok","service":"ai-service","version":"0.5.0"}
  - Predict: ✅ {"objects":["person","bicycle"],"confidence":[0.98,0.85]}
  - Evidence: reports/ai-health.json, reports/ai-service-logs.txt
- [x] **API ready:** container API trả `200` cho `/health` và các endpoint hoạt động khi token hợp lệ.
  - Health: ✅ {"status":"ok","service":"iot-ingestion","version":"0.5.0"}
  - Endpoints: ✅ GET /health (200), GET /readings/latest (200)
  - Evidence: reports/api-health.json, reports/api-logs.txt
- [x] **Environment variables:** `.env` đã được thiết lập đúng (APP_PORT, POSTGRES_USER, AUTH_TOKEN,…). Không sử dụng secret thật; lưu secret vào `.env` cục bộ, commit `.env.example`.
  - .env.example: ✅ Không chứa secret thật
  - .env cục bộ: ✅ Thiết lập cho local development
- [x] **Network & Ports:** mạng `team-internal` hoạt động; API gọi được AI bằng hostname `ai-service`; ports 8000 (API), 9000 (AI) và 5432 (DB) được map đúng.
  - Network: ✅ team-internal bridge
  - Ports: ✅ API:8000, AI:9000, DB:5432
  - Evidence: reports/docker-ps.txt
- [ ] **Image tags:** bạn đã build image với tag `v0.1.0-<team>` và push lên registry (ghcr.io hoặc Docker Hub). Xác nhận rằng tag xuất hiện trong registry.

Ghi chú thêm những vấn đề gặp phải hoặc điều chỉnh tại đây:

- ✅ Docker Compose stack hoạt động với 3 services (API, DB, AI-Service)
- ✅ Postman collection + Newman test: 7/7 assertions passed
- ✅ Tất cả endpoints health check return 200 OK
- ✅ Readiness checks pass: DB accepting connections, AI service responding, API healthy
- ✅ Environment variables đúng (.env.example committed, .env.local in .gitignore)
- ✅ Docker network team-internal + ports mapping chính xác
- ✅ Reports folder có đầy đủ evidence: logs, health checks, Newman HTML/JSON report
- ✅ .gitignore setup
- ⏳ Image push to registry (optional, can do with: docker tag + docker push)