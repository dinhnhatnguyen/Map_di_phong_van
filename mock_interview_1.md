# Giới thiệu bản thân:

Chào anh/chị, em là Nguyễn Đình Nhật, hiện đang là sinh viên năm cuối ngành Kỹ thuật phần mềm tại Đại học Khoa học Huế. Em có kinh nghiệm thực tế về phát triển full-stack thông qua dự án thực tập tại FPT Software, nơi em đã xây dựng một hệ thống thi đấu lập trình online sử dụng Spring Boot, ReactJS và Docker. Dự án này đã giúp em đạt giải Ba tại cuộc thi Mock Project của FPT và giải Khuyến khích tại HUE-ICT CHALLENGE 2025.

Em đặc biệt quan tâm đến backend development và có kinh nghiệm với các công nghệ như Java, Spring Boot, AWS, Docker.

Về mục tiêu: Em đang tìm kiếm cơ hội làm Fresher Software Engineer để áp dụng kiến thức đã học vào sản phẩm thực tế và học hỏi từ các senior developer.

# Giới thiệu Dự án OCCS

## 1. Giới thiệu tổng quan dự án

Dạ, **OCCS** là viết tắt của _Online Coding Competition System_, một nền tảng luyện tập và thi đấu lập trình trực tuyến mà em phát triển trong thời gian thực tập và hoàn thiện trong khóa luận tốt nghiệp.  
Ý tưởng của dự án là xây dựng một hệ thống tương tự như **LeetCode** nhưng được Việt hóa, giúp sinh viên rèn luyện kỹ năng lập trình, tổ chức các cuộc thi nội bộ với chấm điểm tự động, xếp hạng real-time và giao diện thân thiện.

---

## 2. Kiến trúc và công nghệ sử dụng

Hệ thống được thiết kế theo kiến trúc **microservices**, gồm:

- **Backend**: Spring Boot để xây dựng RESTful APIs quản lý người dùng, bài tập, submissions, cuộc thi.
- **Frontend**: ReactJS với CodeMirror editor cho phép code trực tiếp trên trình duyệt.
- **Docker**: Mỗi bài nộp chạy trong một container riêng biệt để đảm bảo an toàn và cô lập.
- **AWS SQS**: Dùng làm hàng đợi xử lý submissions bất đồng bộ, tăng khả năng chịu tải.
- **PostgreSQL**: Lưu trữ problems, test cases, lịch sử submissions, dữ liệu người dùng.
- **Triển khai**: AWS EC2 để đảm bảo truy cập từ bên ngoài.

---

## 3. Quy trình hoạt động

1. Người dùng submit code trên giao diện ReactJS.
2. Backend gửi thông tin bài nộp vào **AWS SQS queue**.
3. **Worker service** lấy request từ queue, tạo Docker container để chạy code với test cases.
4. Kết quả được lưu vào **PostgreSQL database**.
5. **Leaderboard** được cập nhật ngay lập tức để phản ánh kết quả mới.

---

## 4. Thách thức và cách giải quyết

- **Bảo mật**: Code của user có thể chứa lệnh độc hại → Giải quyết bằng Docker isolation, giới hạn CPU, RAM và thời gian chạy.
- **Hiệu năng**: Nhiều submissions cùng lúc có thể gây quá tải → Xử lý bất đồng bộ qua message queue.
- **Hỗ trợ đa ngôn ngữ**: Cấu hình nhiều Docker image cho Java, Python, C++ với compiler tương ứng.

---

## Câu hỏi follow-up có thể gặp

### "Tại sao chọn Docker cho code execution?"

"Docker giúp isolation hoàn toàn - user code chạy trong container riêng với resource limits, không thể access host system hay affect submissions khác. Còn nếu run trực tiếp trên server sẽ rất nguy hiểm."

### "Message queue có cần thiết không?"

"Cần thiết lắm ạ! Nếu 100 người submit cùng lúc mà xử lý đồng bộ thì server sẽ crash. Queue giúp smooth load và ensure no submission bị lost."

### Có measure performance metrics không?

"Ở thời điểm tham gia cuộc thi thì em chưa có điều kiện để test performance nhưng ý tưởng của em là xây dựng hệ thống với ..."

I am a final-year Software Engineering student with strong skills in full-stack web development and cloud technologies.
Built an online coding competition system (Spring Boot, ReactJS, Docker, AWS) that won awards at FPT Software and HUE-ICT Challenge 2025.
Gained experience with Java, Spring Boot, REST APIs, ReactJS, Next.js, and cloud services.
Passionate about learning and applying microservices, containerization, and AWS to build scalable applications.
I am seeking a Fresher/Junior Software Engineer position where I can contribute, grow, and continue learning new technologies.
