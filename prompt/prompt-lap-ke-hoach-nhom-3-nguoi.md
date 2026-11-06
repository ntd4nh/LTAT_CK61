# Prompt lập kế hoạch triển khai đồ án cho nhóm 3 người

Bạn là người hướng dẫn đồ án và trưởng nhóm kỹ thuật có kinh nghiệm về phân tích mã nguồn Solidity, học máy và tái hiện nghiên cứu. Hãy đọc repository hiện tại, sau đó lập một kế hoạch triển khai cụ thể, khả thi và có phân công cân bằng cho nhóm **đúng 3 thành viên: Thành viên A, Thành viên B, Thành viên C**.

Tôi cần biết rõ **nhóm phải làm những gì, từng người làm gì, tạo ra sản phẩm nào, bàn giao cho ai và khi nào một công việc được coi là hoàn thành**. Chỉ lập kế hoạch; chưa viết mã triển khai hay chạy thí nghiệm.

## 1. Đọc và đối chiếu tài liệu trước khi lập kế hoạch

Kiểm tra cấu trúc repository và đọc các tài liệu sau, với đường dẫn tính từ thư mục gốc repository:

- `docs/Project.md`: tiêu chí chấm điểm và yêu cầu đồ án.
- `docs/Fine-grained smart contract vulnerability detection by heterogeneous code feature learning and automated dataset construction - Science.pdf`: tài liệu bài báo; phải mở xem trực tiếp các trang PDF để kiểm tra cả hình, sơ đồ, bảng và chú thích nếu có.
- `docs/Fine-grained smart contract vulnerability detection by heterogeneous code feature learning and automated dataset construction - Science.md`: bản chuyển nội dung chữ, dùng hỗ trợ tra cứu; không dùng thay cho việc xem hình trong PDF.
- `docs/tong-quan-de-tai-smart-contract-vulnerability-detection.md`: tài liệu tổng quan; phân biệt phần tóm tắt bài báo với hướng triển khai do người viết đề xuất.

Tại thời điểm soạn prompt, repository chỉ có tài liệu, chưa có mã nguồn triển khai, dữ liệu hoặc mô hình. Hãy kiểm tra lại trạng thái hiện tại trước khi kết luận phần nào đã có sẵn.

**Giới hạn nguồn cần lưu ý:** PDF hiện có là bản in trang ScienceDirect dài 8 trang, gồm tóm tắt, giới thiệu và trích đoạn các mục, chưa phải toàn văn. Trang 5 nhắc đến Fig. 11 và Table 4 nhưng không chứa chính hình và bảng đó. Không tự mô tả nội dung hình chưa nhìn thấy, không suy đoán danh mục hình, chi tiết kiến trúc hoặc cấu hình thí nghiệm rồi trình bày như thông tin đã xác nhận.

Nếu có công cụ truy cập Internet, kiểm tra nguồn chính thức của bài báo/tác giả để xác minh khả năng lấy toàn văn, mã nguồn và dữ liệu công khai. Dẫn nguồn cho kết luận. Nếu chưa xác minh được, ghi rõ “chưa xác minh”; không đồng nhất “chưa tìm thấy” với “không có mã nguồn”. Vẫn lập kế hoạch với giả định minh bạch và chỉ rõ điểm cần cập nhật khi có tài liệu bổ sung.

Với các hình/bảng thực sự đọc được, chỉ ra chúng làm rõ thành phần nào trong kế hoạch, kèm số hình/bảng và trang. Nếu không có, nêu ngắn gọn phần thông tin còn thiếu. Khi nguồn mâu thuẫn, ghi rõ mâu thuẫn và ưu tiên tài liệu gốc cho mô tả phương pháp; ưu tiên `Project.md` cho yêu cầu đồ án.

## 2. Bối cảnh kỹ thuật và mục tiêu đồ án

Đề tài: **Fine-grained smart contract vulnerability detection by heterogeneous code feature learning and automated dataset construction**.

Các ý chính đã xuất hiện trong tài liệu:

- Phát hiện lỗ hổng Solidity ở **mức câu lệnh**, được mô hình hóa thành bài toán phân loại nút; một câu lệnh có thể trải trên nhiều dòng.
- Xây dựng dữ liệu dựa trên **Security Best Practices (SBP)**: nhận diện cơ chế bảo vệ, loại bỏ cơ chế đó và gán nhãn các câu lệnh liên quan theo quy tắc được kiểm chứng.
- Biểu diễn từng hàm bằng đồ thị mã nguồn, trong đó nút chứa cây AST con của câu lệnh và cạnh thể hiện quan hệ giữa các câu lệnh, chẳng hạn luồng điều khiển CFG.
- Mô hình **SS-GNN** kết hợp **child-sum Tree-LSTM** để học cấu trúc cú pháp với **GNN** để học quan hệ giữa các nút.
- Đầu ra demo cần ánh xạ dự đoán về câu lệnh/vị trí trong mã Solidity.

Không mặc định nhãn `neutral` là mã đã được chứng minh an toàn. Không mặc định loại bỏ SBP luôn tạo ra lỗ hổng có thể khai thác. Kế hoạch phải có bước kiểm tra chất lượng nhãn và phân tích hạn chế này.

Các chỉ số Precision 91,40% và Recall 89,77% được trích trong tài liệu thuộc thí nghiệm reentrancy **ở cấp hợp đồng**, không phải chỉ số định vị câu lệnh. Không dùng chúng làm kết quả dự kiến hay ngưỡng bắt buộc của nhóm.

Bám sát tiêu chí trong `Project.md`:

- Hiểu bài toán và thuyết trình: 1 điểm.
- Nắm bắt, giải thích phương pháp và kỹ thuật: 3 điểm.
- Hiện thực/demo hoặc tái hiện thí nghiệm, kèm phân tích ưu nhược điểm: 5 điểm.
- Phần xuất sắc/mở rộng: 1 điểm.

Tùy trạng thái mã nguồn của tác giả đã xác minh, chọn hướng chính phù hợp với tiêu chí: tái hiện thí nghiệm nếu có mã nguồn công khai; hiện thực các giai đoạn/kỹ thuật chính nếu không có. Nếu còn chưa rõ, nêu kế hoạch cơ sở và các điều chỉnh có điều kiện, tránh viết hai kế hoạch đầy đủ trùng lặp. Nếu phải rút gọn phạm vi so với rubric, chỉ rõ phần chưa đáp ứng.

## 3. Ràng buộc bắt buộc

- **Không tạo timeline, lịch, ngày tháng, deadline, tuần, sprint hoặc ước lượng số giờ/ngày.** Chỉ thể hiện thứ tự thực hiện, quan hệ phụ thuộc và điều kiện chuyển bước.
- Giả định 3 người có năng lực ban đầu và mức đóng góp tương đương vì chưa có thông tin riêng. Không tự giả định một người là chuyên gia hoặc có nhiều nguồn lực hơn.
- Chia đều theo **công sức, độ khó kỹ thuật, rủi ro, việc tích hợp và kiểm chứng**, không chỉ theo số lượng đầu việc.
- Mỗi người phải có phần nghiên cứu, phần hiện thực kỹ thuật có ý nghĩa, phần kiểm thử/đánh giá và phần báo cáo/thuyết trình gắn với sản phẩm mình phụ trách.
- Không phân công một người làm toàn bộ mô hình còn người khác chỉ làm tài liệu hoặc giao diện. Tránh gom toàn bộ tích hợp và sửa lỗi cuối cùng cho một người.
- Mỗi đầu việc có **một người chịu trách nhiệm chính** và một người review khác. Nếu cần phối hợp, tách rõ phần việc và sản phẩm của từng người, không ghi chung chung “cả nhóm cùng làm”.
- Không ghi những việc mơ hồ như “tìm hiểu GNN”, “xử lý dữ liệu”, “viết báo cáo” mà thiếu phạm vi, sản phẩm và tiêu chí hoàn thành.
- Chọn một phạm vi cốt lõi vừa sức nhóm 3 người. Có thể đề xuất reentrancy làm điểm bắt đầu, nhưng phải ghi đây là lựa chọn triển khai của nhóm nếu chưa xác minh toàn bộ phạm vi bài báo.
- Tách rõ **phần cốt lõi bắt buộc** và **phần mở rộng tùy chọn**. Không lấy việc mở rộng không bắt buộc để bù khối lượng cho một thành viên đang được giao quá ít việc cốt lõi.
- Không hứa tái hiện chính xác bài báo khi còn thiếu toàn văn, mã nguồn, dữ liệu hoặc cấu hình. Phân biệt rõ “theo tài liệu”, “đề xuất của nhóm” và “cần xác minh”.

## 4. Nội dung kế hoạch phải bao phủ

Hãy chuyển các nhóm nội dung dưới đây thành các công việc đủ nhỏ để giao việc, nhưng không vụn đến mức liệt kê từng dòng mã:

1. **Chốt phạm vi và cách tái hiện:** lỗ hổng mục tiêu, nguồn tham khảo, phần bám theo bài báo, phần cần tự thiết kế, nguồn lực tính toán và phương án khi thiếu dữ liệu/toàn văn/mã nguồn.
2. **Dữ liệu và nhãn:** nguồn hợp đồng, SBP áp dụng, điều kiện nhận diện và biến đổi, nhãn câu lệnh, nguồn gốc mẫu, kiểm tra biên dịch, kiểm tra thủ công một phần dữ liệu, thống kê và lọc trùng.
3. **Chia dữ liệu:** train/validation/test; giữ hợp đồng gốc và các biến thể hoặc mẫu gần trùng trong cùng một nhóm chia để hạn chế rò rỉ; không học từ vựng hoặc chọn tham số dựa trên tập test.
4. **Biểu diễn mã:** chọn parser/compiler phù hợp, trích AST, dựng CFG/đồ thị, định nghĩa nút và cạnh, ánh xạ AST-câu lệnh-vị trí mã, xử lý trường hợp không hỗ trợ và định dạng dữ liệu trung gian.
5. **Mô hình:** baseline khả thi, bộ mã hóa cây, phần GNN và đầu phân loại nút, hàm mất mát, batching, huấn luyện, lưu/nạp checkpoint và dự đoán. Làm rõ thành phần nào có thể tái sử dụng và thành phần nào phải tự hiện thực.
6. **Đánh giá:** Precision/Recall/F1 ở mức câu lệnh; nếu có cấp hợp đồng thì báo cáo riêng và định nghĩa cách tổng hợp. Có baseline, thí nghiệm loại bỏ/thay thành phần để đánh giá vai trò học cú pháp khi khả thi, phân tích cảnh báo nhầm/bỏ sót và chất lượng nhãn. Không chỉ báo cáo accuracy trên dữ liệu lệch lớp.
7. **Tích hợp và demo:** từ tệp `.sol` đến danh sách/đánh dấu câu lệnh nghi ngờ; quy định đầu vào, đầu ra, trường hợp lỗi và mẫu demo. Chọn CLI hoặc giao diện đơn giản đủ chứng minh kết quả, kèm hướng dẫn chạy lại.
8. **Báo cáo và thuyết trình:** giải thích bài toán/phương pháp, sơ đồ do nhóm dựng và ghi nguồn, kết quả thực nghiệm, phân tích hạn chế, phần việc của từng người và minh chứng theo rubric.

Đề xuất công nghệ kèm lý do ngắn gọn, đủ để thống nhất triển khai; không biến kế hoạch thành danh sách công cụ. Không coi lựa chọn thư viện của nhóm là công nghệ tác giả đã sử dụng nếu chưa có nguồn xác nhận.

## 5. Cấu trúc đầu ra mong muốn

Viết kế hoạch bằng tiếng Việt, rõ ràng và có thể dùng trực tiếp để phân công. Trình bày theo cấu trúc sau:

### A. Hiện trạng, căn cứ và phạm vi

Tóm tắt repository, tài liệu đã đọc, thông tin đã xác nhận/còn thiếu; nêu mục tiêu đầu vào-đầu ra, phạm vi cốt lõi, phần ngoài phạm vi và giả định cần thiết. Chọn một hướng triển khai chính và giải thích ngắn gọn.

### B. Luồng triển khai và các điểm bàn giao

Mô tả các chặng theo quan hệ phụ thuộc, không gắn thời gian. Mỗi chặng có mục tiêu, điều kiện bắt đầu, sản phẩm và điều kiện hoàn thành. Chỉ ra công việc có thể làm song song và chỗ dùng mẫu dữ liệu nhỏ/mock theo schema để người khác không phải chờ toàn bộ pipeline.

### C. Bảng công việc chi tiết

Mỗi công việc có mã riêng. Dùng bảng với các cột:

| Mã việc | Công việc và phạm vi cụ thể | Người chính | Người review | Phụ thuộc/đầu vào | Sản phẩm bàn giao | Tiêu chí hoàn thành | Điểm công sức |
| --- | --- | --- | --- | --- | --- | --- | --- |

Nếu bảng dài, chia theo chặng nhưng giữ mã công việc duy nhất. Chỉ đánh dấu phần bắt buộc/mở rộng bằng nhãn rõ ràng, không lẫn lộn.

Tiêu chí hoàn thành phải kiểm tra được, ví dụ: schema có mẫu hợp lệ; ánh xạ nút về đúng đoạn mã trên các tình huống đã chọn; lưu/nạp mô hình cho dự đoán nhất quán; lệnh đánh giá sinh bảng chỉ số từ tập test cố định. Không dùng “đạt độ chính xác cao” làm tiêu chí không có căn cứ.

### D. Phân công và kiểm tra cân bằng cho 3 thành viên

Tổng hợp riêng cho A, B, C: mã việc sở hữu, phần review, sản phẩm kỹ thuật, phần đánh giá, phần báo cáo/thuyết trình và các bàn giao nhận/gửi.

Dùng thang **điểm công sức tương đối 1, 2, 3, 5, 8**, không quy đổi ra thời gian. Giải thích ngắn cách ước lượng dựa trên độ phức tạp, độ bất định và trách nhiệm kiểm chứng. Tính cả công review, phối hợp, tích hợp và tài liệu; có thể tách thành đầu việc riêng, nhưng không tính trùng.

Lập bảng tổng điểm và tỷ trọng của mỗi người. Mục tiêu mỗi người khoảng **30-36% khối lượng cốt lõi**, đồng thời có độ khó kỹ thuật tương đương. Nếu lệch, điều chỉnh thực chất cách chia module/đầu việc trước khi chốt, không chỉ sửa điểm cho đẹp. Tính riêng phần mở rộng để chứng minh kế hoạch cốt lõi vẫn chia đều.

### E. Quy ước tích hợp và kiểm chứng

Đề xuất cấu trúc thư mục/module, schema tối thiểu giữa dữ liệu - đồ thị - mô hình - dự đoán, người cung cấp/người dùng từng đầu ra và cách review chéo. Nêu các kiểm tra quan trọng cho dữ liệu, ánh xạ vị trí, mô hình và luồng demo; cách lưu cấu hình, seed, checkpoint và kết quả để chạy lại.

### F. Sản phẩm cuối, rủi ro và checklist nghiệm thu

Liệt kê sản phẩm nộp, đối chiếu chúng với tiêu chí 1-3-5-1 điểm. Với từng rủi ro chính, chỉ rõ dấu hiệu nhận biết, người xử lý và phương án thay thế khả thi. Ghi rõ giới hạn của phương án thay thế so với mục tiêu tái hiện ban đầu.

Kết thúc bằng checklist cho từng người và các việc có thể bắt đầu ngay, tham chiếu mã công việc đã có. Không thêm timeline hay hạn hoàn thành.

## 6. Tự kiểm tra trước khi trả kết quả

Đảm bảo kế hoạch không có công việc thiếu chủ sở hữu, không có phụ thuộc vòng, không có đầu việc tích hợp/báo cáo bị bỏ sót; mọi người đều có đóng góp kỹ thuật và phần trình bày riêng; tổng điểm khớp bảng chi tiết; mức chia đều đạt được ngay trong phần cốt lõi.

Khi thiếu thông tin không thiết yếu, tự chọn giả định hợp lý và nêu rõ để tiếp tục lập kế hoạch. Chỉ hỏi bổ sung khi thông tin đó thực sự làm thay đổi căn bản tính khả thi; không dừng chỉ vì chưa biết tên thành viên hoặc chưa có toàn văn.

**Đầu ra cần là một bản kế hoạch triển khai hoàn chỉnh để nhóm dùng phân việc, không phải bài tóm tắt lại nghiên cứu và không phải một prompt khác.**
