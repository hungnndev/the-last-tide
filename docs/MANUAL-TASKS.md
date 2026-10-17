# Việc bạn làm bằng tay

Làm theo thứ tự. Mục nào có ⛔ là **mình cần kết quả của bạn mới làm tiếp được**, còn các mục khác bạn làm lúc nào rảnh cũng được. Làm xong mục nào thì đánh dấu `[x]` và báo mình.

Spec tham chiếu: [docs/superpowers/specs/2026-10-14-the-last-tide-survivors-design.md](superpowers/specs/2026-10-14-the-last-tide-survivors-design.md)

---

## 0. Duyệt spec ⛔ (khoảng 15 phút)

- [ ] Đọc spec ở trên và báo mình chỗ nào muốn sửa. Bạn duyệt xong thì mình mới lên kế hoạch chi tiết và bắt đầu code M1.

---

## 1. Tạo art trên Kaggle

Code không phải chờ art. M1 và M2 chạy bằng hình tạm, nên phần này bạn làm song song lúc rảnh. Art chỉ cần có trước M3 (khoảng tuần thứ 5).

Mọi input đã có sẵn trong [art/kaggle/](../art/kaggle/):

| File | Nội dung |
|---|---|
| `style.json` | 3 style mẫu A/B/C và negative prompt |
| `prompts.json` | 54 asset, tổng cộng 234 ảnh |
| `generate_assets.ipynb` | Notebook để chạy |

### 1.1 Chuẩn bị tài khoản (khoảng 10 phút, chỉ làm một lần)

- [ ] Tạo tài khoản tại <https://www.kaggle.com>.
- [ ] **Xác minh số điện thoại** trong *Settings → Phone verification*. Không xác minh thì không bật được GPU và Internet.

### 1.2 Upload input

- [ ] Vào *Datasets → New Dataset*, đặt tên **`last-tide-art`**, upload `style.json` và `prompts.json`. Để chế độ **Private**.
- [ ] Vào *Code → New Notebook → File → Import Notebook*, chọn `generate_assets.ipynb`.
- [ ] Trong notebook, mở *Settings* bên phải:
  - **Accelerator** = GPU T4 x2 (hoặc GPU P100).
  - **Internet** = On.
- [ ] Bấm *Add Input* và chọn dataset `last-tide-art`.

### 1.3 Chọn style ⛔ (khoảng 30 phút, chủ yếu là chờ máy chạy)

- [ ] Ở ô **Cấu hình**, để `MODE = "explore"`, rồi bấm **Run All**. Sẽ tạo 48 ảnh, mất khoảng 20–25 phút.
  - Lần đầu chạy sẽ tải model khoảng 7GB. Việc này diễn ra trên máy của Kaggle, không tốn ổ cứng của bạn.
- [ ] Ở tab **Output**, tải file `last_tide_art.zip` về. Mở thư mục `_sheets/` bên trong: mỗi file là một tấm ảnh ghép của một style.
- [ ] Báo mình hai thứ:
  1. Chọn **style A, B hay C**, hoặc "A nhưng tối màu hơn"... Mình sẽ chỉnh `style.json` theo.
  2. **3–5 ảnh đẹp nhất** (tên file, ví dụ `explore/A/shark_2.png`). Các ảnh này sẽ làm ảnh mẫu để mọi asset sau ra cùng một style.

### 1.4 Tạo toàn bộ asset (khoảng 2 giờ chạy máy, bạn không cần ngồi canh)

Làm sau khi mình đã cập nhật `style.json`.

- [ ] Vào dataset `last-tide-art` → *New Version*:
  - Upload lại `style.json` mới.
  - Tạo thư mục `refs/` chứa 3–5 ảnh mẫu đã chọn. Nếu có ảnh chân dung thì để riêng vào `refs/portrait/`.
- [ ] Trong notebook, đổi `MODE = "assets"`, rồi bấm **Run All**. 234 ảnh mất khoảng 1,5–2 giờ.
  - Nếu bị ngắt giữa chừng, chỉ cần chạy lại: notebook **tự bỏ qua các ảnh đã tạo**.
  - Nếu muốn chia ra nhiều lần chạy: dùng `ONLY_CATEGORIES = ["enemies", "bosses"]`.
  - Kaggle cho khoảng 30 giờ GPU miễn phí mỗi tuần, thừa sức cho việc này.
- [ ] Tải `last_tide_art.zip`, giải nén vào **`art/raw/`** trong dự án, rồi báo mình.
  - Toàn bộ ảnh chiếm khoảng 300–400MB ổ cứng.
  - Mình sẽ làm một trang HTML nhỏ để bạn bấm chọn ảnh tốt nhất cho mỗi asset. Sau đó script sẽ tự tách nền, resize và đóng gói.

### 1.5 Tạo lại ảnh chưa ưng (làm bất cứ lúc nào)

- [ ] Ghi id của các ảnh chưa ưng vào `ONLY_IDS` (ví dụ `["the_maw", "portrait_tavi"]`). Nếu muốn đổi mô tả thì sửa `subject` trong `prompts.json` rồi upload version mới cho dataset. Sau đó bấm Run All.

---

## 2. Kiểm tra tên game (khoảng 10 phút, làm trước khi phát hành)

- [ ] Tìm "The Last Tide" trên CrazyGames, Poki, itch.io, Steam, Google Play và App Store. Nếu đã có game trùng tên và cùng thể loại thì báo mình để nghĩ tên khác hoặc thêm phụ đề (ví dụ *The Last Tide: Raft Survivors*).

---

## 3. Tài khoản phát hành (cần trước M3)

- [ ] Tạo tài khoản developer tại <https://developer.crazygames.com>. Có thể dùng chung tài khoản với game billiard.
- [ ] (Không bắt buộc) Tạo trang game mới trên itch.io, để ở chế độ **Draft**.
- [ ] (Không bắt buộc) Tạo project PostHog mới cho The Last Tide, hoặc dùng chung với billiard. Gửi mình **Project API key** (dạng `phc_...`, đây là key công khai).

---

## 4. Âm thanh (cần trước M3)

Đến M3, mình sẽ gửi danh sách hiệu ứng âm thanh và nhạc nền cần có, kèm từ khóa tìm kiếm cho từng file.

- [ ] Tạo tài khoản tại <https://freesound.org>. Cần đăng nhập mới tải được file.

---

## 5. Chơi thử (sau mỗi mốc)

Mình sẽ bổ sung checklist chi tiết khi tới từng mốc. Mỗi lần chơi thử, bạn ghi lại:

- [ ] **Sau M1:** chơi chương 1 khoảng 3–5 lượt, trên cả máy tính và điện thoại. Ghi lại: điều khiển có thuận tay không, chết ở phút thứ mấy, có thấy chán ở đoạn nào không.
- [ ] **Sau M2:** chơi đủ 3 chương. Ghi lại: độ khó, vũ khí nào quá mạnh hoặc quá yếu, giá nâng cấp vĩnh viễn có hợp lý không.
- [ ] **Sau M3:** test trên link Cloudflare/itch với điện thoại thật. Ghi lại: thời gian tải, quảng cáo hiện đúng chỗ chưa, save có còn sau khi tải lại trang không.
