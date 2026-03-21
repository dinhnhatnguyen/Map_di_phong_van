## Context

- Hiện tại cty a tui là làm về xây dựng nhưng mà thích ứng dụng công nghệ vào đấy để tối ưu sản xuất
- Mà h kiếm người biết về xây dựng mà chịu code thì k có, ngược lại IT mà nhảy qua làm mấy này thì k có kiến thức về xây dựng
- a tui là dân xây dựng nhưng lại thích code nên h ổng đang gánh về mảng code tool cho cái xây dựng này,
- Mà ổng chỉ biết code mấy tool nhẹ nhàn làm việc với autocad kiểu như plugin thôi còn sâu quá về kiểu trích xuất data các kiểu thì ổng chịu
- Nên thường mấy này thì ống thường k nhận nếu sếp nhờ ác quá thì ổng nhờ người ngoài cụ thể là tui giúp ổng mấy vụ này
- Mà lâu dài kiểu này thì k ổn lắm tại tui thì mất thời gian còn ổng nhờ hoài cũng kì mà thuê người thì họ k thuê,
- Với lại cty nhật có kiểu là nó ưu tiên người cty làm, làm lâu cũng được hạn chế thuê ngoài để nó độc quyền được mấy cái sản phẩm đấy tránh bị bán tùm lum giảm sức cạnh tranh của nó
- Nên h nó muốn tuyển một người lâu dài chịu theo xây dựng để code tool tối ưu sản xuất các thứ, mấy tool này mang đi thi bên nhật nữa á
- Giờ cty ni nó có cái vấn đề chính là :
  - Đầu tiên là nó muốn kiểu nó có một đống chi tiết cần cắt cnc giờ tb làm sao đó để nghiên cứu ra cái tool để xếp mấy cái chi tiết đó lên tấm thép cho tối ưu diện tích nhất có thể, cùng 1 tấm thép mà xếp được nhiều chi tiết cắt cnc nhất có thể để tối ưu được diện tích hữu dụng tránh mấy cái khoản trống thừa k dùng được làm lãng phí (mà cái tool này tự động nghen ví dụ họ vứt vào một đống chi tiết cần cnc => tool xử lý => ra một cái bản bố trí chi tiết trên tấm thép tối ưu)
  - Thứ 2 là xử lý trích xuất dữ liệu bản vẽ tự động => bản vẽ khách hàng nó gửi cũng dầm cột các kiểu mà cách bố trí thông số mỗi khách nó lại mỗi khác giờ họ muốn làm như nào đó để build model hay dựng tool gì đó để nó tự động detect đúng dữ liệu họ cần lấy cho dù input nó định dạng như nào (này là best practice, còn đợt tui làm giúp a tui là mỗi loại bản vẽ tui code 1 tool thực ra thì có vài file python à :v)
- Tb vào thì chủ yếu lo 2 vấn đề trên còn việc code c++ với c# để làm với mấy tool autocad thì mảng này a tui làm hoặc sau này từ từ học thoải mái

## Pros

- H mà tb gật đầu cái thì 80% là vào làm luôn tại a tui là người tuyển trực tiếp với tb sau sẽ làm việc trực tiếp với a tui
- Ổn định thì vcl luôn vô đây thì 5-10 năm tới tb đếch bao h sợ thất nghiệp các thứ
- Tb vô đây đúng kiểu học thuật nghiên cứu tool đề xuất mấy sếp (Mấy cha ni già k biết chi về tech hết chỉ đưa input rồi output là chi các kiểu) tb đăng ký học chi tốn chừng mô tiền cần dùng tool chi để build hoặc để học các thứ cty refund cho hết
- Tự do thì vcl, tại cty này chỉ chuyên về kiến trúc k biết gì về tech cả nên tb vào đây thì xem như 1 mình 1 mảng tb tự do nghiên cứu làm chi cũng được dùng bất cứ tool chi k có vụ giới hạn quyền các thứ các thứ strict như mấy cty IT thuần
- Tui nghĩ là cái này đúng theo thế mạnh của tb là train model với xử lý hình ảnh (tại file pdf thường thì tui làm thì cũng phải chuyển thành ảnh rồi mới bắt đầu phân tích)

## Cons

- Đầu tiên thì họ cần ổn định tb làm ở đây thì phải theo được ít nhất tầm 4-5 năm k nhảy việc thì họ mới dám đầu tư tại đây là thị trường nghách tuyển người rất khó luôn
- Thứ 2 là do cty k có nền tech nên tb sẽ tự làm tất cả k có mentor về IT chỉ có mấy a chỉ về mấy cái nghiệp vụ bên xây dựng đồ thôi
- Vô đây thì chỉ có bạn với a tui gánh mảng IT của cty ni
- Quy trình coding rồi release các kiểu thì tb tự túc thôi chứ nó k chuyên nghiệp như IT các kiểu mà nếu tb theo thì mấy này tui chỉ tb được vào tui sẽ hướng dẫn tb nên tổ chức dự án như nào dựng CICD như nào, git workflow như nào, ... cái này tui chỉ được hết tại tui kênh nhiều mảng r mấy này tui lo được
