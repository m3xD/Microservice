# Microservice

## 1. Tại sao nên sử dụng microservice:
### 1.1. Một số đặc điểm của việc sử dụng monolithic:
- Lợi ích:
 * Dễ dàng để phát triển do chỉ tập trung vào một ứng dụng.
 * Dễ dàng thay đổi code, database schema, build, deploy.
 * Dễ dàng để test do ứng dụng được phát triển để viết cho người dùng cuối.
 * Dễ dàng deploy do chỉ có duy nhất một file (package).
 * Dễ dàng scale do có thể chạy trên nhiều instance.
-> Nhưng việc phát triển, test, scale sẽ gặp nhiều vấn đề.
- Những hạn chế:
  * Mỗi lần thêm các feature làm cho code base càng ngày lớn hơn.
  * Khi code base càng lớn, độ phức tạp các code càng cao, càng khó hiểu, không thể dễ dàng debug, testing.
  * Quá trình phát triển phần mềm cũng tốn thời gian hơn do các giai đoạn edit, build, run, test đều kéo dài ra do sự ảnh hưởng của code base quá lớn.
  * Update các tính năng cho ứng dụng trở nên khó khăn hơn.
  * Vấn đề scaling cho từng module không còn đủ tương thích với từng nhiệm vụ của module.
  * Tính tin cậy của ứng dụng khi chạy cũng không đảm bảo do code base lớn, việc test trở nên khó khăn, khi có bug xảy ra thì các instance đều có thể đồng loạt sập.
  * Tech stack của ứng dụng không thể thay đổi.
### 1.2. Microservice giải vây:
- Sử dụng **monolithic** có nhiều vấn đề, trong đó những vấn đề lớn nhất là: khả năng mở rộng kém, các yêu cầu phi chức năng như testing, auth,... không thể được đảm bảo, từ đó ta đưa ra khái niệm **microservice**.
- Khái niệm **microservice**: ta sử dụng khái niệm scale cube của `Martin Abbott and Michael Fisher’s excellent book, The Art of Scalability` được định nghĩa thành 3 chiều scale khác nhau trong đó:
  * Chiều X: mở rộng thành nhiều **instance**, phân bố các request đều lên các **instance** khác nhau thông qua **load balancer**.
    ![X_Scale](png/scale_x.png)
  * Chiều Y: giống chiều X, nhưng các **instance** lại đảm nhận lưu trữ một tập dữ liệu theo một đặc điểm nào nó như **userID**.
    ![Y_Scale](png/scale_y.png)
    -> Scale theo 2 chiều X và Y đều giúp đảm bảo khả năng sẵn có cũng như dung lượng nhưng không giải quyết được bài toán phát triển mở rộng hay code base quá phức tạp, và chúng ta có thêm scale chiều Z.
  * Chiều Z: được định nghĩa là phân bố **monolithic** thành các **services**, các **services** được định nghĩa là các ứng dụng nhỏ hơn của ứng dụng tổng thể, các service có thể được scale theo chiều X và có thể là cả chiều Y.
    ![Z_Scale](png/scale_z.png)
