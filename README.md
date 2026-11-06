# Đọc Màu Tem — ứng dụng demo cho đề tài KHKT

Ứng dụng đọc màu của **tem chỉ thị độ tươi hải sản làm từ hoa đậu biếc**. Hướng camera vào tem, app lấy màu ở vòng tròn giữa khung hình, đổi sang góc màu (hue) rồi báo **Còn tươi / Sắp ươn / Đã ươn**, kèm gợi ý xử lý. App có thêm:

- **Hiệu chuẩn bằng mẫu của nhóm**: thay ngưỡng mặc định bằng màu tem thật của nhóm (quan trọng nhất, xem mục 4).
- **Mốc trắng**: chỉnh lệch màu do ánh đèn.
- **Nhật ký đo + xuất CSV**: ghi loại hải sản, giờ bảo quản, pH, màu RGB, kết quả. Đây là dữ liệu gốc để đưa vào hồ sơ đề tài.

App chạy hoàn toàn trên máy, không gửi ảnh hay dữ liệu đi đâu.

---

## 1. Ba cách chạy app (chọn một)

### Cách A — Nhanh nhất, không cần APK (khoảng 5 phút)

Dùng GitHub Pages để có một đường link https, mở bằng Chrome trên điện thoại là camera chạy được.

1. Tạo tài khoản GitHub, tạo repository mới (ví dụ `doc-mau-tem`), để **Public**.
2. Bấm **Add file → Upload files**, kéo cả thư mục này vào, bấm **Commit changes**.
3. Vào **Settings → Pages**. Mục *Source* chọn **Deploy from a branch**, nhánh `main`, thư mục **/docs**, bấm Save.
4. Chờ 1–2 phút, GitHub hiện link dạng `https://TENCUABAN.github.io/doc-mau-tem/`.
5. Mở link bằng **Chrome trên Android**, cho phép camera. Bấm menu ⋮ → **Thêm vào màn hình chính** để có biểu tượng như một app.

### Cách B — Ra file APK bằng GitHub Actions (không cần cài gì)

Làm tiếp sau bước 2 của cách A (cùng một repository):

1. Vào tab **Actions**. Nếu GitHub hỏi bật workflow, bấm đồng ý.
2. Chọn workflow **Build APK** → **Run workflow**. Đợi khoảng 3–6 phút.
3. Mở lần chạy vừa xong, kéo xuống mục **Artifacts**, tải **DocMauTem-apk** (file zip, bên trong là `app-debug.apk`).
4. Chép file APK sang điện thoại và mở để cài. Android sẽ hỏi cho phép cài từ nguồn này; Google Play Protect có thể cảnh báo vì đây là bản debug tự dựng, chọn **Vẫn cài**.

> Nếu tải lên bằng điện thoại không thấy thư mục `.github`: dùng máy tính để kéo thư mục vào trang Upload, hoặc chọn **Add file → Create new file**, gõ tên `.github/workflows/build-apk.yml` rồi dán nội dung file cùng tên trong thư mục này.

### Cách C — Android Studio (nếu có máy tính)

1. Cài Android Studio, chọn **Open** và trỏ vào thư mục này. Đợi Gradle đồng bộ lần đầu (cần mạng).
2. Bật **Developer options → USB debugging** trên điện thoại, cắm cáp, bấm nút ▶ **Run**.
3. Muốn ra file APK: **Build → Build Bundle(s) / APK(s) → Build APK(s)**. File nằm ở `app/build/outputs/apk/debug/app-debug.apk`.

---

## 2. Cách dùng

1. **Lấy mốc trắng** (tab *Hiệu chuẩn*): hướng camera vào tờ giấy trắng cùng chỗ sáng với tem, bấm *Lấy mốc*.
2. **Đọc màu** (tab *Đọc màu*): đặt tem ở giữa vòng tròn, chỗ sáng đều, không bóng đổ, không chói. Bấm *Đọc màu*.
3. **Lưu**: điền loại hải sản, giờ bảo quản, pH (nếu có đo), bấm *Lưu vào nhật ký*.
4. **Xuất dữ liệu** (tab *Nhật ký*): bấm *Sao chép CSV*, dán vào Excel hoặc Google Sheets.

Chưa có tem thì bấm một trong ba nút **màu mẫu** ở thẻ “Chưa có kết quả” để xem app trả kết quả ra sao. Nếu camera không mở được, nút **Chụp ảnh** cho đọc màu từ một ảnh.

---

## 3. App tính kết quả như thế nào

- Lấy một ô vuông nhỏ ở giữa khung hình (12% cạnh ngắn), thu về 24×24 điểm ảnh, **bỏ 10% điểm sáng nhất và tối nhất** (vệt chói, bóng), lấy trung bình.
- Nếu có mốc trắng, chia từng kênh R, G, B cho mốc trắng.
- **Cách mặc định (chưa hiệu chuẩn)**: đổi sang góc màu (hue). Chỉ số màu = (260 − hue) / 160 × 100.
  - Dưới 25: **Tươi** (tím, xanh chàm)
  - Từ 25 đến dưới 65: **Sắp ươn** (xanh dương, xanh ngọc)
  - Từ 65 trở lên: **Ươn** (xanh lục, vàng olive)
  - Ngoài khoảng đọc nếu màu hồng/đỏ (thường do tem dính chanh, giấm), quá tối, hoặc quá nhạt.
- **Cách hiệu chuẩn** (đủ ít nhất 1 mẫu cho cả 3 mức): so màu với trung bình mẫu của từng mức trong không gian CIE Lab, chọn mức gần nhất. Màu cách xa mọi mẫu (ΔE > 40) thì báo “Màu lạ”.

---

## 4. Những điều cần nói thật trong hồ sơ

- **Ngưỡng mặc định là ước lượng** từ màu hoa đậu biếc trong các tài liệu, **chưa phải số đo của nhóm**. Muốn kết quả đáng tin, nhóm phải hiệu chuẩn bằng tem thật và đối chiếu với **pH thịt và cảm quan** (mùi, độ đàn hồi, nhớt). Mỗi mức nên có từ 5 mẫu trở lên, đo ở nhiều lần làm tem khác nhau.
- Màu hiển thị phụ thuộc ánh đèn, camera từng máy, độ ẩm tem. Hãy báo cáo độ lệch giữa các lần đo thay vì chỉ báo “đúng”.
- App **hỗ trợ tham khảo, không thay thế kiểm nghiệm an toàn thực phẩm**.
- Phần mềm này được viết với sự hỗ trợ của AI (Claude). Quy định mới về nghiên cứu KHKT yêu cầu khai báo việc dùng công cụ AI, ghi nhật ký nghiên cứu và giữ dữ liệu gốc, nên hãy ghi rõ phần nào nhóm tự thiết kế, thí nghiệm, kiểm tra. Hỏi giáo viên hướng dẫn để chắc về mẫu khai báo của trường.

---

## 5. Cấu trúc thư mục

```
docs/index.html                       Bản web (dùng cho GitHub Pages)
app/src/main/assets/index.html        Bản đóng vào APK (không cần mạng)
app/src/main/java/.../MainActivity.java   Vỏ Android: WebView, xin quyền camera
.github/workflows/build-apk.yml       Tự dựng APK trên GitHub
```

Muốn sửa app: sửa `docs/index.html`, rồi chép sang `app/src/main/assets/index.html` (bản APK chỉ khác ở chỗ bỏ 3 dòng nạp phông Google Fonts ở đầu file).

## 6. Sự cố thường gặp

| Hiện tượng | Cách xử lý |
|---|---|
| Camera không mở trên link Pages | Phải mở bằng **https** (link github.io), không mở file HTML trực tiếp. Vào cài đặt trang web của Chrome, bật quyền Camera. |
| Bấm “Chụp ảnh” chỉ ra thư viện ảnh | Chọn chụp mới trong trình chọn của máy hoặc dùng camera trực tiếp. |
| Màu lúc xanh lúc tím dù cùng một tem | Do ánh sáng. Lấy lại mốc trắng, đọc ở cùng một chỗ, tắt đèn flash, tránh bóng tay. |
| Actions báo lỗi đỏ | Mở bước **Build debug APK** xem dòng lỗi đầu tiên, gửi lại cho người hỗ trợ. |
| Mất hiệu chuẩn | Dữ liệu nằm trong bộ nhớ của app/trình duyệt. Gỡ app hoặc xóa dữ liệu trang sẽ mất, nên xuất CSV thường xuyên. |
