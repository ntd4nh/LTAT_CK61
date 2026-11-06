# Tóm tắt kế hoạch — bản cho thành viên nhóm

Bản này giúp mỗi người nhận vai, biết làm việc gì trước và khi nào một việc được coi là xong. Mọi chi tiết (phạm vi đầy đủ, tiêu chí hoàn thành đầy đủ, schema, rủi ro, nguồn trích dẫn) nằm trong [kế hoạch đầy đủ](ke-hoach-trien-khai-nhom-3-nguoi.md). Khi hai bản lệch nhau, theo bản đầy đủ.

## 1. Nhóm làm gì

Xây một công cụ nhận một tệp Solidity (`.sol`) và **đánh dấu các câu lệnh có thể bị lỗi reentrancy**, theo phương pháp của bài báo:

1. **Tạo dữ liệu có nhãn tự động** bằng cách tìm các đoạn mã có cơ chế chống reentrancy (ví dụ modifier `nonReentrant`), gỡ cơ chế đó ra, rồi gán nhãn các câu lệnh liên quan.
2. **Biến mỗi hàm thành một đồ thị**: mỗi nút là một câu lệnh, kèm cây cú pháp (AST) của câu lệnh đó; các cạnh là luồng điều khiển (CFG).
3. **Huấn luyện mô hình SS-GNN**: Tree-LSTM đọc cây cú pháp của từng câu lệnh, GNN đọc quan hệ giữa các câu lệnh, rồi dự đoán từng câu lệnh có lỗi hay không.

Đầu ra demo: danh sách câu lệnh nghi ngờ, kèm hàm, số dòng, đoạn mã và điểm số, xem được trên terminal, dạng JSON hoặc trang HTML tô sáng.

**Điều cần biết trước khi làm**

- Bộ dữ liệu của tác giả **có công khai** (`smartcontract-detect-yzu/sbp_dataset` trên GitHub). Mã mô hình SS-GNN **không tìm thấy**. Toàn văn bài báo **chưa đọc được**.
- Nhóm theo nhánh rubric "bài báo không có mã nguồn công khai": tự hiện thực các kỹ thuật chính và phân tích ưu, nhược điểm. **Phải hỏi giảng viên xác nhận nhánh này** (việc P-01), vì vài nguồn ghi bài này "có mã".
- Phạm vi chỉ có **reentrancy**. Bài báo (theo nguồn thứ cấp) xét 7 loại lỗ hổng, nên đây là phạm vi rút gọn.

## 2. Ba vai — chọn ai làm A, B, C

Mỗi vai sở hữu **một trụ cột** của bài báo, tự hiện thực, tự đánh giá và tự thuyết trình phần đó. Khối lượng ba vai gần bằng nhau.

| Vai | Trụ cột | Hợp với người thích | Điểm công sức | Người nhận vai |
| --- | --- | --- | ---: | --- |
| **A** | Dữ liệu dựa trên SBP: biên dịch, tìm và gỡ cơ chế bảo vệ, gán nhãn | Solidity, bảo mật smart contract | 49 | ____________ |
| **B** | Đồ thị mã: AST, CFG, ánh xạ về dòng mã; demo; bộ tính chỉ số | Trình biên dịch, thuật toán đồ thị | 48 | ____________ |
| **C** | Mô hình: Tree-LSTM, GNN, huấn luyện, thực nghiệm; chia dữ liệu | PyTorch, học máy | 48 | ____________ |

Điểm là công sức tương đối (thang 1-2-3-5-8), không phải thời gian. Ngoài phần trên, mỗi người còn review việc của người khác: A 12, B 10, C 11 điểm review.

## 3. Thứ tự các chặng

```text
Chặng 0  Đặc tả và khung repo
   ↓
Chặng 1  Schema và dữ liệu giả (mock) để làm song song
   ↓
Chặng 2  Mỗi người xây phần lõi của mình  ──►  Mốc I-01: chạy thông trên dữ liệu giả
   ↓
Chặng 3  Dữ liệu thật có nhãn và đồ thị thật  ──►  Mốc I-02: chạy thông trên một ít dữ liệu thật
   ↓
Chặng 4  Lọc trùng, chia tập, kiểm tra nhãn bằng tay, baseline
   ↓
Chặng 5  Thực nghiệm chính và phân tích lỗi
   ↓
Chặng 6  Demo và kiểm tra người khác chạy lại được
   ↓
Chặng 7  Báo cáo, slide, diễn tập
```

Không ai phải ngồi chờ: C dùng đồ thị giả của B để làm mô hình; B dùng dự đoán giả để làm bộ tính chỉ số; A làm trên các tệp `.sol` mẫu tự viết (fixture) trong lúc corpus còn đang biên dịch.

## 4. Việc của từng người

Cột "Xong khi" là bản rút gọn; tiêu chí đầy đủ ở mục C của kế hoạch đầy đủ. Số trong ngoặc là điểm công sức.

### Thành viên A — Dữ liệu SBP (49 điểm)

| # | Việc | Cần có trước | Xong khi | Review |
| --- | --- | --- | --- | --- |
| 1 | **P-01** (2) Sổ căn cứ; hỏi giảng viên về nhánh rubric | — | Mọi khẳng định về bài báo có trang hoặc URL; có ý kiến của giảng viên | C |
| 2 | **P-02** (3) Đặc tả cơ chế bảo vệ (SBP) và luật gán nhãn, ≥12 tệp `.sol` mẫu | — (cần P-05 chỉ để biên dịch tệp mẫu) | B xác nhận đủ để gắn nhãn vào nút | B |
| 3 | **S-01** (2) Schema `sample-v1` và bộ kiểm tra | P-02, P-05 | Nhận mẫu đúng, báo đúng trường lỗi ở mẫu sai | B |
| 4 | **D-01** (3) Thu thập hợp đồng (ưu tiên `sbp_dataset`), lập manifest, khảo sát đủ dữ liệu chưa | P-02, P-05 | Manifest đủ nguồn và sha256; có kết luận đi tiếp hay bổ sung nguồn | C |
| 5 | **D-02** (5) Biên dịch hàng loạt nhiều phiên bản solc, hàm `compile_file()` dùng chung | D-01 | Có cache; mọi tệp có trạng thái và lý do lỗi | B |
| 6 | **D-03** (5) Tìm cơ chế khóa: modifier `nonReentrant` và khóa viết trong thân hàm | G-01, D-02 | Đúng 100% tệp mẫu; kiểm tra tay 60 hàm trên dữ liệu thật | B |
| 7 | **D-04** (5) Gỡ cơ chế khóa, biên dịch lại, so AST trước và sau | D-03 | Mọi biến thể biên dịch được; AST chỉ khác đúng phần đã gỡ | B |
| 8 | **D-05** (5) Gán nhãn câu lệnh, xuất `samples` | D-04, G-02, S-01 | Qua bộ kiểm tra; mỗi nhãn dương rơi đúng một nút | C |
| 9 | **I-02** (3) Chạy thông cả chuỗi trên một ít dữ liệu thật | D-05, G-04, G-05, M-04, E-01 | Một lệnh chạy hết; lỗi tìm được đã giao đúng chủ module | B |
| 10 | **D-10** (3) Kiểm kê và đối chiếu bộ dữ liệu của tác giả | D-01, D-05 | Biết tệp chứa gì; nếu có nhãn thì có bảng so sánh với nhãn của nhóm | C |
| 11 | **E-02** (3) Baseline B0 bằng luật (không học máy) | G-04, D-05, S-03 | Đúng trên tệp mẫu; chạy được qua bộ tính chỉ số | C |
| 12 | **D-08** (3) Thống kê dữ liệu, phân tích nhãn mâu thuẫn | D-06, D-07, D-09, E-03 | Bảng sinh lại được bằng script; có mục hạn chế nhãn kèm số liệu | C |
| 13 | **R-04** (2) Báo cáo và slide phần bối cảnh bài toán | P-01 | Mọi khẳng định về bài báo có số trang | C |
| 14 | **R-01** (3) Báo cáo và slide phần dữ liệu SBP | D-08, D-10, E-02 | Có ví dụ trước và sau khi gỡ khóa; ≥3 hạn chế có số liệu | C |
| 15 | **R-05** (2) Biên tập báo cáo tổng, bảng đối chiếu rubric | R-01…R-04, E-05 | Mỗi tiêu chí rubric có minh chứng cụ thể | B |

**A review cho:** P-04, G-01, G-03, M-03, E-01, D-06, D-07, D-09, E-03, I-03, I-04 (chạy lại toàn bộ từ đầu), R-02.

### Thành viên B — Đồ thị mã, demo, đánh giá (48 điểm)

| # | Việc | Cần có trước | Xong khi | Review |
| --- | --- | --- | --- | --- |
| 1 | **P-03** (3) Đặc tả đồ thị (nút, cạnh, cách chuẩn hóa token), ≥15 tệp `.sol` mẫu | — (cần P-05 chỉ để biên dịch tệp mẫu) | C xác nhận đủ để viết schema | C |
| 2 | **S-02** (3) Schema `graph-v1`, bộ kiểm tra, ≥200 đồ thị giả cho C | P-03, P-05 | Đồ thị giả qua bộ kiểm tra; C nạp được | C |
| 3 | **G-01** (5) Đọc AST, chuẩn hóa giữa các phiên bản solc, đổi vị trí byte sang dòng–cột | P-03, P-05 | Trích đúng đoạn mã kể cả khi có chữ Unicode hay xuống dòng kiểu Windows | A |
| 4 | **G-02** (5) Tách nút câu lệnh và cây AST con; hàm `enclosing_statement` | G-01, S-02 | Khớp tệp mẫu. **Giao hàm `enclosing_statement` cho A ngay khi qua test** | C |
| 5 | **E-01** (3) Bộ tính Precision, Recall, F1, PR-AUC | S-03, P-04 | Khớp ví dụ tính tay; bắt được lỗi đếm nhầm FP và FN | A |
| 6 | **G-03** (8) Dựng CFG cấp câu lệnh — việc khó nhất của nhóm | G-02 | Cạnh khớp tệp mẫu; từ ENTRY đi tới được mọi nút | A |
| 7 | **G-04** (3) Xuất đồ thị cho toàn bộ dữ liệu, gắn nhãn | G-03, D-05 | Tổng nhãn trong đồ thị khớp với `samples` | C |
| 8 | **E-03** (3) Baseline Slither | D-06, G-04 | Báo tỷ lệ chạy được; kiểm tra tay 10 cảnh báo | A |
| 9 | **D-09** (2) Chấm nhãn bằng tay lượt 2 (độc lập với lượt 1) | D-07 | Chấm đủ 100% mẫu | A |
| 10 | **E-05** (3) Phân tích cảnh báo nhầm và bỏ sót | E-04, D-07, D-09 | Có bảng nguyên nhân; có chỉ số trên phần nhãn đã kiểm tra tay | C |
| 11 | **I-03** (5) Demo CLI và 5 tệp mẫu | M-04, G-04, D-02, I-01 (bản cuối cần checkpoint từ E-04) | Test 5 mẫu đạt; A chạy lại được trên máy mình | A |
| 12 | **R-02** (3) Báo cáo và slide phần đồ thị và demo | G-04, I-03, E-03, E-05 | Sơ đồ khớp đầu ra thật | A |
| 13 | **R-06** (2) Ghép slide, ngân hàng câu hỏi, diễn tập | R-01…R-04, I-03 | Cả 3 người diễn tập phần mình, có demo | C |

**B review cho:** P-02, P-05, S-01, S-03, D-02, D-03, D-04, G-05, M-01, M-02, M-04, I-01, I-02, E-04, R-03, R-05.

### Thành viên C — Mô hình và thực nghiệm (48 điểm)

| # | Việc | Cần có trước | Xong khi | Review |
| --- | --- | --- | --- | --- |
| 1 | **P-05** (2) Khung repo và môi trường. **Làm đầu tiên**, vì A và B cần nó để biên dịch tệp mẫu | — | B cài theo README và chạy được `pytest` | B |
| 2 | **P-04** (3) Đặc tả mô hình và cách làm thí nghiệm | — | Mỗi chỉ số có công thức; không lấy 91,40%/89,77% làm mục tiêu | A |
| 3 | **S-03** (2) Schema `prediction-v1` và `metrics-v1` | P-04, P-05 | B viết được E-01 chỉ từ schema | B |
| 4 | **M-01** (5) Child-sum Tree-LSTM xử lý theo tầng | P-04 | Khớp bản cài đặt đệ quy trên ≥100 cây ngẫu nhiên | B |
| 5 | **G-05** (5) Từ vựng và chuyển đồ thị sang tensor PyG | S-02 | Từ vựng chỉ xây từ tập train; gộp batch đúng chỉ số | B |
| 6 | **M-02** (5) SS-GNN và hai biến thể so sánh B2, B3 | M-01, G-05 | Cả 3 cấu hình chạy được forward và backward | B |
| 7 | **M-03** (5) Huấn luyện, lưu và nạp checkpoint | M-02, S-03 | Học thuộc được 1–2 đồ thị; lưu rồi nạp lại cho dự đoán giống hệt | A |
| 8 | **M-04** (2) Xuất dự đoán | M-03 | Qua bộ kiểm tra; chạy hai lần ra giống nhau | B |
| 9 | **I-01** (2) Chạy thông cả chuỗi trên dữ liệu giả | S-02, G-05, M-03, M-04, E-01 | Một lệnh ra được `metrics.json` | B |
| 10 | **D-06** (3) Lọc trùng, chia train/val/test theo nhóm, gắn tag `data-v1` | G-04, D-05, D-01 | Không nhóm hợp đồng nào, không hàm trùng nào nằm ở hai tập | A |
| 11 | **D-07** (5) Kiểm tra nhãn bằng tay lượt 1, tính kappa với lượt 2 | D-06, D-05 | Đủ mẫu; kappa có khoảng tin cậy; bất đồng đã phân xử | A |
| 12 | **E-04** (3) Thực nghiệm chính: SS-GNN, B2, B3, B0, B1, ≥3 seed | D-06, G-05, M-04, E-01, E-02, E-03, I-02 | Mọi số trong bảng truy được về một lần chạy trong registry | B |
| 13 | **I-04** (3) Lệnh chạy lại toàn bộ, hướng dẫn tái lập | E-04, I-03 | A chạy lại từ đầu ra đúng chỉ số | A |
| 14 | **R-03** (3) Báo cáo và slide phần mô hình và kết quả | E-04 | Số liệu truy về registry; có ưu và nhược điểm | B |

**C review cho:** P-01, P-03, S-02, D-01, G-02, D-05, G-04, D-10, D-08, E-02, E-05, R-04, R-01, R-06.

### Phần mở rộng — chỉ làm khi đã xong phần của mình và E-04 đã xong

| Người | Việc |
| --- | --- |
| A | X-02 tập kiểm thử có nhãn do người gán; X-04 thêm lỗi tràn số (SafeMath); X-06 giao diện web |
| B | X-03 thêm cơ chế checks-effects-interactions; X-07 thử cách gán nhãn và dựng đồ thị của tác giả |
| C | X-01 so sánh 4 loại GNN; X-05 thêm cạnh phụ thuộc dữ liệu |

Nên làm X-01, X-07, X-02 trước, vì chúng gần với kịch bản của bài báo nhất (giúp lấy điểm xuất sắc).

## 5. Bắt đầu ngay

| Người | Làm ngay | Làm tiếp khi việc trước xong |
| --- | --- | --- |
| A | P-01, P-02 | S-01, D-01, rồi D-02 |
| B | P-03 | S-02, G-01, rồi G-02 và E-01 |
| C | **P-05 trước tiên**, rồi P-04 | S-03, M-01, rồi G-05 |

Ba chỗ bàn giao dễ làm người khác phải chờ nhất:

- C giao khung repo (P-05) → A và B.
- B giao hàm `enclosing_statement` (G-02) → A.
- A giao `compile_file()` (D-02) → B.

## 6. Quy tắc làm việc chung

- **Mỗi việc một nhánh Git**, đặt tên theo mã việc, ví dụ `task/D-03-sbp-detect`. Chỉ gộp vào `main` khi người review trong bảng chấp nhận.
- **Người review phải chạy lại** test hoặc lệnh, không chỉ đọc mã.
- **Xong** nghĩa là đạt hết tiêu chí trong kế hoạch đầy đủ **và** đã được review.
- **Lỗi khi ghép module** được mở issue và giao cho chủ module có lỗi. Không tự sửa module của người khác, trừ khi chủ module đồng ý.
- **Đổi schema** thì tăng phiên bản (ví dụ `graph-v2`), và cả bên gửi lẫn bên nhận phải đồng ý.
- **Tập test bị khóa:** chọn ngưỡng và siêu tham số trên tập validation; chỉ chạy test với cấu hình đã chốt.
- **Ghi nguồn rõ ràng** trong đặc tả và báo cáo: TL (theo bài báo, kèm trang), NTC (nguồn thứ cấp hoặc mã/dữ liệu của tác giả), ĐX (nhóm đề xuất), CXM (cần xác minh).
- **Không đưa lên Git** thư mục `data/` và `runs/`. Không phát hành lại dữ liệu của tác giả; chỉ trích dẫn.

## 7. Những chỗ dễ sai

- Nhãn **neutral không có nghĩa là an toàn**. Nhãn 1 cũng **không có nghĩa là chắc chắn khai thác được**. Phải nói rõ trong báo cáo.
- **Không so** kết quả của nhóm với 91,40% / 89,77%: đó là kết quả **cấp hợp đồng** của bài báo, còn nhóm đo **cấp câu lệnh**.
- Vị trí trong AST của solc tính theo **byte**, không theo ký tự. Tệp trên Windows thường dùng xuống dòng CRLF.
- **Không dùng lại** hàm `calculate_metrics()` trong repo BSP_SC của tác giả: nó đếm nhầm FP và FN.
- Đồ thị của nhóm **không chứa modifier**, nên hàm có `nonReentrant` và hàm đã gỡ khóa cho **cùng một đồ thị**. Đây là hạn chế cần trình bày. Tác giả làm khác (có nút ảo `v_modifier`); X-07 thử cách đó.
- Tác giả đặt nhãn reentrancy ở nút `v_modifier`, còn nhóm đặt ở câu lệnh gọi ra ngoài. D-10 đo mức khác nhau giữa hai cách.
