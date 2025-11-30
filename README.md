# Mập đi phỏng vấn :3

&nbsp;&nbsp;&nbsp;&nbsp;Đi phỏng vấn luôn là nỗi lo của hầu hết các bạn mới ra trường hoặc mới chuyển ngành, nhất là khi chưa có nhiều kinh nghiệm thực chiến. Trước đây mập cũng rứa — hồi hộp, thiếu tự tin, sợ bị hỏi khó, sợ nói sai, sợ bị đánh giá… Nhưng sau khi trải qua kha khá buổi phỏng vấn từ nhỏ tới lớn, thì Mập rút ra một điều quan trọng: phỏng vấn không phải cuộc “thi đấu sinh tử”, mà là một quá trình học hỏi và lựa chọn lẫn nhau.

&nbsp;&nbsp;&nbsp;&nbsp;Bài viết này là tổng hợp những kinh nghiệm mà Mập rút ra trong quá trình học – làm – tạch – đậu, hy vọng giúp được bạn mô đó đang chuẩn bị tìm việc, (đặc biệt là fresher backend/spring boot như Mập). Nội dung chia sẻ từ trải nghiệm thực tế không màu mè (nếu k muốn nói là cùi cùi :v ). Ai đang loay hoay hoặc mất tự tin thì hy vọng đọc xong sẽ chắc cũng thấy nhẹ đầu hơn (hoặc không :3), biết mình cần chuẩn bị gì và bước vào phỏng vấn với tâm thế chủ động hơn.

## Kiến thức phải có

### OOP

    - Thuộc lòng 4 tính chất của oop (đọc khái niệm xong cho ví dụ )
    - Phân biệt được overide và overload
    - Interface vs abstrac class ( cái ni hên xui hỏi thôi mà thường là khôn)
    - Coupling và Cohesion ( cái này cũng hay hỏi)
    - SOLID principles ( Học thuộc lòng khái niệm hiểu chém mượt cái này là điểm cộng lớn luôn )

### Design Patterns ( Quan trọng chắc chắn sẽ hỏi )

Không cần nhớ hết các mẫu, mỗi loại patterns nắm 1-2 cái là đủ rồi, khi trả lời thì đọc tên trước rồi sau đó trình bày mẫu đó làm chi (kèm theo được giải thích khái niệm thì tốt không thì thôi)

[Tự search hoặc học từ đây cũng được hí](https://refactoring.guru/design-patterns)

#### Creational Design Patterns

    - Singleton ( cái này có thể cho ví dụ kết nối db các thứ)
    - Builder (Đối với mấy bạn code java thì có thể lấy @Builder của lombok làm ví dụ)

#### Structural Design Patterns

    - Adapter (Học mẫu này kĩ kĩ chút, tại nó hay gặp với lại họ rất thích hỏi về mẫu này)
    - Facade

#### Behavioral Design Patterns

    - Observer
    - Strategy

#### Note: Đối với mấy bạn dùng springboot á thì chắc chắn phải nắm vững mẫu dependency injection (Khi phỏng vấn về springboot mà nói được cái này thì điểm cộng rất lớn)

### SQL

    - Các câu lệnh cơ bản: SELECT, INSERT, UPDATE, DELETE
    - Thứ tự thực hiện các câu lệnh trong SQL
    - Phân biệt các loại JOIN
    - Chuẩn hóa CSDL (Normal forms) -> Nắm sơ, có thể sẽ hỏi
    - Chỉ mục (Indexes) và tạo chỉ mục -> Học kỹ vào chắc chắn sẽ nỏi
    - Hiểu biết về transaction và các cấp độ cộng đồng (ACID properties) -> Học kỹ vào chắc chắn sẽ hỏi
    - Hiểu biết về quan hệ giữa các bảng (One-to-One, One-to-Many, Many-to-Many)
    - Làm sao để tối ưu tốc độ của một câu truy vấn ( tí bỏ xuống câu hỏi thường gặp)

### Backend

- REST Principles
- Các HTTP Method (chú ý vào PUT với POST á kiểu gì họ cũng hỏi bẫy cái ni) -> Học kỹ vào
- HTTP Status code
- Stateless vs Statefull ( Chủ yếu xoáy vào JWT với Session á)

## Kiến thức nên có

#### 1. Tiếng anh ( Có Toeic 700+ thì ngon đỡ phải test nhiều còn k thì chịu khó làm test xíu chủ yếu giao tiếp được là oke)

#### 2. Microservice vs Monolithic

    - Microservice là gì
    - Monolithic là gì
    - Phân biệt được kiến trúc Microservice với Monolithic
    - Đặc điểm của Microservice với Monolithic
    - Khi nào nên dùng Microservice, khi nào nên dùng Monolithic
    - Điểm yếu của kiến trúc Microservice với Monolithic là gì

#### 3. Caching

    - Tìm hiểu về Redis
    - Cách sử dụng Redis trong ứng dụng backend
    - Các chiến thuật caching

#### 4. Message queue

    - Message queue dùng để làm gì áp dụng cho trường hợp nào áp dụng nó thì giải quyết được vấn đề gì
    - Tìm hiểu về RabbitMQ hoặc Kafka (Nên bắt đầu từ kafka)

#### 5. DSA ( data structures and algorithms )

- Nắm rõ đặc điểm của các cấu trúc dữ liệu
- Luyện [Leetcode](https://iamtuna.org/2024-12-08/Leetcode-starter-kit#dsa-resources) ( Tầm 150 bài thì bỏ CV cũng ngon luôn)

#### 6. Kiến thức về cloud computing ( cái này nâng cao có được thì quá ngon còn k thì từ từ học cũng được)

## Cần chuẩn bị

- CV
- Làm Profile trên Linkedin (Kiếm việc được ở trên này luôn)
- Lời giới thiệu ( Cả tiếng anh với tiếng việt hi)
- Kiến thức về project của mình ( chú trọng vô cái project khóa luận hay cái nào mà tự tin nhất á)
- Được thì nghiên cứu thêm về Cover letter với làm thêm một trang Portfolio thì ngon

#### Lưu ý : Dự án bỏ trong CV thì nên host hoặc làm cách nào đó mà có thể xem được demo luôn (vì HR đôi khi không có thời gian vào soi code như nào đâu)

## Câu hỏi thường gặp:

### Cơ bản:

    - Giới thiệu về bản thân
    - Điểm mạnh điểm yếu
    - Vì sao chọn công việc này ( Hoặc em nghĩ vì sao công việc này phù hợp với bản thân)
    - Em biết gì về công ty

### Về Technical

    - 4 thuộc tính OOP
    - Em biết những mẫu design pattern nào
    - Em Biết gì về REST API
    - Làm sao để tối ưu tốc độ truy vấn của câu lệnh SQL (Khi hỏi câu này thì họ muốn nghe nói về INDEX )
    - Chức năng nào của SQL đảm bảo về tính toàn vẹn và nhất quán của dữ liệu trong db ( Khi hỏi câu này là họ muốn nói về Transaction)
    - Điểm yếu chí mạng của Microservice/Monolithic là gì
    - Phân biệt PUT với POST(Câu này hay bẫy, Về cơ bản 2 cái này hoạt động giống nhau nma đặt khác tên để phân biệt được chức năng thôi)
    -...

## Kinh nghiệm khi đi phỏng vấn của mập

&nbsp;&nbsp;&nbsp;&nbsp; Khi rải CV thì nên rải từ trên xuống, Nhắm mấy công ty top tập đoàn lớn đầu tiên xong rồi hạ thấo mục tiêu từ từ sẽ tối ưu được cơ hội hơn ( mặc dù mất thời gian tìm việc hơn , còn nếu cần gấp một công việc thì cứ đại đại hoi :3)

### 1. Phỏng vấn là cơ hội không phải thử thách

- Phỏng vấn là cơ hội thực tế nhất để rà soát lại kiến thức của bản thân. Từ những câu hỏi phỏng vấn có thể nhận ra mình đang mạnh ở đâu, còn thiếu gì, phần nào cần củng cố, từ đó có lộ trình học tập rõ ràng hơn thay vì học lan man.

- Phỏng vấn là cơ hội trực tiếp để biết về nhu cầu thực tế của thị trường. Những yêu cầu mà nhà tuyển dụng đặt ra chính là điều mà các công ty đang tìm kiếm. Từ đó điều chỉnh hướng phát triển kỹ năng để phù hợp với xu hướng tuyển dụng bựa ni.

- Mỗi cuộc phỏng vấn là một bài học kinh nghiệm quý giá. Không những rèn khả năng giao tiếp, tư duy và ứng biến mà còn có thể học hỏi trực tiếp từ các anh chị phỏng vấn. Nếu gặp kiến thức chưa biết thì cứ mạnh dạng nói “Phần này em chưa biết rõ, anh/chị có thể giải thích thêm giúp em được không ạ?”. Đừng sợ họ đánh giá là thiếu kiến thức mà làm như này còn thể hiện thái độ cầu tiến sẵn sàng học hỏi các thứ cái ni họ cũng đánh giá cao á ( họ k thích kiểu fake CV với giấu dốt)

- Phỏng vấn là cơ hội để luyện tinh thần với sự tự tin. Qua nhiều lần trải nghiệm, sẽ bớt căng thẳng, biết cách trình bày vấn đề logic hơn và hiểu rõ với đoán được ý của họ và trả theo cách mà doanh nghiệp muốn nghe.

- Ngay cả khi không đậu, vẫn thu được kinh nghiệm thực chiến. Mỗi lần phỏng vấn đều cung cấp phản hồi, rút kinh nghiệm để cải thiện bản thân, giống như một lần “tập dượt” để chuẩn bị tốt hơn cho cơ hội phù hợp hơn sau này.

## 2. Tìm việc chứ không phải đi xin việc

&nbsp;&nbsp;&nbsp;&nbsp;Về bản chất, mối quan hệ giữa ứng viên với bên tuyển dụng là mối quan hệ hai chiều, cả hai đều tìm kiếm lợi ích phù hợp từ đối phương. Công ty cần người có khả năng giải quyết vấn đề, còn ứng viên cần môi trường phù hợp để phát triển — nếu “hợp” thì hợp tác, nếu không thì thôi. Điều này giống như một cuộc đàm phán win–win chứ không phải quan hệ xin–cho.

&nbsp;&nbsp;&nbsp;&nbsp;Vì vậy, khi đi phỏng vấn hoặc nộp đơn vào bất kỳ công ty nào, đừng mang tâm lý “đi xin việc”. Việc tự hạ thấp giá trị bản thân làm mình thiếu tự tin, dễ bị động và đánh mất đi chính điều mà doanh nghiệp đang tìm kiếm: một người có năng lực và biết rõ giá trị của mình. Nhớ là mình cũng đang “chọn” công ty, không chỉ công ty “chọn” mình.

## 3. Sau khi phỏng vấn cần làm gì

- Note lại toàn bộ câu hỏi đã được hỏi trong buổi phỏng vấn. Đây là cách tốt nhất để đánh giá xem mình đang thiếu kiến thức ở đâu, câu trả lời nào còn lúng túng, phần nào cần cải thiện để chuẩn bị kỹ hơn cho lần phỏng vấn lần sau.

- Tự đánh giá lại phong thái phỏng vấn. Xem thử mình trả lời có rõ ràng chưa, có bị nói nhanh/nói dài, có trình bày được ý chính không, phần giới thiệu bản thân có đủ mạch lạc chưa…

- Hỏi lại feedback nếu được ( Hên xui họ trả lời thôi mà đại đại có khi họ trả lời thì ngon )

- Tiếp tục apply mấy cty khác. Đừng bị động chờ ho nhận hoặc chê thò mới apply cty apply để không bị động với lại lỡ có đậu nhiều cty thì có nhiều lựa chọn hơn. Phỏng vấn càng nhiều, kinh nghiệm càng dày.

- Cập nhật lại CV hoặc portfolio nếu thấy có điểm nào chưa đủ thuyết phục. Đôi khi chỉ cần điều chỉnh cách mô tả dự án, thêm số liệu, hoặc minh hoạ rõ hơn vai trò của mình là đã tăng cơ hội đậu cho lần tiếp theo.

- Nếu mà phỏng vấn mấy công ty lớn á ( kiểu quốc tế đồ) thì nên sau buổi phỏng vấn trong vòng 24 giờ,có thể gửi email cảm ơn ngắn gọn: cảm ơn thời gian, nhấn mạnh lại sự phù hợp của mình, và mong được cập nhật kết quả. Đây là điểm cộng nhỏ nhưng rất chuyên nghiệp.

- Tiếp tục học và bổ sung lại kiến thức còn thiếu

- Giữ tâm lý bình tĩnh . Tạch phỏng vấn nhiều khi k phải mình kém (hoặc kém thiệt thì ít ra cũng biết là mình đang kém để học thêm), có thể là do không phù hợp. Mỗi công ty có tiêu chí riêng, nên đừng để một kết quả ảnh hưởng đến sự tự tin của mình.

---

## NOTE thêm mấy cái mập chuẩn bị khi đi phỏng vấn:

### Link câu hỏi chuẩn bị

https://claude.ai/share/9bb60155-137b-4b11-b6c1-6b4f36354492

https://claude.ai/share/ddcd1c2a-2e25-4887-b4b0-7add63c212de

### Mấy cái mập chuẩn bị trước khi phỏng vấn fresher

- [NOTE 1](chuan_bi_phong_van.md)

- [NOTE 2](mock_interview_1.md)

---

&nbsp;&nbsp;&nbsp;&nbsp;Hy vọng những kinh nghiệm nhỏ này sẽ giúp ích cho bạn nào đang chuẩn bị đi tìm việc — nhất là mấy bạn fresher.

Đừng ngại bị tạch, đừng sợ bị đánh giá, mạnh dạn apply, mạnh dạn học,sửa sai.

Đọc thấy cái chi k phù hợp thì hoặc có kinh nghiệm chi cần chia sẻ thì liên hệ qua:

📌 Facebook: https://www.facebook.com/nhatnguyen118203/

📌 LinkedIn: https://www.linkedin.com/in/nhatnguyendinh/

📌 Email: nhatnguyen188203@gmail.com

📌 GitHub: https://github.com/dinhnhatnguyen

Chúc mấy bạn thành công 🔥
