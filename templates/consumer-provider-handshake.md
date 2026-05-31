# Consumer–Provider Handshake

## Thông tin chung

- **Lab**: FIT4110 Lab 03
- **Ngày**: 2026-05-31
- **Provider team**: `team-iot` (IoT Ingestion)
- **Consumer team**: `team-analytics` (Analytics & Alert Service)
- **Provider service**: IoT Ingestion Service
- **Consumer service**: Analytics & Alert Service

## Contract

- **Contract file**: [contracts/iot-ingestion.openapi.yaml](file:///c:/Users/Admin/Desktop/lab03-NguyenVanToan12/contracts/iot-ingestion.openapi.yaml)
- **Mock base URL**: `http://localhost:4011`
- **Auth method**: Bearer JWT (`bearerAuth`)
- **Endpoint được test**: `POST /readings` (Sự kiện tích hợp thuộc Cặp 06)

## Smoke test

### Request

```http
POST /readings
Authorization: Bearer mock-jwt-token-12345
Content-Type: application/json
```

```json
{
  "device_id": "dev-temp-0042",
  "metric": "temperature",
  "value": 24.5,
  "unit": "celsius",
  "timestamp": "2026-05-29T10:30:00.123Z"
}
```

### Expected response

```json
{
  "reading_id": "R-20260513-0001",
  "device_id": "ESP32-LAB-A01",
  "metric": "temperature",
  "accepted": true,
  "created_at": "2026-05-13T08:30:01+07:00"
}
```

## Kết quả

- [x] Consumer gọi mock thành công.
- [x] Consumer parse được field cần dùng (`reading_id`, `accepted`).
- [x] Consumer hiểu lỗi 4xx/5xx provider trả về (theo cấu trúc `ProblemDetails`).
- [x] Có Newman report hoặc screenshot.

## Ghi chú thay đổi hợp đồng

| Nội dung | Trước | Sau | Người đồng ý |
|---|---|---|---|
| Hướng di chuyển | `IN` / `OUT` | `ENTER` / `EXIT` | Cặp 09 (Gate & Analytics) |
| Định dạng Thẻ | `cardId` (Plain text) | `cardHash` (SHA-256) | Cặp 09 (Gate & Analytics) |
| Ảnh chụp Camera | Base64 nhúng trực tiếp | `snapshotUrl` (S3 URL) | Cặp 07 (Camera & Analytics) |
| Định dạng gửi IoT | Gộp danh sách (Batch) | Tin nhắn đơn lẻ (Single event) | Cặp 06 (IoT & Analytics) |
| Trùng lặp event | Gửi trùng tự do | Hỗ trợ băm deduplication | Cặp 06 (IoT & Analytics) |

## Xác nhận

- **Provider representative**: Đối tác đàm phán (IoT / Camera / Core / Gate)
- **Consumer representative**: Nguyễn Văn Toàn (Đại diện Analytics & Alert Service)
