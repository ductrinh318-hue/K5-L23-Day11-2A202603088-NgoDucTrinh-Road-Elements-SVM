# Phiếu QC 1 — Road v1.0

- Lô được kiểm (học viên): Giang Nguyễn Xuân Bách (`giangnguyenxuanbach`).
- Người kiểm (học viên): Ngọ Đức Trinh (2A202603088).
- Ngày: 21/11/2026. Phiên bản guideline: Road v1.0.
- File được kiểm: `giangnguyenxuanbach-front.zip`, `giangnguyenxuanbach-fisheye.zip`; manifest `giangnguyenxuanbach-manifest.json`, `revision` **1**.
- Số ảnh kiểm trực tiếp: 6 ảnh bắt buộc (A01, A05, A06, A07, A09, B07) + 2 ảnh thêm (A13, A14). Đã parse và rà tag/camera/source trên cả 28 ảnh.
- Số nhãn đã kiểm trực tiếp (shape + tag): **58** ở 6 ảnh bắt buộc (34 shape + 24 tag); **23** ở 2 ảnh thêm (15 shape + 8 tag), ghi riêng, không đưa vào mẫu số quyết định.

## Tổng hợp theo mã — 6 ảnh bắt buộc

Số mục kiểm ở các mã có thể trùng nhau vì một nhãn được kiểm theo nhiều quy tắc. Các cảnh báo A05/A06/A07 không được tính là lỗi QC1-02 khi ảnh gốc không cho thấy vạch cần gán một cách rõ ràng; việc miễn trừ chính thức cần coach xác nhận.

| Mã | Loại lỗi | Số mục kiểm | Số lỗi xác nhận | Mức |
| --- | --- | ---: | ---: | --- |
| QC1-01 tag camera | sai thuộc tính / bỏ sót | 28 ảnh (kiểm XML) | 0 | nặng |
| QC1-02 đủ L1, R1 | bỏ sót | 5 ảnh front | 0; 3 ca chờ coach | nặng |
| QC1-03 position | sai thuộc tính / thừa | 11 lane | 1 | nặng |
| QC1-04 tâm vạch, chiều, điểm dừng | sai phạm vi / bỏ sót | 9 lane front | 1 | chính |
| QC1-05 vạch đứt, che | sai phạm vi / sai thuộc tính | 11 lane | 0 | chính |
| QC1-06 style, color, kind | sai thuộc tính | 11 lane | 0 | chính |
| QC1-07 vùng lái | sai phạm vi / bỏ sót / thừa | 15 drivable/crosswalk/road_curb | 2 | chính |
| QC1-08 đèn, màu | bỏ sót / sai phạm vi / sai thuộc tính | 6 traffic_light | 0 | nặng / chính |
| QC1-09 facing, type, group, biển phụ | sai thuộc tính | 8 box đèn/biển | 0 | chính |
| QC1-10 hình in, đủ biển | bỏ sót / sai phạm vi | 2 traffic_sign đã gán + ảnh A06 | 2 | chính |
| QC1-11 fisheye | sai phạm vi / sai thuộc tính | 5 shape ở B07 | 0 | chính |
| QC1-12 file | lỗi file | 2 ZIP | 0 | nhẹ / nặng |

## Danh sách lỗi xác nhận — 6 ảnh bắt buộc

CVAT for images 1.1 không cung cấp `id` ổn định cho các shape trong XML; tọa độ điểm đầu hoặc tâm box được dùng để nhận diện nhãn.

| # | Mã | Ảnh | Nhãn (label, tọa độ x/y) | Mức | Mô tả và hướng sửa |
| ---: | --- | --- | --- | --- | --- |
| 1 | QC1-03 | A09 | `lane`, điểm đầu (523, 666), `position=R1` | nặng | Vạch đứt này ở **bên trái vùng direct**, là ranh giới L1 của làn đang đi. Đổi `position` thành L1; không cộng thêm lỗi QC1-02 cho cùng vạch. |
| 2 | QC1-04 | A09 | `lane`, điểm đầu (469, 662), `position=L2` | chính | Polyline của vạch đôi đi từ gần lên xa rồi quay về gần (điểm cuối y≈661), thành hình chữ U. Vẽ lại một polyline theo tâm vạch đôi, từ gần ra xa. |
| 3 | QC1-07 | A06 | `drivable direct`, điểm đầu (612, 437) | chính | Polygon chỉ phủ đoạn xa, dừng ở khoảng y≈420–440, bỏ trống phần đường lái được phía trước xe tới gần đáy ảnh. Mở rộng vùng direct theo mặt đường thực. |
| 4 | QC1-07 | A09 | `drivable direct`, điểm đầu (530, 664) | chính | Polygon tràn xuống phần capô/thân xe mình ở đáy ảnh. Dừng vùng direct tại mép mặt đường. |
| 5 | QC1-10 | A06 | `traffic_sign` thiếu, tâm khoảng (576, 318) | chính | Biển cảnh báo màu cam bên phải đường, cao hơn 8 px, chưa có box. Gán box riêng sát viền và thuộc tính phù hợp. |
| 6 | QC1-10 | A06 | `traffic_sign` thiếu, tâm khoảng (878, 354) | chính | Biển cảnh báo hình thoi màu vàng bên phải, cao hơn 8 px, chưa có box. Gán box riêng sát viền và thuộc tính phù hợp. |

## Ảnh thêm — ghi riêng, không tính vào 58 nhãn bắt buộc

| Mã | Ảnh | Nhãn | Mức | Nhận xét |
| --- | --- | --- | --- | --- |
| QC1-02 | A13 | `lane L1` chưa gán | nặng | Vạch vàng bên trái làn direct nhìn thấy rõ trên ảnh; ảnh thêm này chưa đạt điều kiện thưởng. |
| QC1-06 | A13 | `lane R1`, điểm đầu (768, 558) | chính | Vạch trắng phân làn nhìn thấy là vạch đứt nhưng `style=solid`. |
| QC1-06 | A14 | hai `lane L1`, điểm đầu (297, 607) và (326, 610) | chính | Hai nét của cùng vạch vàng đôi được gán thành hai polyline, mỗi polyline lại có `kind=double`. Cần một polyline ở tâm vạch với `kind=double`. |

## Ca mơ hồ và ảnh không lỗi trong mẫu

| Ảnh | Câu hỏi cho coach / kết quả quan sát |
| --- | --- |
| A05 | Không thấy L1/R1 sơn rõ trên mặt đường; đề nghị coach xác nhận miễn QC1-02. |
| A06 | Không thấy R1 rõ trong vùng lái; đề nghị coach xác nhận miễn QC1-02. |
| A07 | Bên trái làn direct là dải phân cách, không thấy L1 sơn rõ; đề nghị coach xác nhận miễn QC1-02. |
| A01 | Đã kiểm ảnh và các nhãn trong mẫu; không phát hiện lỗi theo các mã đã kiểm. |
| B07 | Đã kiểm ảnh fisheye và 5 shape trong mẫu; không phát hiện lỗi theo các mã đã kiểm. |

## Quyết định

Lỗi nặng xác nhận ở ảnh bắt buộc: **1**. Lỗi chính xác nhận: **5 / 58 nhãn = 8,62%**. Lỗi nhẹ: **0**. Ca chờ coach: **3** (A05, A06, A07).

Quyết định (Release / Hold / Rework): **Rework**.
Lý do: QC1-03 ở A09 là lỗi nặng; đồng thời 8,62% lỗi chính vượt ngưỡng 5%. Sửa các lỗi đã ghi rồi xuất lại và tăng `revision` trong manifest. Các ca QC1-02 chờ coach xác nhận nhưng không làm thay đổi quyết định Rework hiện tại.

## Dấu vết file được kiểm

- Front ZIP SHA-256: `e9cc5c87e082243e2d333fb86080cefe8c281cc337988a27617cab6f862a3ef5`.
- Fisheye ZIP SHA-256: `ce0919cc578ee25fd2d801a52c36ec6fd43c4f3d0f4a41673f01ab032da7b305`.
- Manifest `road-delivery/1` khai báo đúng định dạng CVAT for images 1.1, Road v1.0, `revision=1`; tên file, kích thước, SHA-256, số ảnh và số shape theo nhãn đều khớp hai ZIP đã kiểm. Cả hai mục đều có `contains_images=false`.
- Cả hai ZIP chỉ chứa `annotations.xml`, parse được; 16 ảnh front và 12 ảnh fisheye đều có đủ bốn tag và camera đúng hậu tố. Không có ảnh trong ZIP. Manifest ghi 6 cảnh báo nặng, 2 cảnh báo chính từ lint; đây là cảnh báo tự động, khác với số lỗi đã xác nhận bằng ảnh trong phiếu QC1.
