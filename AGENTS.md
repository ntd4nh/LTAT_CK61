# Hướng dẫn cho AI agent trong repo này

Repo của đồ án *Fine-grained smart contract vulnerability detection by heterogeneous code feature learning and automated dataset construction*, nhóm 3 người, mang vai A, B, C.

Mục tiêu chính của file này: **mỗi khi một thành viên dùng AI agent làm xong một việc, agent phải viết báo cáo, cập nhật trạng thái và thông báo ai làm việc tiếp theo.**

| Tệp | Dùng để |
| --- | --- |
| [plan/ke-hoach-trien-khai-nhom-3-nguoi.md](plan/ke-hoach-trien-khai-nhom-3-nguoi.md) | Kế hoạch đầy đủ. Phạm vi, sản phẩm và **tiêu chí hoàn thành** của từng việc nằm ở mục C |
| [plan/tom-tat-ke-hoach.md](plan/tom-tat-ke-hoach.md) | Bản tóm tắt; thứ tự làm việc theo đợt ở mục 5 |
| [plan/trang-thai-cong-viec.md](plan/trang-thai-cong-viec.md) | Trạng thái từng việc, tên thành viên, nhật ký bàn giao |
| `plan/bao-cao-viec/<MÃ>.md` | Báo cáo hoàn thành của từng việc, theo mẫu ở mục 5 dưới đây |

## 1. Khi bắt đầu làm một việc

1. Xác định người dùng là vai nào (A, B hay C) và đang làm việc mã nào. Suy từ tên nhánh `task/<MÃ>-...` nếu có, không thì hỏi.
2. Đọc dòng của việc đó trong bảng mục C của kế hoạch đầy đủ: phạm vi, sản phẩm bàn giao, tiêu chí hoàn thành.
3. Kiểm tra trạng thái của việc trong `plan/trang-thai-cong-viec.md`:
   - **🟩 Sẵn sàng:** đổi sang 🟦 Đang làm và thêm một dòng nhật ký.
   - **⬜ Chưa sẵn sàng:** báo người dùng việc nào còn chặn và ai đang làm việc đó. Chỉ làm tiếp trên fixture hoặc mock nếu kế hoạch cho phép.

## 2. Khi người dùng báo đã làm xong một việc

Áp dụng khi người dùng nói "xong", "hoàn thành <MÃ>", "done", hoặc khi agent vừa làm xong phần việc được giao. Làm đủ các bước theo thứ tự:

1. **Kiểm tra tiêu chí hoàn thành.**
   - Chạy test và lệnh liên quan, rồi đối chiếu từng tiêu chí trong kế hoạch.
   - Nếu còn tiêu chí chưa đạt thì **không** chuyển sang Chờ review. Báo rõ còn thiếu gì và dừng ở đây.
2. **Viết báo cáo** `plan/bao-cao-viec/<MÃ>.md` theo mẫu ở mục 5.
3. **Cập nhật** `plan/trang-thai-cong-viec.md`:
   - đổi dòng của việc sang 🟨 Chờ review;
   - điền link báo cáo vào cột "Báo cáo";
   - thêm một dòng nhật ký.
4. **Xác định ai làm tiếp** theo mục 4.
5. **In thông báo** theo mẫu A ở mục 6, để người dùng gửi vào nhóm chat.
6. **Commit trên nhánh của việc** với thông điệp `<MÃ>: <tóm tắt ngắn>`.
   - Không tự gộp vào `main`.
   - Nhắc người dùng mở pull request và gán người review ghi trong cột Review.

## 3. Khi người review báo kết quả

Áp dụng khi người dùng nói "duyệt <MÃ>", "review xong <MÃ>", "đạt", "cần sửa <MÃ>". Người review không được là người chính của chính việc đó.

1. Chạy lại các lệnh ở mục "Cách kiểm tra lại" trong báo cáo, rồi ghi kết quả vào mục "Kết quả review" của báo cáo. Không chấp nhận một việc chỉ vì đọc mã thấy ổn.
2. Nếu **đạt**:
   - đổi dòng của việc sang ✅ Xong;
   - tìm các việc được mở khóa (mục 4b) và đổi chúng sang 🟩 Sẵn sàng;
   - thêm dòng nhật ký;
   - in thông báo theo mẫu B.
3. Nếu **chưa đạt**:
   - đổi dòng sang 🟥 Cần sửa;
   - ghi cụ thể cần sửa gì vào báo cáo;
   - thêm dòng nhật ký;
   - in thông báo theo mẫu C.
4. Commit kèm thông điệp `<MÃ>: review <đạt | cần sửa>`.

## 4. Cách xác định việc tiếp theo và người làm

Luôn dùng bảng trong `plan/trang-thai-cong-viec.md`.

**a. Người review:** lấy ở cột "Review" của việc vừa xong.

**b. Việc được mở khóa:** một việc X được mở khóa khi thỏa cả hai điều kiện:
- mã của việc vừa xong có trong cột "Cần xong trước" của X;
- mọi mã **không có dấu `*`** trong cột đó đều đã ✅.

Người làm việc X là người ở cột "Người chính" của X; lấy tên từ bảng Thành viên.

Một việc chỉ thực sự mở khóa việc khác khi nó chuyển sang ✅. Khi việc mới ở 🟨 Chờ review, chỉ liệt kê dưới dạng "sẽ mở khóa khi được duyệt".

**c. Việc tiếp theo của chính người dùng** (theo thứ tự ưu tiên):
1. Việc 🟥 Cần sửa của họ.
2. Việc review người khác đang chờ họ. Ưu tiên việc trên đường găng: G-01, G-02, G-03, G-04, D-06, E-04.
3. Việc 🟩 Sẵn sàng của vai đó có số "Đợt" nhỏ nhất.
4. Nếu không còn gì: việc chuẩn bị trước theo phần "Ghi chú về thứ tự" ở mục 5 của bản tóm tắt (viết nháp báo cáo, làm trên mock, viết sẵn mã trên dữ liệu mẫu).

**d. Việc mở rộng (X-xx):** chỉ sẵn sàng khi E-04 đã ✅ **và** người đó đã ✅ mọi việc cốt lõi của mình.

**e. Kiểm tra trước khi in thông báo:** không ghi một việc là "đã mở khóa" nếu còn mã nào chưa ✅. Phải ghi rõ việc đó còn chờ mã nào.

## 5. Mẫu báo cáo hoàn thành

Tạo tệp `plan/bao-cao-viec/<MÃ>.md`. Viết ngắn và cụ thể; mọi khẳng định phải có minh chứng là lệnh, test, tệp hoặc số liệu.

```markdown
# <MÃ> — <tên việc>

| | |
| --- | --- |
| Người chính | <vai> – <tên> |
| Người review | <vai> – <tên> |
| Trạng thái | 🟨 Chờ review |
| Nhánh / PR | `task/<MÃ>-...` / #<số PR> |

## 1. Đã làm gì
- <3–5 gạch đầu dòng>

## 2. Sản phẩm bàn giao
| Sản phẩm | Đường dẫn | Ai dùng tiếp |
| --- | --- | --- |

## 3. Đối chiếu tiêu chí hoàn thành
| Tiêu chí (chép từ kế hoạch) | Đạt? | Minh chứng |
| --- | --- | --- |

## 4. Cách kiểm tra lại (cho người review)
Các lệnh cần chạy, và kết quả mong đợi của từng lệnh.

## 5. Khác với kế hoạch và quyết định mới
Ghi nhãn ĐX hoặc CXM cho từng quyết định. Nếu không có thì ghi "Không có".

## 6. Vấn đề còn mở và rủi ro

## 7. Bàn giao tiếp
- Người review: <vai> – <tên>
- Sẽ mở khóa khi được duyệt: <MÃ> <tên> (<vai> – <tên>), ...
- Việc tiếp theo của tôi: <MÃ> <tên>

## 8. Phần AI agent đã hỗ trợ
Ghi trung thực phần nào do AI viết hoặc gợi ý, phần nào người làm tự kiểm tra.

## 9. Kết quả review (người review điền)
- Kết quả: Đạt / Cần sửa
- Đã chạy lại:
- Cần sửa (nếu có):
```

## 6. Mẫu thông báo cho nhóm chat

In nguyên khối văn bản, thay phần trong `< >` bằng thông tin thật và lấy tên từ bảng Thành viên. Bỏ dòng nào không có nội dung. Không dùng `@tên` GitHub nếu bảng Thành viên chưa có tài khoản GitHub thật.

**Mẫu A — người chính làm xong, chờ review**

```text
📣 [<MÃ>] <tên việc> — 🟨 Chờ review
Người làm: <vai> – <tên>
👉 Nhờ review: <vai> – <tên>. Báo cáo: plan/bao-cao-viec/<MÃ>.md (PR #<số>)
🔓 Khi được duyệt sẽ mở khóa:
   • <MÃ> <tên việc> — <vai> – <tên>
⏳ Vẫn còn chờ thêm: <MÃ> (cần <MÃ chưa xong>)
⏭️ Việc tiếp theo của <tên người làm>: <MÃ> <tên việc>
```

**Mẫu B — review đạt**

```text
✅ [<MÃ>] <tên việc> — Đã duyệt bởi <vai> – <tên>
🔓 Đã mở khóa, có thể bắt đầu ngay:
   • <MÃ> <tên việc> — <vai> – <tên>
⏳ Vẫn đang chờ: <MÃ> (cần <MÃ chưa xong>)
⏭️ Việc tiếp theo của <tên người review>: <MÃ> <tên việc>
```

**Mẫu C — review cần sửa**

```text
🟥 [<MÃ>] <tên việc> — Cần sửa (review bởi <vai> – <tên>)
Cần sửa:
   • <điểm cần sửa>
👉 <vai> – <tên người chính> sửa xong thì báo lại để review lần nữa
⏸️ Các việc đang chờ <MÃ>: <MÃ> (<vai> – <tên>), ...
```

## 7. Quy tắc luôn áp dụng

- **Ranh giới module:** chỉ sửa mã thuộc việc của người dùng. Gặp lỗi ở module người khác thì mở issue và giao cho chủ module, không tự sửa trừ khi chủ module đồng ý.
- **Không đưa lên Git** các thư mục `data/` và `runs/`. Không phát hành lại dữ liệu của tác giả (`sbp_dataset` không có giấy phép); chỉ trích dẫn.
- **Đổi schema** phải tăng phiên bản (ví dụ `graph-v2`) và được cả bên gửi lẫn bên nhận đồng ý.
- **Tập test bị khóa:** chọn ngưỡng và siêu tham số trên validation; chỉ chạy test với cấu hình đã chốt và ghi vào registry.
- **Ghi nguồn:** dùng các nhãn TL (theo bài báo, kèm trang), NTC (nguồn thứ cấp hoặc mã/dữ liệu của tác giả), ĐX (nhóm đề xuất), CXM (cần xác minh). Không mô tả nội dung Fig. 11 hay Table 4 của bài báo, vì nhóm chưa xem được.
- **Không dùng lại** hàm `calculate_metrics()` trong repo BSP_SC của tác giả: nó đếm nhầm FP và FN.
- **Không tự sửa kế hoạch** (`ke-hoach-trien-khai-nhom-3-nguoi.md`, `tom-tat-ke-hoach.md`) trừ khi người dùng yêu cầu. Thay đổi phạm vi thì ghi vào mục 5 của báo cáo.
- **Không thêm lịch hay hạn chót** vào kế hoạch hoặc trạng thái. Nhóm chỉ theo thứ tự phụ thuộc và đợt.
- Nhắc người dùng **tự kiểm tra** phần AI viết trước khi gửi review. Người chính chịu trách nhiệm về sản phẩm của mình.
