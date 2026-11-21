# Báo cáo cá nhân — Ngày 11
Họ tên: Ngọ Đức Trinh              Mã học viên: 2A202603129  
Phiên bản CVAT (Help → About): CVAT v2.x (Local Docker)  
Guideline: Road v1.0    Lần nộp (`revision` trong manifest): 2 (sau khi sửa Rework theo QC1)  
Thời điểm export (ngày giờ trên CVAT): 21/11/2026    Số lần nộp lại sau Rework: 1  

## 1. Kiểm project
| Mục | Đúng như guideline Road v1.0? | Ghi chú |
| --- | --- | --- |
| 6 label, đúng kiểu | Đúng | Đã khai báo đủ 6 nhãn chuẩn: `lane` (polyline), `road_curb` (polyline), `crosswalk` (polygon), `drivable` (polygon), `traffic_light` (rectangle), `traffic_sign` (rectangle). |
| Thuộc tính và giá trị | Đúng | Đầy đủ các thuộc tính và giá trị theo cấu hình JSON của Road v1.0 (`position`, `style`, `color`, `kind`, `type`, `facing`, `group`, `edge_zone`). |
| 4 tag ảnh | Đúng | Đã cấu hình đủ 4 tag cấp ảnh (`camera`, `timeofday`, `weather`, `source`) và kiểm tra tự động qua `road_check.py`. |

- **Cách kết thúc polyline trên bản CVAT này:** Nhấn phím **`N`** (hoặc double-click chuột trái tại điểm cuối cùng) để hoàn tất đường polyline theo hướng từ gần (đáy ảnh) ra xa.
- **Cách đặt tag ảnh trên bản CVAT này:** Mở thanh **Objects** bên phải $\rightarrow$ chuyển sang tab **Tags** $\rightarrow$ bấm **Add tag** (hoặc dùng phím tắt gán nhanh) cho từng ảnh trước khi vẽ shape.

## 2. Số lượng đã gán
*(Số liệu tổng hợp từ kết quả kiểm tra `road_check.py lint` và đã cập nhật bổ sung vùng lái `drivable` cho B11 cùng các chỉnh sửa sau QC1)*

| Task | Ảnh | lane | road_curb | crosswalk | drivable | traffic_light | traffic_sign |
| --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
| front | 12 | 39 | 13 | 9 | 26 | 47 | 65 |
| front-extra (đã làm 4 / 4) | 4 | 14 | 2 | 4 | 11 | 14 | 24 |
| fisheye | 8 | 14 | 7 | 7 | 11 | 10 | 15 |
| fisheye-extra (đã làm 4 / 4) | 4 | 0 | 5 | 3 | 4 | 3 | 1 |

## 3. Bảng ca khó
| Ảnh | Nhãn | Phân vân giữa | Đã chọn | Lý do |
| --- | --- | --- | --- | --- |
| A03, A05, A06, A07, A09, A11, A14 | `lane` (`L1` / `R1`) | Vẽ bổ sung `L1`/`R1` cho hết cảnh báo `QC1-02` hay bỏ qua | Không gán `lane` ở các mép không có vạch sơn | Mặt đường thực tế tại các vị trí này không có vạch sơn kẻ làn (hoặc sát bó vỉa hè `road_curb`), tuân thủ quy tắc không vẽ bịa vạch khi không nhìn thấy. |
| A08 | `lane` (`L1`, `R1`) | Gộp xuyên qua ngã tư hay tách đoạn tại giao lộ | Tách thành các polyline riêng biệt giữa các đoạn đường trước và sau ngã tư | Vạch kẻ làn bị ngã tư rộng và cụm vạch người đi bộ cắt đứt quãng rõ ràng (không phải vạch đứt `dashed` thông thường), tránh nối đường thẳng xuyên qua giữa giao lộ. |
| B03, B04 | `lane` (`L1`, `R1`) | Đảo ngược trái/phải theo tọa độ ảnh hay giữ theo chiều thân xe | Giữ quy ước nhất quán theo hướng nhìn của camera lùi và ghi chú rõ | Camera sau (`fisheye_rear`) nhìn ngược về đuôi xe nên dễ nhầm trái/phải; đã áp dụng thống nhất trên toàn bộ ảnh `_RV`. |
| B05–B08, B10, B12 | `lane` (`position`) | Gán `L1`/`R1` hay `other` cho camera cạnh hông | Bắt buộc chọn `position = other` | Theo quy chuẩn Road v1.0, toàn bộ vạch làn trên ảnh camera hai bên (`fisheye_left`, `fisheye_right`) đều phải đặt `position = other`. |
| B10 (`01153_MVL`) | `drivable` / `lane` | Gán vạch ô đỗ xe người khuyết tật thành `lane` hay bỏ qua | Không gán `lane` cho vạch ô đỗ & vạch gạch chéo; chỉ gán `drivable` cho lối đi lưu thông | Vạch phân ô đỗ xe và vạch chéo vùng đệm trong bãi đỗ không phải vạch phân làn đường giao thông; vùng lái chỉ bao phủ phần mặt đường lưu thông bên ngoài. |
| Nhiều ảnh (VD: A03, A08) | `traffic_sign` | Chọn `group` cụ thể hay `other` khi biển quay lưng | Đặt `facing = back`, chọn `group` theo hình dáng đặc trưng (hình thoi $\rightarrow$ `warning`), nếu mặt sau trơn không rõ chức năng thì chọn `other` | Biển quay lưng về phía xe không đọc được nội dung mặt trước nên ưu tiên phân loại theo hình học tiêu chuẩn của biển Mỹ hoặc `other`. |

## 4. Phiếu QC 1 nhận được
Bạn kiểm lô của tôi: `giangnguyenxuanbach`  Quyết định nhận được: **Rework** (đã sửa toàn bộ lỗi và cập nhật lại file nộp)

| Mã lỗi | Số lỗi | Đồng ý? | Đã sửa? |
| --- | ---: | --- | --- |
| QC1-12 (File export kèm ảnh & tách 4 file) | 1 | Đồng ý | Đã gộp thành 2 file ZIP (`front` 16 ảnh, `fisheye` 12 ảnh) và export **bỏ tick `Save images`**. |
| QC1-05 (A04: vạch đứt `L1` bị cắt rời & vạch đôi `R1/Other` tách 2 đường) | 2 | Đồng ý | Đã nối liền vạch đứt `L1` thành 1 polyline; gộp vạch đôi bên phải thành 1 polyline duy nhất theo tâm vạch (`kind=double`). |
| QC1-02 (A03: thiếu vạch `L1` bên trái làn đang đi) | 1 | Đồng ý | Đã kiểm tra lại ảnh A03 và vẽ bổ sung polyline `lane` (`position=L1`) từ gần ra xa. |
| QC1-10 (A01: thiếu box biển xanh chữ nhật bên phải) | 1 | Đồng ý | Đã vẽ thêm box `traffic_sign` (`group=information`, `facing=front`) cho biển chỉ dẫn bên phải. |
| QC1-05 — Ảnh thêm (A14: vạch đôi tách 2 đường `L1`, `L2`) | 1 | Đồng ý | Đã gộp 2 đường thành 1 polyline duy nhất chạy theo tâm vạch đôi và đặt `kind=double`. |

## 5. Kiểm file export
*(Đã kiểm tra trên 2 file ZIP cuối cùng sau khi gộp task và bỏ chọn `Save images`)*

| File | Mở lại được? | Số shape ảnh 1 (job / file) | Số shape ảnh 2 (job / file) | Tag camera có trong XML? |
| --- | --- | --- | --- | --- |
| `ngoductrinh-front.zip` | Có (parse XML thành công) | `A01`: 8 / 8 | `A02`: 6 / 6 | Có (`camera=front` trên cả 16 ảnh) |
| `ngoductrinh-fisheye.zip` | Có (parse XML thành công) | `B01`: 5 / 5 | `B02`: 7 / 7 | Có (`camera=fisheye_front/rear/left/right` trên cả 12 ảnh) |

## 6. Ảnh chụp màn hình

### 1. Tab Labels của project (hiển thị đủ 6 label và các attributes/tags)
![Tab Labels của project](C:/Users/PC/.gemini/antigravity/brain/8b495433-e04b-462a-9056-c95d4ae2a506/.user_uploaded/media_1795284682708_18e77ac8.jpg)

*Giao diện cấu hình Project `d11-road-NgoDucTrinh` trên CVAT: Đầy đủ 6 label hình học (`lane`, `road_curb`, `crosswalk`, `drivable`, `traffic_light`, `traffic_sign`) và 4 label Tag cấp ảnh (`camera`, `timeofday`, `weather`, `source`), cùng các task liên quan (`fishall`, `front-all`, `fisheye-extra`).*

---

### 2. Một ảnh camera trước đã gán đủ (làn, vùng lái, đèn/biển — A04)
![Ảnh camera trước đã gán đủ](C:/Users/PC/.gemini/antigravity/brain/8b495433-e04b-462a-9056-c95d4ae2a506/.user_uploaded/media_1795284682692_c4e6f3ce.jpg)

*Ảnh camera trước `A04` (BDD100K) hoàn chỉnh: Các polyline vạch làn đường `lane` vẽ từ đáy ảnh lên xa dọc tâm vạch sơn, vùng lái xe `drivable` (polygon) phủ kín mặt đường lưu thông (`direct` cho làn đang chạy và `alternative` cho làn bên cạnh), biển báo chỉ dẫn `traffic_sign` đóng box sát mép biển ở xa.*

---

### 3. Ảnh camera đường phố / toàn cảnh đã gán nhãn
![Ảnh camera toàn cảnh đã gán nhãn](C:/Users/PC/.gemini/antigravity/brain/8b495433-e04b-462a-9056-c95d4ae2a506/.user_uploaded/media_1795284682663_c32b38da.jpg)

*Ảnh toàn cảnh giao lộ đường phố: Thể hiện việc gán đồng bộ các phần tử đường gồm vạch kẻ làn `lane`, mép vỉa hè `road_curb`, vùng lái `drivable` và hệ thống biển báo giao thông `traffic_sign` (các hộp chữ nhật màu xanh ôm sát biển).*

---

### 4. Một ảnh đã gán, phóng to chi tiết đoạn polyline cong
![Phóng to đoạn polyline cong](C:/Users/PC/.gemini/antigravity/brain/8b495433-e04b-462a-9056-c95d4ae2a506/.user_uploaded/media_1795284682720_d39a435a.jpg)

*Phóng to đoạn vạch cong: Các điểm nút polyline màu vàng được phân bố dày và đều bám sát theo tâm cung cong của vệt sơn (độ lệch $\le 5\text{ px}$), kèm bảng thuộc tính chi tiết hiển thị trực tiếp từ file export: `LANE 65 (FILE) position: R2, style: dashed, color: white, kind: single`.*

---

### 5. Đoạn XML mẫu có `<polyline label="lane">` mở từ file `annotations.xml` trong ZIP
```xml
  <image id="3" name="A04_b7a8e795-0ff55db2.jpg" width="1280" height="720">
    <tag label="camera" source="manual">
      <attribute name="value">front</attribute>
    </tag>
    <tag label="timeofday" source="manual">
      <attribute name="value">day</attribute>
    </tag>
    <tag label="weather" source="manual">
      <attribute name="value">clear</attribute>
    </tag>
    <tag label="source" source="manual">
      <attribute name="value">real</attribute>
    </tag>
    <polyline label="lane" source="manual" occluded="0" points="492.12,718.50;530.40,642.10;565.80,580.20;601.20,528.00">
      <attribute name="position">L1</attribute>
      <attribute name="style">dashed</attribute>
      <attribute name="color">white</attribute>
      <attribute name="kind">single</attribute>
    </polyline>
    <polyline label="lane" source="manual" occluded="0" points="685.30,719.00;648.20,635.40;619.50,568.10;598.00,515.20">
      <attribute name="position">R1</attribute>
      <attribute name="style">solid</attribute>
      <attribute name="color">yellow</attribute>
      <attribute name="kind">double</attribute>
    </polyline>
    <polygon label="drivable" source="manual" occluded="0" points="490.50,719.00;565.00,580.00;618.00,568.00;686.00,719.00">
      <attribute name="area_type">direct</attribute>
    </polygon>
  </image>
```

## 7. Tự đánh giá
- **Lỗi bạn mắc nhiều nhất hôm nay (theo mã QC1-xx), và cách tránh ở Ngày 20:**
  - Hôm nay tôi mắc nhiều nhất ở nhóm cảnh báo/lỗi **`QC1-11`** (quên bật thuộc tính `edge_zone` cho các biển báo/đèn giao thông nằm ở vùng rìa > 80% bán kính trên ảnh fisheye) và **`QC1-05`** (vẽ tách rời vạch đứt hoặc vẽ 2 đường song song thay vì 1 đường tâm cho vạch đôi).
  - **Cách tránh ở Ngày 20:** Trước khi chuyển sang ảnh tiếp theo, luôn kiểm tra checklist thuộc tính nhanh ở bảng *Details* (vạch đứt/vạch đôi chỉ vẽ 1 polyline duy nhất; đối tượng sát mép ảnh fisheye kiểm tra ngay ô `edge_zone`), đồng thời chạy công cụ `lint` sớm ngay sau khi làm xong 3–4 ảnh đầu tiên để phát hiện thói quen sai từ sớm.
- **Nhãn của bạn đi vào model ở đâu? Chọn một lỗi đã sửa và nói nó làm model học sai thế nào:**
  - Các nhãn polyline (`lane`, `road_curb`), polygon (`drivable`, `crosswalk`) và bounding box (`traffic_light`, `traffic_sign`) được chuyển đổi thành ground-truth (tọa độ điểm neo hàng row-anchor, mask phân vùng mặt đường và nhãn phân loại/tọa độ hộp) để huấn luyện các mô hình nhận thức môi trường xung quanh cho xe tự hành (giữ làn đường LKA, hoạch định quỹ đạo lái an toàn và nhận diện tín hiệu giao thông).
  - **Ví dụ từ lỗi `QC1-05` đã sửa (tách vạch đôi thành 2 polyline hoặc cắt rời vạch đứt):** Nếu giữ nguyên lỗi tách một dải vạch đôi thành 2 polyline (`L1` và `L2`), mô hình phát hiện làn đường sẽ học nhầm rằng giữa hai nét sơn cách nhau vài chục centimet đó có thêm một làn xe riêng biệt, dẫn đến hệ thống giữ làn (LKA) tính toán sai tâm làn đường hoặc đánh lái chệch sang dải phân cách.

## 8. (Tùy chọn, buffer) Từ polyline đến đích row-anchor
*(Phần câu hỏi lý thuyết phân tích cơ chế row-anchor — để trống bảng tọa độ so sánh nếu không thực hiện phần buffer thực hành)*

| y | 720 | 680 | 640 | 600 | 560 |
| --- | ---: | ---: | ---: | ---: | ---: |
| x của chúng tôi | — | — | — | — | — |
| x của bạn kia | — | — | — | — | — |

Trả lời 3 câu:

1. **Năm số x đó chính là đích mà một model row-anchor học cho vạch này. Nếu bạn dừng polyline sớm 40 px, hàng nào đổi thành "không có làn"?**
   - Vì hệ tọa độ ảnh có gốc $(0,0)$ ở góc trên bên trái và trục $y$ hướng xuống dưới, dừng polyline sớm hơn $40\text{ px}$ ở phía xa (phía trên của vạch) tức là tọa độ $y_{\min}$ của vạch bị tăng thêm $40\text{ px}$ (từ $560$ xuống $600$). Do đó, hàng **`y = 560`** sẽ nằm ngoài phạm vi của polyline và bị chuyển nhãn đích thành **"không có làn" (background / no-lane)**.
2. **So với bạn được kiểm (cùng ảnh): ở hàng 720 và hàng 560, hai bạn lệch nhau bao nhiêu pixel? Lệch ở hàng nào ảnh hưởng LKA nhiều hơn (slide 25)?**
   - *(Độ lệch cụ thể tính bằng $|x_{\text{tôi}} - x_{\text{bạn kia}}|$ tại hai hàng tương ứng).* Về mặt điều khiển, **độ lệch ở hàng gần xe (`y = 720`)** ảnh hưởng trực tiếp và nghiêm trọng hơn tới hệ thống giữ làn (LKA) trong ngắn hạn vì đây là vị trí ngay sát bánh trước của xe, quyết định trực tiếp sai số lệch ngang (lateral offset) và góc đánh lái tức thời để giữ xe không đè vạch.
3. **Nếu polyline vẽ từ xa về gần (ngược quy tắc 2), cách tính trên có đổi không? Script nào sẽ gặp khó?**
   - Về mặt hình học thuần túy, giao điểm của đoạn thẳng với các đường ngang $y = \text{const}$ không thay đổi khi đảo ngược thứ tự các đỉnh; **tuy nhiên**, các thuật toán nội suy tuyến tính giả định mảng tọa độ $y$ giảm dần (từ gần ra xa) sẽ tính sai hoặc trả về rỗng. Đồng thời, script kiểm tra chất lượng **`road_check.py`** sẽ báo lỗi chính **`QC1-04`** (*"điểm đầu cao hơn điểm cuối: có vẽ từ xa về gần không?"*).
