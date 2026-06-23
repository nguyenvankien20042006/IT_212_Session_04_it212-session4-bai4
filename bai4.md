# BÀI 4: Phân tích & Lựa chọn (Kỹ thuật Prompt lặp tối ưu hóa thuật toán)

## 1. Đáp án lựa chọn

**Chọn phương án B.**

---

# 2. Vì sao phương án B là phản hồi tối ưu nhất?

Phương án B là phản hồi tối ưu nhất vì nó thể hiện đúng tinh thần của **Iterative Prompting**: không chỉ nói rằng “code đang chậm”, mà còn **phản hồi lại kết quả AI đã sinh ở lượt trước bằng các thông tin kỹ thuật cụ thể**, chỉ ra nguyên nhân hiệu năng kém, xác định rõ hướng tối ưu cần áp dụng và đặt ra ràng buộc đầu ra cho phiên bản mã nguồn tiếp theo.

Nói cách khác, phương án B giúp AI **biết chính xác vấn đề là gì, cần sửa theo hướng nào và phải đạt các tiêu chí kỹ thuật nào sau khi sửa**.

---

# 3. Phân tích phương án B theo tính cụ thể về thuật toán và ràng buộc kỹ thuật

## 3.1. Chỉ ra đúng vấn đề hiệu năng của lời giải ban đầu

Phương án B nêu rất rõ:

> **"Hàm fib đệ quy trên có độ phức tạp O(2N), gây treo hệ thống khi n = 50."**

Điểm mạnh ở đây là prompt không dừng ở mức cảm tính kiểu *“code chạy chậm”*, mà đã **chẩn đoán đúng bản chất kỹ thuật** của vấn đề:

* lời giải hiện tại là **đệ quy cổ điển**;
* số lượng lời gọi hàm tăng theo cấp số nhân;
* khi `n` lớn (ví dụ `n = 50`), chương trình sẽ chạy rất chậm hoặc có cảm giác bị treo.

Trong kỹ thuật **Prompt lặp**, việc phản hồi lại kết quả AI bằng thông tin cụ thể như vậy cực kỳ quan trọng, vì AI không phải đoán “chậm ở đâu” hay “vì sao chậm”.

> **Lưu ý về mặt ký hiệu độ phức tạp:**
> Về mặt học thuật, Fibonacci đệ quy cổ điển thường được mô tả là **O(2^N)** hoặc chính xác hơn là xấp xỉ **O(φ^N)**. Dù prompt B ghi `O(2N)` theo nguyên văn đề bài, tinh thần của nó vẫn đúng ở chỗ **đang nhấn mạnh thuật toán hiện tại tăng theo cấp số nhân và cần thay bằng DP**.

---

## 3.2. Chỉ định rõ kỹ thuật tối ưu cần dùng

Phương án B không chỉ bảo AI “viết lại cho nhanh hơn”, mà yêu cầu rất cụ thể:

> **"Hãy tối ưu lại hàm này sử dụng kỹ thuật Quy hoạch động (Dynamic Programming - Tabulation hoặc Memoization)"**

Đây là điểm cực kỳ mạnh, vì nó:

* **định hướng đúng lớp thuật toán** cần dùng;
* thu hẹp không gian lựa chọn của AI;
* tránh trường hợp AI đề xuất một hướng tối ưu không phù hợp hoặc kém hiệu quả hơn.

Với bài toán Fibonacci, yêu cầu dùng **Dynamic Programming** là hoàn toàn hợp lý vì:

* có **bài toán con chồng lặp**;
* có thể lưu kết quả đã tính để tránh tính lại;
* dễ đưa thời gian chạy từ **hàm mũ** xuống **tuyến tính O(N)**.

Ngoài ra, phương án B còn mở cho AI **2 hướng hợp lệ**:

* **Memoization**: đệ quy + cache;
* **Tabulation**: lặp từ dưới lên.

Điều này vừa đủ chặt chẽ để định hướng, vừa đủ linh hoạt để AI chọn lời giải tốt.

---

## 3.3. Đặt mục tiêu tối ưu bằng thông số định lượng rõ ràng

Phương án B yêu cầu:

> **"đưa độ phức tạp thời gian về O(N) và độ phức tạp không gian O(1) hoặc O(N)."**

Đây là phần rất quan trọng vì nó biến yêu cầu “tối ưu hơn” thành **tiêu chí đo lường được**.

### Ý nghĩa của điều này:

* AI không thể trả lời bằng một thuật toán “có vẻ nhanh hơn” nhưng vẫn chưa đủ tốt.
* Người dùng có thể **đánh giá đầu ra** của AI dựa trên mục tiêu cụ thể.
* Prompt trở nên giống một **spec kỹ thuật** hơn là một lời nhờ chung chung.

Ví dụ:

* Nếu AI chọn **Memoization**, kết quả thường là **O(N)** thời gian và **O(N)** bộ nhớ.
* Nếu AI chọn **Tabulation tối ưu biến**, có thể đạt **O(N)** thời gian và **O(1)** bộ nhớ.

Như vậy prompt B không chỉ nói “hãy sửa”, mà còn nêu rõ **đích tối ưu** mà lời giải phải đạt được.

---

## 3.4. Giữ nguyên ràng buộc giao diện hàm

Phương án B còn nêu:

> **"Giữ nguyên kiểu trả về long."**

Đây là một ràng buộc kỹ thuật quan trọng trong bối cảnh refactor hoặc tối ưu mã nguồn. Nó đảm bảo:

* AI không tự ý đổi sang `BigInteger`, `int`, `double` hoặc kiểu khác;
* code sau tối ưu vẫn tương thích với phần còn lại của hệ thống nếu nơi khác đang gọi `fib()` và mong đợi kiểu `long`;
* việc tối ưu chỉ tác động vào **thuật toán**, không làm thay đổi **contract** của hàm.

Trong kỹ thuật **Iterative Prompting**, đây là một ví dụ điển hình của việc “phản hồi có kiểm soát”: ta cho AI biết **cái gì được thay đổi** (thuật toán), và **cái gì phải giữ nguyên** (kiểu trả về).

---

# 4. Kết luận: Vì sao B là lựa chọn đúng nhất?

Phương án B là tối ưu nhất vì nó có đủ các yếu tố của một prompt lặp chất lượng:

* **Nêu rõ vấn đề của phiên bản trước**: đệ quy gây chậm/treo khi `n = 50`.
* **Chỉ ra bản chất kỹ thuật**: thuật toán hiện tại có độ phức tạp rất lớn.
* **Yêu cầu đúng kỹ thuật thay thế**: Dynamic Programming.
* **Đưa mục tiêu định lượng rõ ràng**: thời gian **O(N)**, không gian **O(1) hoặc O(N)**.
* **Giữ ràng buộc giao diện**: vẫn trả về `long`.

Đây là một prompt vừa **cụ thể**, vừa **kiểm soát được đầu ra**, vừa đúng với mục tiêu của **Iterative Prompting**: dùng phản hồi từ kết quả trước để dẫn AI đến lời giải tốt hơn ở vòng sau.

---

# 5. Phân tích nhược điểm của các phương án còn lại

---

# 5.1. Phân tích phương án A

## Phương án A:

> **"Code chạy chậm quá, viết lại bằng thuật toán khác tối ưu hơn giúp tôi."**

## Nhược điểm

### a) Quá mơ hồ, không chỉ ra nguyên nhân gốc của vấn đề

Prompt A chỉ nói rằng code “chạy chậm”, nhưng không nói:

* chậm vì đệ quy bị lặp tính toán;
* chậm ở mức nào;
* trường hợp nào gây treo;
* thuật toán hiện tại có độ phức tạp ra sao.

AI vì thế phải **tự suy đoán** nguyên nhân, và có thể không đi đúng hướng.

---

### b) Không chỉ rõ kỹ thuật cần dùng

Cụm từ **“thuật toán khác tối ưu hơn”** quá rộng. AI có thể trả về rất nhiều hướng khác nhau:

* dùng vòng lặp đơn giản;
* dùng cache thủ công;
* dùng ma trận lũy thừa;
* dùng công thức Binet;
* thậm chí chỉ chỉnh sửa nhẹ mà không thật sự cải thiện đáng kể.

Mặc dù một số hướng có thể đúng, nhưng prompt A **không kiểm soát được** AI sẽ chọn phương án nào. Trong bài này, đề bài muốn hướng đến **Dynamic Programming**, nên A chưa đủ tốt.

---

### c) Không có mục tiêu định lượng về độ phức tạp

Prompt A không hề yêu cầu:

* thời gian phải về **O(N)**;
* bộ nhớ chấp nhận **O(1)** hay **O(N)**;
* giữ nguyên kiểu trả về.

Do đó AI có thể đưa ra lời giải “nhanh hơn một chút” nhưng vẫn không đạt chuẩn mong muốn.

---

### d) Không mang tinh thần Iterative Prompting mạnh bằng B

Iterative Prompting không chỉ là “làm lại đi”, mà là:

* chỉ ra **đầu ra cũ có vấn đề gì**,
* giải thích **vì sao có vấn đề**,
* yêu cầu **sửa theo tiêu chí cụ thể**.

Prompt A mới dừng ở mức phàn nàn rằng “chạy chậm”, chứ chưa thật sự là một vòng lặp tối ưu có chất lượng.

---

## Kết luận về A

Phương án A không sai hoàn toàn, nhưng **quá chung chung**, thiếu:

* chỉ định thuật toán,
* ràng buộc kỹ thuật,
* thông số độ phức tạp,
* và định hướng rõ ràng cho vòng lặp tối ưu.

Vì vậy A **kém tối ưu hơn B rất nhiều**.

---

# 5.2. Phân tích phương án C

## Phương án C:

> **"Hãy viết lại code tính Fibonacci bằng cách sử dụng Java Stream API để code chạy song song (parallel) cho nhanh hơn."**

## Nhược điểm

### a) Chọn sai hướng tối ưu cho bản chất bài toán

Bài toán Fibonacci bị chậm không phải vì thiếu song song hóa, mà vì thuật toán đệ quy hiện tại **tính đi tính lại cùng một bài toán con rất nhiều lần**. Gốc rễ của vấn đề là **độ phức tạp thuật toán**, không phải cách phân phối luồng xử lý.

Do đó, nếu không xử lý phần **overlapping subproblems**, việc dùng `parallel stream` không giải quyết được bản chất vấn đề.

---

### b) Stream API không phải công cụ phù hợp để tính Fibonacci kiểu này

`Java Stream API` phù hợp hơn với:

* xử lý tập dữ liệu;
* filter/map/reduce;
* pipeline thao tác trên collection.

Trong khi đó, Fibonacci là bài toán tính toán tuần tự, phụ thuộc vào các giá trị trước đó:

* `fib(n)` phụ thuộc vào `fib(n-1)` và `fib(n-2)`;
* với cách tối ưu chuẩn, ta thường dùng mảng DP hoặc hai biến lặp.

Ép AI dùng `Stream API` trong trường hợp này dễ khiến code:

* phức tạp hơn mức cần thiết;
* khó đọc hơn;
* không chắc tối ưu hơn;
* thậm chí tạo thêm overhead.

---

### c) Parallel không đồng nghĩa với tối ưu thuật toán

Một thuật toán tệ nếu chạy song song vẫn có thể là một thuật toán tệ. Nếu bản thân lời giải còn là dạng đệ quy có số lời gọi bùng nổ, việc thêm `parallel` không biến nó thành **O(N)**.

Nói cách khác:

* **tối ưu thực sự** ở đây là đổi mô hình giải từ **recursive exponential** sang **dynamic programming linear**;
* **song song hóa** không phải yêu cầu chính và có thể làm lệch hướng giải.

---

### d) Không có ràng buộc về độ phức tạp và kiểu dữ liệu

Phương án C cũng không nêu rõ:

* cần đạt **O(N)**;
* cần dùng **DP**;
* cần giữ nguyên kiểu trả về `long`.

Vì vậy nó không đảm bảo đầu ra đáp ứng mục tiêu kỹ thuật của đề bài.

---

## Kết luận về C

Phương án C là phương án **lệch hướng**. Nó cố giải quyết bài toán bằng **parallel stream**, trong khi gốc rễ vấn đề nằm ở **thuật toán đệ quy không tối ưu**. Vì thế C không phải lựa chọn phù hợp cho kỹ thuật Prompt lặp trong trường hợp này.

---

# 6. Tổng kết cuối cùng

## Đáp án đúng: **B**

Vì phương án B:

* chỉ ra rõ **vấn đề hiệu năng** của phiên bản AI sinh ra ban đầu;
* nêu **kỹ thuật tối ưu cụ thể** là **Dynamic Programming**;
* đưa ra **thông số thuật toán mục tiêu**: thời gian **O(N)**, không gian **O(1) hoặc O(N)**;
* giữ **ràng buộc kỹ thuật** về kiểu trả về `long`.

Trong khi đó:

* **A** quá chung chung, không đủ thông tin kỹ thuật để điều hướng AI chính xác.
* **C** chọn sai công cụ tối ưu, lệch khỏi bản chất thuật toán của bài toán Fibonacci.

=> Vì vậy, **phương án B là phản hồi tối ưu nhất** khi áp dụng **Iterative Prompting** để yêu cầu AI cải thiện mã nguồn Fibonacci.
