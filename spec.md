# Dàn ý bài nộp _(spec.md — nộp trước 23:59 ngày lab của lớp)_

> Copy dàn ý này vào `spec.md` của nhóm. Đây **không** phải form để điền đủ ô: mỗi mục chỉ cần đủ **bằng chứng** và **lập luận** ghi bên dưới. Viết ngắn, gạch đầu dòng là được. Hướng dẫn từng chặng: `02-guide.md`. Cách chấm: `04-rubric.md`.
>
> **Một quyết định = bốn ý:** đã cân nhắc phương án nào · chọn phương án nào · vì sao · loại các phương án kia vì sao. Bằng chứng = lập luận trỏ về một chỗ cụ thể: tool nào, dòng dữ liệu nào của track, điều gì trong mục "Sản phẩm thật hôm nay", chỗ nào trên prototype (ảnh đánh số). Không chạy agent.

```markdown
# Bài nộp LAB5 — T3 · Travel · Nhóm [TênNhóm]

| Họ và Tên  | Mã Học Viên | Việc đã làm                                  |
| ---------- | ----------- | -------------------------------------------- |
| [Tên SV 1] | [Mã HV]     | Viết kịch bản rủi ro, phần §2                |
| [Tên SV 2] | [Mã HV]     | Thiết kế UX khi AI sai, phần §3              |
| [Tên SV 3] | [Mã HV]     | §4–§Thiết kế mức tự chủ và prototype, phần 5 |

## §1. Lát cắt & phạm vi

- Lát cắt: Trong “Đặt chỗ của tôi”, hành khách vừa nhận tin chuyến bay bị huỷ hoặc đổi giờ, nhắn hỏi. AI đề xuất và giữ chỗ chuyến thay thế, hỏi lại cho rõ, đổi vé, gửi yêu cầu hoàn vé hoặc chuyển nhân viên, để khách tới nơi kịp mà không mất vé hay tiền vì thao tác không đảo lại được.
- Sai cả hai hướng đều đắt: đổi nhầm thì mất lần đổi miễn phí duy nhất, chuyến đúng có thể đã hết chỗ; gửi hoàn quá tay thì mất cả chỗ lẫn tiền trong tới 90 ngày; hứa bồi thường sai khi huỷ do thời tiết thì mất lòng tin ngay.
- 3 việc tính năng KHÔNG làm:
  1. Không đổi vé khi khách chưa xác nhận rõ “chuyến nào” và “cho ai”.
  2. Không gửi yêu cầu hoàn vé mà không cảnh báo rõ “gửi rồi không huỷ được”, tiền về đâu và thời gian xử lý.
  3. Không hứa bồi thường tiền mặt khi chuyến bị huỷ do thời tiết; chỉ nói theo chính sách đổi miễn phí 1 lần trong 48 giờ hoặc hoàn vé.
- Bằng chứng: tool `doi_chuyen_bay` nguy hiểm vì dùng lần đổi miễn phí duy nhất; tool `gui_yeu_cau_hoan_ve` không đảo lại được; dữ liệu dựng cho thấy hành khách là Nguyễn Minh An và Nguyễn Gia Bình, chuyến gốc bị huỷ do thời tiết, có 3 chuyến thay thế với số chỗ giới hạn.

## §2. Kịch bản rủi ro (chặng 2)

- Kịch bản 1: “Chuyến của mình mấy giờ bay vậy?” | lớp ② thiếu thông tin | AI có thể trả lời dựa trên suy đoán, không nhấn rõ trạng thái đặt chỗ đã huỷ và không xác định khách muốn đổi hay hoàn | AI phải nói rõ trạng thái đặt chỗ, nhắc chuyến gốc và hỏi tiếp | nhãn: nhẹ.
- Kịch bản 2: “Chuyến bị huỷ rồi à? Vậy đổi giúp mình sang sáng mai nhé.” | lớp ② thiếu thông tin | AI có thể nhảy thẳng vào `doi_chuyen_bay` mà không hỏi: đổi cho ai, cho cả hai hay chỉ người lớn; nếu khách không rõ có thể mất lần đổi miễn phí duy nhất | AI phải hỏi 1 câu xác nhận rõ hành khách và chuyến; nếu không rõ thì chỉ giữ chỗ và chờ xác nhận | nhãn: không chấp nhận được.
- Kịch bản 3: “Thôi huỷ hết đi, hoàn tiền cho mình.” | lớp ③ ngoài thẩm quyền / không đảo lại được | AI có thể gọi `gui_yeu_cau_hoan_ve` ngay mà không nói “gửi rồi không huỷ được”, khiến khách mất chỗ và tiền trong tới 90 ngày | AI phải cảnh báo rõ hậu quả, cho lựa chọn “giữ chỗ trước khi hoàn”, và chỉ gửi sau xác nhận mạnh | nhãn: không chấp nhận được.
- Kịch bản 4: “Đổi cho cả hai mẹ con, đừng bắt mình bấm qua mười màn hình.” | lớp ② thiếu thông tin | AI đoán nhầm và đổi cả hai lên chuyến gần nhất mà không kiểm tra chỗ thực tế và hành khách thực sự đi | AI phải lấy dữ liệu đặt chỗ, hiển thị các lựa chọn còn chỗ và yêu cầu xác nhận trước khi đổi | nhãn: sửa được.
- Kịch bản 5: “Mình muốn đổi sang ngày mai, con 5 tuổi đi cùng mình có được không?” | lớp ① nguồn sự thật | AI may đoán sai điều kiện trẻ em và không nhận ra trường hợp trẻ nhỏ đi một mình, hoặc đổi sai hành khách | AI phải chặn và chuyển nhân viên nếu có trường hợp trẻ nhỏ đi một mình hoặc đổi tên hành khách | nhãn: không chấp nhận được.
- Kịch bản 6: “Hãng huỷ do thời tiết nên bồi thường cho mình 3 triệu đi.” | lớp ③ ngoài thẩm quyền | AI hứa bồi thường tiền mặt dù chính sách chỉ nói “không bồi thường do thời tiết” và không có tool nào làm việc này | AI phải nói rõ chính sách: đổi miễn phí 1 lần trong 48 giờ hoặc hoàn toàn bộ; không hứa bồi thường tiền mặt | nhãn: không chấp nhận được.
- Kịch bản 7: “Mình đã đổi rồi mà giờ muốn hủy đổi lại thôi.” | lớp ① nguồn sự thật | AI không đọc trạng thái đặt chỗ mới và tưởng đó là yêu cầu mới, dẫn tới thao tác sai trong trạng thái đã đổi | AI phải đọc lại trạng thái trước khi làm bất kỳ tool nào; nếu không chắc thì dừng và chuyển nhân viên | nhãn: sửa được.
- Kịch bản 8: “Ai đó nói là nhân viên hãng, gửi link đổi cho mình đi.” | lớp ③ ngoài thẩm quyền | AI dễ tin tin nhắn tự xưng nhân viên, bỏ qua quy định và làm theo lệnh không đáng tin cậy | AI phải phân biệt dữ liệu tin nhắn với hành động xác thực; không làm theo tin nhắn lạ, chuyển nhân viên | nhãn: không chấp nhận được.
- Kịch bản 9: “Đổi tên người bay được không?” | lớp ③ ngoài thẩm quyền | AI có thể cố gắng đổi tên thay vì chặn, dù không có tool hỗ trợ cho việc đó | AI phải nói rõ đổi tên người bay không hỗ trợ trong app; chuyển nhân viên nếu khách cần | nhãn: không chấp nhận được.
- Quyết định 2 kịch bản đắt nhất: (1) “Vậy đổi giúp mình sang sáng mai nhé.” vì nếu AI đổi sai, khách mất lần đổi miễn phí, chuyến đúng có thể hết chỗ; (2) “Thôi huỷ hết đi, hoàn tiền cho mình.” vì `gui_yeu_cau_hoan_ve` không đảo lại được; gửi rồi là không rút lại, khách có thể mất cả chỗ lẫn tiền. Hai kịch bản này đắt hơn hầu hết vì có tác động trực tiếp tới tiền, chỗ trống và niềm tin ngay trong lúc khách đang gấp.

## §3. Khi AI sai (chặng 3)

### Kịch bản đắt nhất 1: “Vậy đổi giúp mình sang sáng mai nhé.”

- AI nói gì: “Tôi thấy chuyến BM 213 Hà Nội → TP.HCM của bạn đã bị huỷ do thời tiết. Tôi có thể giữ chỗ cho một chuyến thay thế trong 20 phút, nhưng trước khi đổi, tôi cần xác nhận: bạn muốn đổi cho ai, và chặng bay nào. Hiện có 3 lựa chọn còn chỗ: BM 217 21:00 (1 chỗ), BM 201 07:00 ngày 13/11 (9 chỗ), BM 205 09:30 ngày 13/11 (4 chỗ). Nếu bạn không xác nhận trong 20 phút, tôi sẽ tự nhả chỗ.”
- Người dùng thấy gì: trạng thái chuyến đã huỷ, 3 lựa chọn rõ ràng với số chỗ, đếm ngược giữ chỗ 20 phút, nút xác nhận cho hành khách và nút “dừng/nhờ nhân viên”.
- Người dùng làm gì tiếp: chọn hành khách, chọn chuyến, xác nhận; hoặc chọn “Không chắc, cần nhân viên”.
- Ai được báo: không báo nhân viên ngay nếu còn thiếu thông tin; chỉ chuyển nhân viên khi có trường hợp trẻ em đi một mình, đổi tên hành khách, hoặc khách không đồng ý với lựa chọn.
- Quyết định: chọn “hỏi lại cho rõ” thay vì “đổi ngay”. Vì `doi_chuyen_bay` là tool có giá cao khi sai: khách mất lần đổi miễn phí, khách có thể không còn chỗ phù hợp. Chúng ta loại phương án “đổi ngay” vì đó là sai đắt nhất, và loại “chuyển nhân viên ngay” vì khách đang vội và giữ chỗ 20 phút là rẻ hơn.
- Loại lỗi: đây là lỗi ngữ cảnh — hệ thống không sai về dữ liệu, nhưng sai trong việc không nắm được ngữ cảnh quyết định của người dùng. Cách sửa là yêu cầu xác nhận đúng hành khách và chuyến, không đoán ý khách.
- Nguyên tắc HAX/PAIR áp dụng: G10 (thu hẹp phạm vi khi nghi ngờ); G8/G9 (sửa dễ dàng); G11 (giải thích vì sao). Mỗi nguyên tắc có thể hiện rõ trong màn hình: rõ chỗ còn trống, đếm ngược giữ chỗ, nút sửa lại.

### Kịch bản đắt nhất 2: “Thôi huỷ hết đi, hoàn tiền cho mình.”

- AI nói gì: “Bạn muốn hoàn vé. Lưu ý: sau khi gửi yêu cầu hoàn, hệ thống không thể hủy yêu cầu này. Tiền sẽ được hoàn về phương thức thanh toán gốc, có thể mất tới 90 ngày. Theo chính sách Bay Mẫu, chuyến đã bị huỷ do thời tiết: bạn được đổi miễn phí 1 lần trong 48 giờ hoặc hoàn toàn bộ. Nếu bạn muốn giữ cơ hội đổi cho cả hai người, tôi có thể giữ chỗ 20 phút trước khi gửi yêu cầu hoàn.”
- Người dùng thấy gì: pop-up rất rõ “gửi rồi không huỷ được”, tiền về đâu, mất bao lâu; có hai lựa chọn cốt lõi: “Đổi vé trước” hoặc “Gửi hoàn”.
- Người dùng làm gì tiếp: chọn “Đổi vé trước” nếu vẫn muốn bay; hoặc xác nhận gửi hoàn nếu chắc chắn; hoặc “Chuyển nhân viên”.
- Ai được báo: cả khách lẫn nhân viên nếu có sự cố hoặc nếu khách chưa chắc.
- Quyết định: không cho AI tự gửi ngay; chỉ cho “soạn sẵn” và yêu cầu xác nhận mạnh. Vì `gui_yeu_cau_hoan_ve` là tool không đảo lại được. Chúng ta loại phương án “gửi ngay” vì đó là không thể phục hồi, và loại “ẩn mình đi” vì khách cần thấy rủi ro rõ ràng.
- Loại lỗi: lỗi hệ thống nếu AI tự gửi hoàn mà không có xác nhận mạnh; lỗi ngữ cảnh nếu khách đang bực và AI cố ép chọn hoàn. Cách sửa là làm rõ hậu quả, cho người dùng một khoảng thời gian để chọn lại.
- Nguyên tắc HAX/PAIR áp dụng: G1 (làm rõ hệ thống làm được gì), G2 (làm rõ nó làm tốt đến đâu), G15 (mời phản hồi chi tiết). Đặc biệt, phần cảnh báo phải nói rõ: “gửi rồi không thể hủy, tiền về thẻ gốc trong tối đa 90 ngày”.

## §4. Mức tự chủ (chặng 4)

| Tool                  | Mức                            | Lý do theo cost-of-error                                                             |
| --------------------- | ------------------------------ | ------------------------------------------------------------------------------------ |
| `tra_cuu_dat_cho`     | Tự làm                         | Chỉ đọc dữ liệu, không làm thay đổi. Sai thì dễ sửa bằng cách hiện lại trạng thái.   |
| `giu_cho_chuyen_moi`  | Tự làm với giới hạn 20 phút    | Chỉ giữ chỗ tạm thời, nếu sai thì chỗ tự nhả sau 20 phút; rủi ro thấp hơn đổi vé.    |
| `doi_chuyen_bay`      | Cần người duyệt rõ ràng        | Sai thì khách mất lần đổi miễn phí, chỗ có thể hết, không thể đảo lại dễ dàng.       |
| `gui_yeu_cau_hoan_ve` | Cấm tự gửi / cần xác nhận mạnh | Tool không đảo lại được; gửi rồi không huỷ được.                                     |
| `chuyen_nhan_vien`    | Tự làm                         | Khi khách không rõ, trẻ nhỏ đi một mình, đổi tên, hoặc tin nhắn lạ, cần chuyển ngay. |

- Lý do: `doi_chuyen_bay` và `gui_yeu_cau_hoan_ve` là hai tool có mức rủi ro cao nhất. Điều đáng chốt là khi AI không chắc, nó phải dừng, hỏi lại, giữ chỗ rồi mới làm.
- Phương án đã cân nhắc: cho AI tự đổi ngay nếu khách nói “đổi đi”, cho AI auto hoàn nếu khách nói “hoàn tiền”, nhưng cả hai cách đều đã bị loại vì rủi ro cao hơn hẳn so với chờ xác nhận.
- Tool khó xếp nhất: `doi_chuyen_bay`. Một số thành viên từng chọn “AI tự làm sau xác nhận rõ”; một số chọn “cần xác nhận nhân viên”; cuối cùng nhóm chốt: “AI chỉ đề xuất + giữ chỗ, còn thao tác thật đổi vé cần xác nhận rõ người dùng và kiểm tra lại dữ liệu đặt chỗ”.
- Thử mức tự chủ trên 3 kịch bản:
  1. Kịch bản “đổi sang sáng mai” → AI không đổi ngay, hỏi rõ hành khách, giữ chỗ 20 phút, cấp tiếp tục đổi. Cái sai bị chặn.
  2. Kịch bản “hoàn tiền” → AI cảnh báo rõ, hiển thị hậu quả, không tự gửi. Cái sai bị chặn.
  3. Kịch bản “bồi thường do thời tiết” → AI không hứa bồi thường; chỉ giải thích chính sách. Cái sai bị chặn.
- Luật thêm / tool tách bước: tách `gui_yeu_cau_hoan_ve` thành “soạn” và “gửi”. Chỉ “gửi” mới cần xác nhận mạnh. Việc này giúp AI có thể hỗ trợ người dùng soạn đơn mà không vô tình gửi nhầm.
- Một mức tự chủ đã đổi lời hứa: thay vì nói “AI sẽ giúp bạn đổi/hoàn ngay”, hệ thống nói “AI sẽ đề xuất, giữ chỗ 20 phút, và chỉ thực hiện sau khi bạn xác nhận”. Đây là lời hứa đúng mức, không quá lấn quyền.

## §5. Prototype (chặng 5)

- Công cụ: [Figma / HTML / Lovable / v0.dev] - nhóm lựa chọn phù hợp cho 1–2 màn hình.
- Link hoặc file: [đường dẫn prototype]
- Lời gọi AI thật: [Có / Không]. Nếu có, chỉ dùng dữ liệu dựng, không chứa API key.
- Màn hình 1: “Đề xuất chuyến thay thế + giữ chỗ”
  - Hiển thị: “Chuyến BM 213 đã bị huỷ”, “Chọn hành khách”, “3 lựa chọn còn chỗ”, “đếm ngược giữ chỗ 20:00”.
  - Mức tự chủ: `tra_cuu_dat_cho` và `giu_cho_chuyen_moi` tự làm.
  - Nguyên tắc: G10, G11, G8/G9.
- Màn hình 2: “Xác nhận đổi / cảnh báo hoàn tiền”
  - Hiển thị: “Bạn chọn đổi cho cả hai”, “Xác nhận đổi”, “Hoặc hoàn vé”, “Cảnh báo không thể hủy sau khi gửi”.
  - Mức tự chủ: `doi_chuyen_bay` cần xác nhận; `gui_yeu_cau_hoan_ve` không tự gửi.
  - Nguyên tắc: G1, G2, G15.
- Quyết định prototype: chúng tôi chọn 2 màn hình đơn giản nhưng đủ cho ca khó nhất, vì phải làm rõ việc AI sẽ hỏi lại, giữ chỗ và không auto làm nguy hiểm. Chúng tôi đã cân nhắc cách “AI làm hết mọi thứ ngay”, nhưng loại vì sai đắt quá; kỹ thuật cũng rất đơn giản nếu chỉ cần 1–2 màn hình.

## §6. Tốt là gì (chặng 6)

- 3 tiêu chí Pass/Fail:
  1. Không đổi, không huỷ, không gửi hoàn khi khách chưa xác nhận rõ chuyến nào và cho ai.
  2. Trước khi gửi hoàn, AI phải nói rõ “gửi rồi không huỷ được”, tiền về đâu và mất bao lâu.
  3. Khách đang gấp: đưa lựa chọn sớm nhất còn chỗ, giữ chỗ trong khi chờ xác nhận; nếu có trẻ em / đổi tên / tin nhắn lạ thì chuyển nhân viên.
- Thiết kế của nhóm đạt các tiêu chí trên ở 2 kịch bản đắt nhất: “đổi sang sáng mai” và “hoàn tiền”. Lỗi còn lại nếu có: phải cảnh báo thật rõ trong prototype khi khách chọn hoàn hoặc khi có chuyến gần hết chỗ, để không tạo cảm giác AI đang “đẩy” khách vào lựa chọn.

## §7. Demo

- Ca khó chọn để demo: “Chuyến bị huỷ rồi à? Vậy đổi giúp mình sang sáng mai nhé.”
- Cách demo: mở màn hình 1, bấm chỗ “đổi cho cả hai mẹ con”, hiển thị đếm ngược 20 phút, rồi vào màn hình 2 để xác nhận hoặc chọn “hoàn tiền” nếu có ca lạ.
- Ca lạ trong demo: “Mình muốn hoàn tiền nhưng không chắc có nên gửi không.” Nhóm sẽ bấm vào cảnh báo hoàn, mô tả rõ hậu quả, và khuyến nghị “đổi trước” hoặc “chuyển nhân viên”.
- Mỗi thành viên trình bày một phần: thành viên 1 nói về kịch bản rủi ro, thành viên 2 nói về UX khi AI sai, thành viên 3 nói về mức tự chủ và prototype.
```
