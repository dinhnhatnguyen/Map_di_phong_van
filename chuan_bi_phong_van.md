# 1. Giới thiệu bản thân

Chào anh/chị, em là Nguyễn Đình Nhật, hiện đang là sinh viên năm cuối ngành Kỹ thuật phần mềm tại Đại học Khoa học Huế. Em có kinh nghiệm thực tế về phát triển full-stack thông qua dự án thực tập tại FPT Software, nơi em đã xây dựng một hệ thống thi đấu lập trình online sử dụng Spring Boot, ReactJS và Docker. Dự án này đã giúp em đạt giải Ba tại cuộc thi Mock Project của FPT và giải Khuyến khích tại HUE-ICT CHALLENGE 2025.

Em đặc biệt quan tâm đến backend development và có kinh nghiệm với các công nghệ như Java, Spring Boot, AWS, Docker.

Về mục tiêu: Em đang tìm kiếm cơ hội làm Fresher Software Engineer để áp dụng kiến thức đã học vào sản phẩm thực tế và học hỏi từ các senior developer.

# 2. Giới thiệu dự án OCCS

Dạ, **OCCS** là viết tắt của _Online Coding Competition System_, một nền tảng luyện tập và thi đấu lập trình trực tuyến mà em phát triển trong thời gian thực tập và hoàn thiện trong khóa luận tốt nghiệp.  
Ý tưởng của dự án là xây dựng một hệ thống tương tự như **LeetCode** nhưng được Việt hóa, giúp sinh viên rèn luyện kỹ năng lập trình, tổ chức các cuộc thi nội bộ với chấm điểm tự động, xếp hạng real-time và giao diện thân thiện.

- **Backend**: Spring Boot để xây dựng RESTful APIs quản lý người dùng, bài tập, submissions, cuộc thi.
- **Frontend**: ReactJS với CodeMirror editor cho phép code trực tiếp trên trình duyệt.
- **Docker**: Mỗi bài nộp chạy trong một container riêng biệt để đảm bảo an toàn và cô lập.
- **AWS SQS**: Dùng làm hàng đợi xử lý submissions bất đồng bộ, tăng khả năng chịu tải.
- **PostgreSQL**: Lưu trữ problems, test cases, lịch sử submissions, dữ liệu người dùng.
- **Triển khai**: AWS EC2 để đảm bảo truy cập từ bên ngoài.

Dự án này em đảm nhận vai trò Technical Leader cho team 3 intern trong 3 tháng thực tập tại FPT Software

"Với vai trò leader, em đã:

- Thiết kế architecture tổng thể và phân chia công việc: em handle backend + Docker, teammate 1 phụ trách frontend React, teammate 2 đảm nhận database + testing
- Establish quy trình làm việc: daily standup, code review standards, và Git workflow cho toàn team
- Lead các technical decisions: chọn Spring Boot, implement Docker execution, sử dụng message queue với AWS SQS"

# 3. DSA

## 1. So sánh Heap vs Stack:

**Stack (Ngăn xếp):**

- Giống như một chồng đĩa - bạn chỉ có thể thêm hoặc lấy đĩa từ trên cùng
- Hoạt động theo nguyên tắc LIFO (Last In First Out - vào sau ra trước)
- Được quản lý tự động bởi CPU
- Tốc độ truy cập rất nhanh
- Kích thước hạn chế (thường vài MB)
- Lưu trữ: biến local, tham số hàm, địa chỉ return
- Khi hàm kết thúc, bộ nhớ được giải phóng tự động

**Heap (Đống):**

- Giống như một kho lưu trữ lớn, bạn có thể lấy bất cứ thứ gì ở bất cứ đâu
- Không có thứ tự cố định
- Được quản lý thông qua con trỏ
- Tốc độ truy cập chậm hơn Stack
- Kích thước lớn hơn nhiều (có thể GB)
- Lưu trữ: đối tượng lớn, mảng động, dữ liệu được cấp phát động
- Cần giải phóng thủ công (trong C/C++) hoặc có Garbage Collector (Java, C#)

**Ví dụ thực tế:**
Khi bạn gọi một hàm, các biến local sẽ được đẩy vào Stack. Khi bạn tạo một mảng lớn bằng `malloc()` hoặc `new`, nó sẽ được lưu trong Heap.

Stack nhanh nhưng hạn chế, Heap chậm hơn nhưng linh hoạt hơn. Đó là sự đánh đổi cơ bản giữa chúng.

## 2. So sánh Queue vs Stack:

Stack và Queue là hai cấu trúc dữ liệu cơ bản có những đặc điểm khác biệt rõ rệt:

## Stack (Ngăn xếp)

**Nguyên tắc hoạt động:** LIFO (Last In, First Out) - phần tử cuối cùng được thêm vào sẽ được lấy ra đầu tiên.

**Các thao tác chính:**

- `push()`: Thêm phần tử vào đỉnh stack
- `pop()`: Lấy và xóa phần tử ở đỉnh stack
- `peek()/top()`: Xem phần tử ở đỉnh mà không xóa
- `isEmpty()`: Kiểm tra stack có rỗng không

**Ứng dụng:**

- Quản lý lời gọi hàm (call stack)
- Thuật toán backtracking
- Kiểm tra cặp dấu ngoặc
- Chuyển đổi biểu thức toán học
- Undo/Redo trong ứng dụng

## Queue (Hàng đợi)

**Nguyên tắc hoạt động:** FIFO (First In, First Out) - phần tử đầu tiên được thêm vào sẽ được lấy ra đầu tiên.

**Các thao tác chính:**

- `enqueue()`: Thêm phần tử vào cuối queue
- `dequeue()`: Lấy và xóa phần tử ở đầu queue
- `front()`: Xem phần tử ở đầu mà không xóa
- `isEmpty()`: Kiểm tra queue có rỗng không

**Ứng dụng:**

- Xử lý tác vụ theo thứ tự (job scheduling)
- BFS (Breadth-First Search)
- Buffer trong hệ thống I/O
- Xử lý yêu cầu trong web server
- Print queue

## So sánh tổng quan

| Đặc điểm         | Stack                          | Queue                         |
| ---------------- | ------------------------------ | ----------------------------- |
| **Thứ tự**       | LIFO                           | FIFO                          |
| **Thêm phần tử** | Chỉ ở đỉnh (top)               | Chỉ ở cuối (rear)             |
| **Lấy phần tử**  | Chỉ ở đỉnh (top)               | Chỉ ở đầu (front)             |
| **Độ phức tạp**  | O(1) cho các thao tác cơ bản   | O(1) cho các thao tác cơ bản  |
| **Truy cập**     | Chỉ truy cập được phần tử đỉnh | Chỉ truy cập được phần tử đầu |

Cả hai đều có độ phức tạp thời gian O(1) cho các thao tác cơ bản và đều rất hữu ích tùy thuộc vào yêu cầu cụ thể của bài toán.

# 4. Design parten

- Buider - singleton
- Adapter - Facade
- Observer - Strategy

# 5. Java

## 1. Đây là sự so sánh giữa Array (mảng) và ArrayList (danh sách mảng động):

## Array (Mảng)

**Đặc điểm:**

- Kích thước cố định, được xác định khi khai báo
- Lưu trữ các phần tử cùng kiểu dữ liệu
- Truy cập trực tiếp qua chỉ số với độ phức tạp O(1)
- Được cấp phát bộ nhớ liên tục

**Ví dụ:**

```java
int[] arr = new int[5]; // Cố định 5 phần tử
```

## ArrayList (Danh sách mảng động)

**Đặc điểm:**

- Kích thước có thể thay đổi trong quá trình thực thi
- Tự động mở rộng khi cần thiết
- Cung cấp nhiều phương thức tiện ích (add, remove, contains...)
- Sử dụng Array bên trong nhưng quản lý kích thước tự động

**Ví dụ:**

```java
ArrayList<Integer> list = new ArrayList<>(); // Không giới hạn kích thước ban đầu
```

## Tại sao ArrayList "không giới hạn" về số phần tử?

ArrayList thực chất **KHÔNG thực sự không giới hạn**, nhưng tạo cảm giác như vậy vì:

### 1. **Cơ chế Dynamic Resizing**

```java
// Bên trong ArrayList
private Object[] elementData;
private int size;
private static final int DEFAULT_CAPACITY = 10;

// Khi thêm phần tử mà hết chỗ
private void grow() {
    int oldCapacity = elementData.length;
    int newCapacity = oldCapacity + (oldCapacity >> 1); // Tăng 50%
    elementData = Arrays.copyOf(elementData, newCapacity);
}
```

### 2. **Quy trình mở rộng:**

- Ban đầu: Capacity = 10
- Khi đầy: Tạo array mới với kích thước 15 (10 + 5)
- Copy tất cả phần tử từ array cũ sang array mới
- Tiếp tục quá trình này khi cần thiết

### 3. **Giới hạn thực tế:**

- **Giới hạn lý thuyết:** `Integer.MAX_VALUE - 8` (khoảng 2.1 tỷ phần tử)
- **Giới hạn thực tế:** Phụ thuộc vào bộ nhớ RAM khả dụng

## So sánh chi tiết

| Đặc điểm               | Array              | ArrayList            |
| ---------------------- | ------------------ | -------------------- |
| **Kích thước**         | Cố định            | Động, tự mở rộng     |
| **Hiệu năng truy cập** | O(1)               | O(1)                 |
| **Hiệu năng chèn/xóa** | O(n)               | O(n) (trung bình)    |
| **Bộ nhớ**             | Ít overhead        | Nhiều overhead hơn   |
| **Kiểu dữ liệu**       | Primitive + Object | Chỉ Object (wrapper) |
| **Thread-safe**        | Không              | Không                |

## Ví dụ minh họa

```java
// Array - Lỗi nếu vượt quá kích thước
int[] arr = new int[3];
arr[0] = 1;
arr[1] = 2;
arr[2] = 3;
// arr[3] = 4; // ArrayIndexOutOfBoundsException

// ArrayList - Tự động mở rộng
ArrayList<Integer> list = new ArrayList<>();
list.add(1);
list.add(2);
list.add(3);
list.add(4); // Không lỗi, tự động mở rộng
System.out.println(list.size()); // 4
```

## Trade-offs

**Array phù hợp khi:**

- Biết trước kích thước cần thiết
- Cần hiệu năng tối đa
- Sử dụng primitive types
- Bộ nhớ hạn chế

**ArrayList phù hợp khi:**

- Không biết trước kích thước
- Cần thêm/xóa phần tử thường xuyên
- Cần các phương thức tiện ích
- Linh hoạt trong lập trình

Vậy nên ArrayList không thực sự "vô hạn" mà chỉ tạo ảo giác như vậy nhờ cơ chế tự động mở rộng thông minh.

## 2. String

# String trong Java - Hướng dẫn toàn diện

## 1. String là gì?

String trong Java là một **class** đại diện cho chuỗi ký tự. Đây là một trong những class được sử dụng nhiều nhất trong Java.

```java
String str = "Hello World";
String str2 = new String("Hello World");
```

## 2. Đặc điểm quan trọng của String

### **Immutable (Bất biến)**

- Một khi tạo ra, giá trị của String không thể thay đổi
- Mọi thao tác "thay đổi" đều tạo ra String object mới

```java
String str = "Hello";
str = str + " World"; // Tạo String object mới, không sửa đổi "Hello"
```

### **String Pool**

- Java lưu trữ String literals trong một vùng nhớ đặc biệt gọi là String Pool
- Giúp tiết kiệm bộ nhớ bằng cách tái sử dụng các String giống nhau

```java
String s1 = "Hello";        // Lưu trong String Pool
String s2 = "Hello";        // Tái sử dụng từ Pool
String s3 = new String("Hello"); // Tạo object mới trong Heap

System.out.println(s1 == s2);     // true (cùng reference)
System.out.println(s1 == s3);     // false (khác reference)
System.out.println(s1.equals(s3)); // true (cùng nội dung)
```

## 3. Cách tạo String

### **String Literal**

```java
String str = "Hello World";
```

### **Sử dụng constructor**

```java
String str1 = new String("Hello");
String str2 = new String(char[] chars);
String str3 = new String(byte[] bytes);
```

### **Từ các kiểu dữ liệu khác**

```java
String str1 = String.valueOf(123);     // "123"
String str2 = String.valueOf(true);    // "true"
String str3 = String.valueOf(45.67);   // "45.67"
```

## 4. Các phương thức quan trọng

### **Thông tin cơ bản**

```java
String str = "Hello World";

// Độ dài
int length = str.length(); // 11

// Kiểm tra rỗng
boolean isEmpty = str.isEmpty(); // false
boolean isBlank = str.isBlank(); // false (Java 11+)

// Lấy ký tự tại vị trí
char ch = str.charAt(0); // 'H'
```

### **So sánh**

```java
String s1 = "Hello";
String s2 = "hello";

// So sánh phân biệt hoa thường
boolean equals = s1.equals(s2); // false

// So sánh không phân biệt hoa thường
boolean equalsIgnoreCase = s1.equalsIgnoreCase(s2); // true

// So sánh lexicographically
int compare = s1.compareTo(s2); // âm số (H < h trong ASCII)
int compareIgnoreCase = s1.compareToIgnoreCase(s2); // 0
```

### **Tìm kiếm**

```java
String str = "Hello World";

// Tìm ký tự/chuỗi con
int index = str.indexOf('o');        // 4 (lần đầu tiên)
int lastIndex = str.lastIndexOf('o'); // 7 (lần cuối)
int index2 = str.indexOf("World");   // 6

// Kiểm tra bắt đầu/kết thúc
boolean starts = str.startsWith("Hello"); // true
boolean ends = str.endsWith("World");     // true

// Kiểm tra chứa
boolean contains = str.contains("llo");   // true
```

### **Trích xuất**

```java
String str = "Hello World";

// Lấy chuỗi con
String sub1 = str.substring(6);     // "World"
String sub2 = str.substring(0, 5);  // "Hello"

// Tách chuỗi
String[] words = str.split(" ");    // ["Hello", "World"]
String[] chars = str.split("");     // ["H", "e", "l", "l", "o", " ", "W", "o", "r", "l", "d"]
```

### **Biến đổi**

```java
String str = "  Hello World  ";

// Chuyển đổi hoa/thường
String upper = str.toUpperCase();   // "  HELLO WORLD  "
String lower = str.toLowerCase();   // "  hello world  "

// Loại bỏ khoảng trắng
String trimmed = str.trim();        // "Hello World"
String stripped = str.strip();     // "Hello World" (Java 11+)

// Thay thế
String replaced = str.replace("World", "Java");     // "  Hello Java  "
String replacedFirst = str.replaceFirst("l", "L");  // "  HeLlo World  "
String replacedAll = str.replaceAll("l", "L");      // "  HeLLo WorLd  "
```

### **Chuyển đổi**

```java
String str = "Hello World";

// Thành mảng ký tự
char[] charArray = str.toCharArray(); // ['H', 'e', 'l', ...]

// Thành byte array
byte[] bytes = str.getBytes();        // [72, 101, 108, ...]
byte[] utf8Bytes = str.getBytes("UTF-8");
```

## 5. String Formatting

### **String.format()**

```java
String formatted = String.format("Tên: %s, Tuổi: %d, Điểm: %.2f",
                                 "Nam", 25, 8.75);
// "Tên: Nam, Tuổi: 25, Điểm: 8.75"
```

### **printf-style formatting**

```java
String name = "Alice";
int age = 30;
String message = String.format("Hello %s, you are %d years old", name, age);
```

## 6. StringBuilder và StringBuffer

Vì String là immutable, việc nối chuỗi nhiều lần sẽ tạo ra nhiều object không cần thiết:

### **StringBuilder (Không thread-safe, nhanh hơn)**

```java
StringBuilder sb = new StringBuilder();
sb.append("Hello");
sb.append(" ");
sb.append("World");
String result = sb.toString(); // "Hello World"

// Hoặc method chaining
String result2 = new StringBuilder()
    .append("Hello")
    .append(" ")
    .append("World")
    .toString();
```

### **StringBuffer (Thread-safe, chậm hơn)**

```java
StringBuffer buffer = new StringBuffer();
buffer.append("Hello");
buffer.append(" World");
String result = buffer.toString();
```

## 7. Pattern Matching và Regular Expressions

```java
String text = "Hello123World";

// Kiểm tra pattern
boolean matches = text.matches("[A-Za-z0-9]+"); // true

// Tách theo regex
String[] parts = "apple,banana;orange".split("[,;]"); // ["apple", "banana", "orange"]

// Thay thế theo regex
String result = "Hello123World".replaceAll("\\d+", "_"); // "Hello_World"
```

## 8. String Comparison Deep Dive

```java
// So sánh reference vs content
String s1 = "Hello";
String s2 = "Hello";
String s3 = new String("Hello");

System.out.println(s1 == s2);        // true (cùng reference trong pool)
System.out.println(s1 == s3);        // false (khác reference)
System.out.println(s1.equals(s3));   // true (cùng content)

// Intern method - đưa string vào pool
String s4 = s3.intern();
System.out.println(s1 == s4);        // true
```

## 9. Performance Tips

### **Tránh nối chuỗi trong vòng lặp**

```java
// BAD - tạo nhiều object
String result = "";
for (int i = 0; i < 1000; i++) {
    result += "a"; // Tạo 1000 String objects
}

// GOOD - sử dụng StringBuilder
StringBuilder sb = new StringBuilder();
for (int i = 0; i < 1000; i++) {
    sb.append("a");
}
String result = sb.toString(); // Chỉ tạo 1 String object cuối
```

### **Sử dụng String.join() cho việc nối**

```java
// Java 8+
String[] words = {"Hello", "World", "Java"};
String joined = String.join(" ", words); // "Hello World Java"
```

## 10. String trong Java 8+ Features

### **String methods mới**

```java
// Java 11+
String str = "  Hello World  ";
boolean isBlank = str.isBlank();           // false
String stripped = str.strip();            // "Hello World"
String[] lines = "A\nB\nC".lines().toArray(String[]::new); // ["A", "B", "C"]

// Java 12+
String indented = "Hello".indent(4);       // "    Hello"

// Java 15+
String textBlock = """
    Hello
    World
    """;
```

## 11. Memory và Performance

### **String Pool Location**

- Java 7+: String Pool được chuyển từ PermGen sang Heap
- Giúp tránh OutOfMemoryError và có thể garbage collect

### **String Interning**

```java
String s1 = new String("Hello").intern(); // Đưa vào pool
String s2 = "Hello";
System.out.println(s1 == s2); // true
```

## 12. Best Practices

1. **Sử dụng String literals thay vì new String()**

```java
// GOOD
String s = "Hello";

// BAD (không cần thiết)
String s = new String("Hello");
```

2. **Sử dụng StringBuilder cho việc nối chuỗi nhiều**
3. **Cẩn thận với null checks**

```java
// Safe comparison
Objects.equals(str1, str2); // Xử lý null safety

// Hoặc
if (str != null && str.equals("expected")) {
    // logic
}
```

4. **Sử dụng String.isEmpty() và String.isBlank()**

```java
if (str != null && !str.trim().isEmpty()) {
    // Java 11+: str.isBlank()
}
```

String trong Java là một topic rất sâu và rộng, nhưng những kiến thức trên sẽ giúp bạn hiểu và sử dụng String một cách hiệu quả trong hầu hết các tình huống lập trình.

## NOTE thêm Khi đi phỏng vấn của mập

### Link câu hỏi chuẩn bị

https://claude.ai/share/9bb60155-137b-4b11-b6c1-6b4f36354492
https://claude.ai/share/ddcd1c2a-2e25-4887-b4b0-7add63c212de

### Mấy cái mập chuẩn bị trước khi phỏng vấn fresher

- [NOTE 1](#1-giới-thiệu-bản-thân)
- [NOTE 2](mock_interview_1.md)
