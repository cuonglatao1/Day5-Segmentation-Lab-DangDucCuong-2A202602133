# Báo cáo Day 5 — điền trực tiếp trong fork của bạn

**Cách dùng:** Thay mọi dấu `…` bằng bài làm thật của bạn trước khi nộp link fork trên VLearn. Giữ nguyên bốn mục và bảng để coach đọc nhanh. Viết ngắn, cụ thể theo ảnh/vùng; không cần thuật ngữ chuyên sâu. Ví dụ trong [hướng dẫn mẫu](reports/REPORT_TEMPLATE.md) chỉ giúp hiểu cách điền, không phải câu trả lời để chép lại.

- Mã học viên theo lớp: 2A202602133 - Đặng Đức Cường
- Ngày / CVAT local: 17/09/2026 / CVAT local
- Công cụ đã dùng: Brush, Polygon, gợi ý tự động

Mã học viên là mã lớp cấp; không cần ghi họ tên trong report nếu kênh VLearn đã nhận diện bạn. Chỉ ghi công cụ thật sự đã dùng; không có SAM vẫn làm bài bình thường.

## 1. Bài đã nộp

Ghi tên ZIP đúng như file trong `submissions/` và số ảnh đã vẽ, Save. Chưa làm hoặc export lỗi thì ghi `chưa có`, không tạo ZIP rỗng. Cột điểm là điểm tối đa của task, **không phải điểm tự chấm**.

| Task | File ZIP đúng tên | Hoàn thành mấy ảnh | Điểm tối đa (coach chấm sau) |
| --- | --- | ---: | ---: |
| easy_semantic | easy_semantic.zip | 3 / 3 | 20 |
| medium_instance | medium_instance.zip | 3 / 3 | 32 |
| hard_panoptic | hard_panoptic.zip | 2 / 2 | 30 |
| cp1_holes | cp1_holes.zip | 1 / 1 | 3 |
| cp2_slice | cp2_slice.zip | 1 / 1 | 3 |
| cp5_occlusion | cp5_occlusion.zip | 1 / 1 | 3 |
| cp3_thin | cp3_thin.zip | 1 / 1 | 3 |
| cp4_curb | cp4_curb.zip | 1 / 1 | 3 |
| cp6_coverage | cp6_coverage.zip | 1 / 1 | 3 |
| **Tổng tối đa** | | | **100** |

Nếu export lỗi, ghi task, dữ liệu đã Save đến đâu và lỗi đã báo coach.

## 2. Một quyết định trước khi dùng gợi ý

Chọn object đầu tiên bạn tự vẽ ở `medium_instance`, trước khi xem bất kỳ đề xuất tự động nào cho object đó. Ghi ảnh/vị trí đủ để tìm lại; “quy tắc biên” là lý do bạn chọn hoặc dừng mask ở ranh đó.

- Ảnh, vị trí và object Medium đầu tiên tự vẽ: 000000181542.jpg, xe car ở giữa ảnh phía trước
- Class và quy tắc tôi dùng để chọn biên: Class `car`. Chỉ vẽ phần thân xe nhìn thấy được, không đoán biên phía sau nếu bị che khuất bởi vật khác. Dừng mask tại ranh giới với vật che hoặc mép ảnh.
- Nếu dùng gợi ý sau đó: Có sử dụng gợi ý tự động cho các object tiếp theo. Một số gợi ý ăn tràn ra nền hoặc gộp nhầm hai xe gần nhau thành một object. Đã sửa bằng cách tách riêng từng xe thành instance độc lập và xóa vùng nền thừa. Quyết định giữ gợi ý khi biên khớp với phần nhìn thấy và class đúng.
- Nếu không dùng gợi ý: ghi "không dùng"; vẫn giải thích một quyết định gán nhãn của mình.

## 3. Một lỗi tôi tìm thấy và sửa

Chọn một lỗi **có thật** trong bài. Nếu công cụ lỗi khiến bạn chưa sửa được, ghi rõ đã thử gì và cần coach hỗ trợ gì; không ghi “đã sửa” khi chưa sửa.

- Task/ảnh/vùng: `medium_instance`, ảnh 000000373353.jpg, vùng có hai xe car đỗ sát nhau
- Lỗi thuộc loại: gộp-tách - gợi ý tự động gộp hai xe riêng biệt thành một object
- Bằng chứng tôi nhìn thấy: Nhìn thấy khe hở giữa hai thân xe và hai xe có màu khác nhau, rõ ràng là hai chiếc xe độc lập
- Quy tắc và hành động sửa: Theo quy tắc instance, hai vật cùng class là hai instance riêng. Đã xóa mask gộp và tạo hai object riêng biệt cho mỗi xe. Kiểm tra lại trong danh sách Objects để đảm bảo đúng số lượng.
- Sau sửa đã Save và export lại chưa? Đã Save và export lại.

**Kết quả tự chấm:**
- Đã chạy `python scoring/scorecard.py --dir submissions --out reports --group tiers`
- Kết quả ba tier: **47.8 / 82**
  - easy_semantic: 18.4/20 (mIoU 0.815)
  - medium_instance: 14.4/32 (metric 0.602, TP 55, FP 18, FN 16)
  - hard_panoptic: 15.0/30 (PQ 0.425)
- Medium instance còn FP=18 và FN=16, cho thấy vẫn còn một số object bị vẽ thừa/thiếu cần kiểm tra lại.

## 4. Ba ca chưa chắc hoặc đã cân nhắc

Mỗi ca là một **vùng cụ thể** khiến bạn phải cân nhắc hai cách hiểu. Ghi dấu hiệu nhìn thấy hoặc quy tắc đã dùng, rồi nêu quyết định hoặc câu hỏi cho coach. Không cần ba lỗi; ca đã quyết định được cũng hợp lệ.

| Ảnh/vị trí | Hai cách hiểu có thể | Quy tắc/chứng cứ | Quyết định hoặc câu hỏi cho coach |
| --- | --- | --- | --- |
| easy_semantic, 817bca71-00000000.jpg, ranh giới road-sidewalk | Phần màu giống mặt đường: gán cho `road` hay `sidewalk`? | Theo quy tắc, ranh road-sidewalk là ranh chức năng (bó vỉa), không chỉ theo màu sắc. Phần nền nâng cao là sidewalk. | Đã gán vùng bó vỉa cho `sidewalk`. Tuy nhiên vẫn còn phân vân tại vị trí màu ảnh gần giống - xin coach xác nhận ranh giới có đúng. |
| hard_panoptic, 000000350023.jpg, vùng người xa/nhỏ | Vùng mờ phía xa: có phải `person` không, hay bỏ qua vì quá nhỏ? | Quy tắc yêu cầu vẽ mọi instance nhìn thấy được. Nếu nhận diện được hình dạng người thì phải vẽ. | Đã vẽ những người thấy rõ. Còn lại 8 FN cho thấy có thể bỏ sót người nhỏ/xa. Câu hỏi: vùng mờ nhỏ hơn 10 pixel có cần vẽ không? |
| hard_panoptic, 000000460147.jpg, vùng sidewalk | Phần nền sát đường: `road` (stuff) hay `sidewalk` (stuff)? | Kết quả cho thấy sidewalk có 2 FP, 2 FN (PQ=0.000), nghĩa là có thể nhầm hoàn toàn với road. | Đã cố gắng phân biệt theo độ cao/chức năng nhưng kết quả cho thấy sai. Cần xem lại tiêu chí phân biệt road-sidewalk khi không có bó vỉa rõ ràng. |
