# Funny app – bộ data v2 (24/10/2026)

Bộ data mới cho app **Cười Xả Stress - Chém Gió** (`apps/funny`). Giữ **đúng format API cũ** mà
`useFunnyStore.ts` đang đọc, nên **không cần sửa code app** – chỉ cần đẩy lên repo data.

## Triển khai
1. Clone repo data: `git clone https://github.com/alexiosdev/Hai.git`
2. Chép đè các thư mục của bộ này vào gốc repo: `stories/`, `chemgio/`, `channel/`, `img/`
   (file cũ không còn được tham chiếu có thể giữ hoặc xoá sau).
3. Commit + push lên nhánh `master`. App cache 24h nên người dùng thấy data mới trong vòng 1 ngày.

## Có gì trong bộ này
| Mảng | Cũ | Mới |
|---|---|---|
| Truyện cười | 15 danh mục (2 lỗi), 1.844 truyện | 12 danh mục, 1.254 truyện (1.184 truyện cũ đã duyệt + 70 truyện mới) |
| Chém gió | 78 danh mục, 7.782 câu | 22 danh mục, 6.933 câu (đã duyệt + 240 câu mới theo trend 2026) |
| Video | 13 kênh, nhiều kênh con hỏng, video 2016–2020 | 21 kênh phẳng: 13 kênh còn hoạt động (lấy video mới nhất của từng kênh) + 8 nhóm "kinh điển"; 1.128 video, đều qua kiểm tra nhúng |

- Ảnh danh mục: bộ ảnh mới đồng bộ (gradient + icon Lucide), không dùng ảnh nhân vật có bản quyền.
- Truyện có thêm trường `hot` (điểm 5) và `isNew`; danh mục có `count`, `description`; kênh có `group`
  (`Mới cập nhật` / `Kinh điển`) và `youtubeChannelId`. App hiện bỏ qua các trường này – dùng khi muốn
  làm mục "Hot", "Mới", hoặc nhóm kênh.
- `list_channel.txt` không còn kênh con (`hasSubCategory` = false hết): bản app hiện tại không đọc
  `channels` lồng nhau nên các kênh con cũ (FAP TV, SVM TV, Kem Xôi…) đang mở ra danh sách lỗi.

## Kiểm duyệt nội dung
Toàn bộ truyện và ~2.900 câu chém gió rủi ro được đọc và chấm từng mục (giữ/bỏ + lý do + danh mục mới +
điểm vui 1–5), sau đó lọc từ khoá lần cuối. Bỏ: tình dục/ẩn ý tục, chửi thề, miệt thị vùng miền/giới/
khuyết tật/ngoại hình, chính trị, đùa về chết chóc/dịch bệnh, rượu bia + lái xe, bạo hành; bỏ hẳn
2 danh mục "18+" và các câu hỏi-đáp "Ad ơi" từ fanpage cũ. Chi tiết số liệu: `_build/report_*.json`.

## Cập nhật lại video sau này
Video mới được lấy ngày 24/10/2026 và sẽ cũ dần. Danh sách kênh + YouTube channel ID nằm trong
`_build/video_new.json`; nên làm job định kỳ (vd. GitHub Action hằng tuần dùng YouTube Data API)
để lấy ~20 video mới nhất mỗi kênh và chạy `_build/scripts/verify.js` trước khi push.

`_build/` chỉ là tài liệu/script dựng lại – không cần đẩy lên repo data.
