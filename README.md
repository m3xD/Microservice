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
  * Chiều Z: giống chiều X, nhưng các **instance** lại đảm nhận lưu trữ một tập dữ liệu theo một đặc điểm nào nó như **userID**.
    ![Z_Scale](png/scale_y.png)
    
    -> Scale theo 2 chiều X và Z đều giúp đảm bảo khả năng sẵn có cũng như dung lượng nhưng không giải quyết được bài toán phát triển mở rộng hay code base quá phức tạp, và chúng ta có thêm scale chiều Z.
  * Chiều Y: được định nghĩa là phân bố **monolithic** thành các **services**, các **services** được định nghĩa là các ứng dụng nhỏ hơn của ứng dụng tổng thể, các service có thể được scale theo chiều X và có thể là cả chiều Y.
    ![Y_Scale](png/scale_z.png)
- Lợi ích của việc sử dụng kiến trúc microservice:
  * Có thể tích hợp các tính năng mới và phát triển ứng dụng một cách lâu dài
    * Thực hiện được các test yêu cầu bởi quá trình CD (continuous delivery/development) do các microservices nhỏ, các automated test dễ viết hơn và ít bug hơn.
    * Có thể thực hiện update liên tục được yêu cầu bởi quá trình CD do các microservices là độc lập, các teams có thể thực hiện liên tục các thay đổi tới ứng dụng mà không ảnh hưởng đến các services khác.
    * Quá trình phát triển mỗi team được chủ động và có liên kết lỏng lẻo (loosely coupled).
  * Mỗi service đều dễ bảo trì và kích thước nhỏ: do có code base ít nên dev có thể nắm bắt dễ hơn.
  * Mỗi service đều có thể scale một cách độc lập: các service đều có thể scale theo chiều X hoặc Z giống như ứng dụng monolithic, hơn thế các service được chạy trên các phần cứng phù hợp hơn, tối ưu hiệu suất.
  * Các service có thể xảy ra lỗi mà không ảnh hưởng đến các service khác.
  * Có thể thử nghiệm và phát triển các tính năng một cách dễ dàng: dev có thể tự do lựa chọn ngôn ngữ, framework để phát triển service.
- Hạn chế tồn tại:
  * Làm sao để phân chia các service là một bài toán khó.
  * Hệ thống phân tán rất phức tạp: các service có cách giao tiếp phức tạp hơn (interprocess communication mechanism) là chỉ gọi hàm như ứng dụng nguyên khối. Các service cũng phải xử lý được các lỗi khi liên lạc với các service khác.
  * Triển khai các tính năng cho service cần có sự tính toán hợp lý.
  * Chọn đúng thời điểm để chuyển qua kiến trúc microservice.
- Các pattern của microservices:
  *   Phân rã ứng dụng thành các services.
  *   Communication giữa các service: bao gồm communication style, discovery(làm sao để client có thể xác định được IP service cần dùng), reliability (làm sao để đảm bảo tính sẵn có giữa 2 services khi contact), transactional messaging (làm sao để thực hiện transaction db), external API (client contanct với service như nào?).
  *   Xử lý giao dịch: sử dụng Saga pattern.
  *   Truy vấn dữ liệu.
  *   Deploy service.
  *   Quản lý các service (log của serivce): health check API, log aggregation,...
  *   Autmated test: Consumer-driven contract test(service đạt được mong muốn của client), consumer-side contract test(client có thể liên lạc với service), service component test.
  *   Thực hiện các pattern obser và discovery tối ưu: Externalized Configuration pattern.
  *   Security.
