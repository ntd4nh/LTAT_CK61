# Trạng thái công việc

File này do AI agent cập nhật theo quy trình trong [AGENTS.md](../AGENTS.md). Có thể sửa tay, nhưng mỗi người chỉ sửa dòng của việc mình làm hoặc mình review.

## Thành viên

Điền tên khi đã chọn vai. AI agent dùng bảng này để ghi tên trong thông báo.

| Vai | Họ tên | Liên hệ (nhóm chat, GitHub) |
| --- | --- | --- |
| A — Dữ liệu SBP | | |
| B — Đồ thị, demo, đánh giá | | |
| C — Mô hình, thực nghiệm | | |

## Ý nghĩa trạng thái

| Trạng thái | Nghĩa |
| --- | --- |
| ⬜ Chưa sẵn sàng | Còn việc ở cột "Cần xong trước" chưa ✅ |
| 🟩 Sẵn sàng | Mọi việc ở cột "Cần xong trước" (trừ mã có dấu `*`) đã ✅; có thể bắt đầu |
| 🟦 Đang làm | Người chính đang làm |
| 🟨 Chờ review | Người chính đã xong và đã có báo cáo; đang chờ người review |
| 🟥 Cần sửa | Người review yêu cầu sửa; người chính sửa xong thì chuyển lại 🟨 |
| ✅ Xong | Người review đã chấp nhận |

Dấu `*` là **phụ thuộc mềm**: được bắt đầu trước khi việc đó xong. P-02 và P-03 chỉ cần P-05 để biên dịch tệp mẫu; I-03 chỉ cần E-04 khi thay checkpoint thật cho bản cuối.

Việc báo cáo (R-xx) được viết nháp phần không cần kết quả trước khi chuyển 🟩. Đợt lấy theo [mục 5 của bản tóm tắt](tom-tat-ke-hoach.md).

## Việc cốt lõi

| Mã | Việc | Người chính | Review | Cần xong trước | Đợt | Trạng thái | Báo cáo |
| --- | --- | --- | --- | --- | --- | --- | --- |
| P-01 | Sổ căn cứ; hỏi giảng viên về nhánh rubric | A | C | — | 1 | 🟩 Sẵn sàng | — |
| P-02 | Đặc tả cơ chế bảo vệ (SBP) và luật gán nhãn, ≥12 tệp `.sol` mẫu | A | B | P-05* | 1 | 🟩 Sẵn sàng | — |
| P-03 | Đặc tả đồ thị (nút, cạnh, cách chuẩn hóa token), ≥15 tệp `.sol` mẫu | B | C | P-05* | 1 | 🟩 Sẵn sàng | — |
| P-04 | Đặc tả mô hình và cách làm thí nghiệm | C | A | — | 1 | 🟩 Sẵn sàng | — |
| P-05 | Khung repo và môi trường (làm đầu tiên) | C | B | — | 1 | 🟩 Sẵn sàng | — |
| D-01 | Thu thập hợp đồng (ưu tiên `sbp_dataset`), lập manifest, khảo sát đủ dữ liệu chưa | A | C | P-05, P-02 | 2 | ⬜ Chưa sẵn sàng | — |
| R-04 | Báo cáo và slide phần bối cảnh bài toán | A | C | P-01 | 2 | ⬜ Chưa sẵn sàng | — |
| S-01 | Schema `sample-v1` và bộ kiểm tra | A | B | P-02, P-05 | 2 | ⬜ Chưa sẵn sàng | — |
| G-01 | Đọc AST, chuẩn hóa giữa các phiên bản solc, đổi vị trí byte sang dòng–cột | B | A | P-03, P-05 | 2 | ⬜ Chưa sẵn sàng | — |
| S-02 | Schema `graph-v1`, bộ kiểm tra, ≥200 đồ thị giả cho C | B | C | P-03, P-05 | 2 | ⬜ Chưa sẵn sàng | — |
| M-01 | Child-sum Tree-LSTM xử lý theo tầng | C | B | P-04 | 2 | ⬜ Chưa sẵn sàng | — |
| S-03 | Schema `prediction-v1` và `metrics-v1` | C | B | P-04, P-05 | 2 | ⬜ Chưa sẵn sàng | — |
| D-02 | Biên dịch hàng loạt nhiều phiên bản solc, hàm `compile_file()` dùng chung | A | B | D-01, P-05 | 3 | ⬜ Chưa sẵn sàng | — |
| E-01 | Bộ tính Precision, Recall, F1, PR-AUC | B | A | S-03, P-04 | 3 | ⬜ Chưa sẵn sàng | — |
| G-02 | Tách nút câu lệnh và cây AST con; hàm `enclosing_statement` | B | C | G-01, S-02 | 3 | ⬜ Chưa sẵn sàng | — |
| G-05 | Từ vựng và chuyển đồ thị sang tensor PyG | C | B | S-02 | 3 | ⬜ Chưa sẵn sàng | — |
| D-03 | Tìm cơ chế khóa: modifier `nonReentrant` và khóa viết trong thân hàm | A | B | G-01, D-02, P-02 | 4 | ⬜ Chưa sẵn sàng | — |
| G-03 | Dựng CFG cấp câu lệnh — việc khó nhất của nhóm | B | A | G-02 | 4 | ⬜ Chưa sẵn sàng | — |
| M-02 | SS-GNN và hai biến thể so sánh B2, B3 | C | B | M-01, G-05 | 4 | ⬜ Chưa sẵn sàng | — |
| M-03 | Huấn luyện, lưu và nạp checkpoint | C | A | M-02, S-03 | 4 | ⬜ Chưa sẵn sàng | — |
| M-04 | Xuất dự đoán | C | B | M-03, S-03 | 4 | ⬜ Chưa sẵn sàng | — |
| D-04 | Gỡ cơ chế khóa, biên dịch lại, so AST trước và sau | A | B | D-03, D-02 | 5 | ⬜ Chưa sẵn sàng | — |
| D-05 | Gán nhãn câu lệnh, xuất `samples` | A | C | D-04, G-02, S-01 | 5 | ⬜ Chưa sẵn sàng | — |
| I-01 | Chạy thông cả chuỗi trên dữ liệu giả | C | B | S-02, G-05, M-03, M-04, E-01 | 5 | ⬜ Chưa sẵn sàng | — |
| D-10 | Kiểm kê và đối chiếu bộ dữ liệu của tác giả | A | C | D-01, D-05 | 6 | ⬜ Chưa sẵn sàng | — |
| I-02 | Chạy thông cả chuỗi trên một ít dữ liệu thật | A | B | D-05, G-04, G-05, M-04, E-01 | 6 | ⬜ Chưa sẵn sàng | — |
| G-04 | Xuất đồ thị cho toàn bộ dữ liệu, gắn nhãn | B | C | G-03, D-05, S-01, S-02 | 6 | ⬜ Chưa sẵn sàng | — |
| D-06 | Lọc trùng, chia train/val/test theo nhóm, gắn tag `data-v1` | C | A | G-04, D-05, D-01 | 6 | ⬜ Chưa sẵn sàng | — |
| E-02 | Baseline B0 bằng luật (không học máy) | A | C | G-04, D-05, S-03 | 7 | ⬜ Chưa sẵn sàng | — |
| E-03 | Baseline Slither | B | A | D-06, G-04, S-03 | 7 | ⬜ Chưa sẵn sàng | — |
| I-03 | Demo CLI và 5 tệp mẫu | B | A | M-04, G-04, D-02, I-01, E-04* | 7 | ⬜ Chưa sẵn sàng | — |
| D-07 | Kiểm tra nhãn bằng tay lượt 1, tính kappa với lượt 2 | C | A | D-06, D-05 | 7 | ⬜ Chưa sẵn sàng | — |
| D-08 | Thống kê dữ liệu, phân tích nhãn mâu thuẫn | A | C | D-06, D-07, D-09, E-03 | 8 | ⬜ Chưa sẵn sàng | — |
| R-01 | Báo cáo và slide phần dữ liệu SBP | A | C | D-08, D-10, E-02 | 8 | ⬜ Chưa sẵn sàng | — |
| D-09 | Chấm nhãn bằng tay lượt 2 (độc lập với lượt 1) | B | A | D-07 | 8 | ⬜ Chưa sẵn sàng | — |
| E-04 | Thực nghiệm chính: SS-GNN, B2, B3, B0, B1, ≥3 seed | C | B | D-06, G-05, M-04, E-01, E-02, E-03, I-02 | 8 | ⬜ Chưa sẵn sàng | — |
| E-05 | Phân tích cảnh báo nhầm và bỏ sót | B | C | E-04, D-07, D-09 | 9 | ⬜ Chưa sẵn sàng | — |
| R-03 | Báo cáo và slide phần mô hình và kết quả | C | B | E-04 | 9 | ⬜ Chưa sẵn sàng | — |
| R-05 | Biên tập báo cáo tổng, bảng đối chiếu rubric | A | B | R-01, R-02, R-03, R-04, E-05 | 10 | ⬜ Chưa sẵn sàng | — |
| R-02 | Báo cáo và slide phần đồ thị và demo | B | A | G-04, I-03, E-03, E-05 | 10 | ⬜ Chưa sẵn sàng | — |
| R-06 | Ghép slide, ngân hàng câu hỏi, diễn tập | B | C | R-01, R-02, R-03, R-04, I-03 | 10 | ⬜ Chưa sẵn sàng | — |
| I-04 | Lệnh chạy lại toàn bộ, hướng dẫn tái lập | C | A | E-04, I-03 | 10 | ⬜ Chưa sẵn sàng | — |

## Việc mở rộng

Chỉ chuyển sang 🟩 khi E-04 đã ✅ **và** người chính đã ✅ mọi việc cốt lõi của mình.

| Mã | Việc | Người chính | Review | Cần xong trước | Đợt | Trạng thái | Báo cáo |
| --- | --- | --- | --- | --- | --- | --- | --- |
| X-02 | Tập kiểm thử có nhãn do người gán (SmartBugs-curated) | A | C | E-04, I-03 | sau 8 | ⬜ Chưa sẵn sàng | — |
| X-04 | Thêm lỗi tràn số (SafeMath) | A | C | E-04, D-05 | sau 8 | ⬜ Chưa sẵn sàng | — |
| X-06 | Giao diện web cho demo | A | B | I-03 | sau 8 | ⬜ Chưa sẵn sàng | — |
| X-03 | Thêm SBP checks-effects-interactions | B | A | E-04, G-03, D-04 | sau 8 | ⬜ Chưa sẵn sàng | — |
| X-07 | Thử cách gán nhãn và dựng đồ thị của tác giả (v_modifier) | B | C | E-04, G-03, D-10 | sau 8 | ⬜ Chưa sẵn sàng | — |
| X-01 | So sánh 4 loại GNN | C | B | E-04 | sau 8 | ⬜ Chưa sẵn sàng | — |
| X-05 | Thêm cạnh phụ thuộc dữ liệu | C | B | E-04, G-03 | sau 8 | ⬜ Chưa sẵn sàng | — |

## Nhật ký bàn giao

AI agent thêm một dòng mỗi khi trạng thái một việc thay đổi. Dòng mới nhất ở dưới cùng.

| # | Mã | Thay đổi | Người thực hiện | Commit | Ghi chú |
| --- | --- | --- | --- | --- | --- |
