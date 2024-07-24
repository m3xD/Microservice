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

## 2. Decomposition pattern:
### 2.1 By business capability:
- Phân chia theo năng lực, nghiệp vụ, những nhiệm vụ của công ty.
- Lợi ích của việc phân chia theo pattern này:
  * Phân chia các service sẽ ổn định nếu nghiệp vụ của công ty ổn định.
  * Có thể phân chia thành nhiều team độc lập với từng service.
  * Các service có liên hệ ít (loosely coupled).
- Ví dụ:
  * Công ty bán hàng thì thường có các nghiệp vụ quản lý hàng hóa, đơn hàng, vận chuyển,...
  * Công ty bảo hiểm quản lý các hợp đồng bảo hiểm, các gói bảo hiểm,...
  * Công ty an ninh mạng quản lý servers, các đối tượng cần bảo vệ,....
### 2.2. Subdomain:
- Định nghĩa bussiness thành một domain lớn.
- Chia domain thành các subdomain, các subdomain cũng gần giống với business capability.
- DDD gọi scope của các subdomain là bounded context, bounded context xác định được tập các service(s).
- Nhược điểm:
  * Có thể phân rã ra thành rất nhiều subdomain, tăng nhiều service rời rạc.
### 2.3. Transaction:
- Trong hệ phân tán, các transaction cần gọi 1 hay nhiều service để hoàn thành. Để giảm thiểu độ trễ, chúng ta gọi các service theo một transaction.
- Lợi ích:
  * Giảm thiểu thời gian chờ đợi.
  * Dữ liệu nhất quán.
- Nhược điểm:
  * Có thể tạo thành một ứng dụng nguyên khối phân tán.
  * Phức tạp hơn.
### 2.4. Team:
- Chia các service ra để cho từng team xây dựng và phát triển.
- Các team đều phải đảm bảo các qui tắc:
  * Nhỏ, chỉ từ 5-9 người.
  * Độc lập với các team khác.
- Lợi ích:
  * Các team hoàn toàn tự chủ mà không cần chờ đợi hay phụ thuộc vào team khác.
  * Từ một code base phức tạp như ứng dụng nguyên khối có thể phân rã thành các code base dễ hiểu hơn.
- Nhược điểm:
  * Khi phát triển các tính năng mới thì yêu cầu có nhiều team hơn hoặc các team phải tách ra độc lập.
### 2.5. Funtional:
- Phân chia hệ thống theo chức năng theo kiểu service hoặc module.
- Mỗi service chỉ đảm nhiệm duy nhất 1 chức năng.
- Ưu điểm:
  * Dễ dàng phân chia khi nghiệp vụ business nhỏ
- Nhược điểm:
  * Không phân chia service rõ ràng khiến cho service có thể xử lý hơn một chức năng.

### 3. Inter-process communication:
### 3.1. Communication style:
- Được phân loại thành 2 chiều khác nhau.
- Loại 1:
  * One-to-one: từng client request chỉ được xử lý bởi một service.
  * One-to-many: từng client request được xử lý bởi nhiều service.
- Loại 2:
  * Synchronous: client gửi request và chờ đợi nhận được response và có thể bị block.
  * Asynchronous: client không block, gửi request và response có thể không cần gửi ngay.
#### Có thể được so sánh bởi bảng sau:
|          | One-to-one | One-to-many | 
| ------------- | ------------- | --------- |
| Synchronous  | Request/Response  | x |
| Asynchronous | Asynchronous reqeust/response or One-way notification  | Publish/subscribe or Publish/async responses |
- Các term có thể được giải thích như sau:
  * Request/Response: Client gửi request lên service và chờ response, trong quá trình đó thì có thể client bị block.
  * Asynchronous reqeust/response: Client gửi request lên service và service không cần trả lời ngay, client không bị block.
  * One-way notification: Client gửi request lên service và không cần nhận phản hồi từ service.
  * Publish/subscribe: Client gửi nhiều request, được đọc bởi một hay nhiều services.
  * Publish/async responses: Client gửi request và chờ khoảng thời gian để nhận phản hồi từ services mong muốn.
### 3.2. Synchronous:
#### 3.2.1. Sử dụng REST:
- Ưu điểm:
  * Đơn giản và dễ làm quen.
  * Có thể kiểm thử HTTP API thông qua trình duyệt, postman,...
  * Hỗ trợ kiểu giao tiếp request/response.
  * Tương thích đa số firewall hiện thời.
  * Hệ thống tương đối đơn giản do không có trung gian để thực hiện request/response.
- Nhược điểm:
  * Chỉ support kiểu giao tiếp request/response.
  * Giảm khả năng sẵn có do không có trung gian đứng ra để buffer request, dẫn tới có thể mất gói tin khi vận chuyển qua đường truyền mạng.
  * Client phải biết được URL của service instance.
  * Lấy nhiều dữ liệu từ nhiều service khác nhau là bài toán khó.
  * Khó trong việc đặt tên các operation có cùng chức năng nhưng khác mục đích (POST).
#### 3.2.2. Sử dụng gRPC
- Sử dụng binary message nên có hiệu năng cao, nhỏ, hiệu quả.
- Sử dụng HTTP/2.
- Ưu điểm:
  * Khắc phục yếu điểm của REST khi có thể thiết kế nhiều API cùng operation nhưng khác mục đích.
  * Hiệu năng tốt, đặc biệt là khi trao đổi các msg có dung lượng lớn.
  * Streaming ở cả client và server.
  * Client hoặc server đều có thể viết bằng các ngôn ngữ lập trình khác nhau mà vẫn có thể gửi request/response.
- Nhược điểm:
  * JavaSciprt client phải thực hiện nhiều thao tác hơn để xử lý msg của gRPC thay vì là REST.
  * Những firewall cũ có thể không tương thích với HTTP/2.
### 3.3. Asynchronous
#### 3.3.1. Message
- Các services sẽ contact với nhau thông qua message.
- Sẽ có thành phần trung gian là message-broker để trao đổi message.
- Một message sẽ bao gồm phần header và phần body:
  * Header: tập hợp các cặp name:value để mô tả dữ liệu cần gửi.
  * Body: là dữ liệu cần, có thể ở dạng text hoặc binary.
- Một số kiểu message:
  * Document: Chỉ chứa dữ liệu cần gửi.
  * Command: chứa operation và parameter để thực hiện hành động nào đó.
  * Event: Các event có thể xảy ra ở phía sender.
#### 3.3.2. Channel:
- Message được trao đổi thông qua channel.
- Mỗi bên sender hay reciever đều có sending port và recieve port để thực hiện push hay pull dữ liệu từ message channel.
- Có 2 kiểu channel:
  * Point-to-point: vận chuyển message đến đúng một consumer đang đọc từ message channel. Command message thường sử dụng loại channel này. Thường là style one-to-one communication.
  * Publish-subcribe: vận chuyển từng message đến từng consumers đang đọc từ message channel. Event message thường sử dụng loại channel này. Thường là style one-to-many communication.
#### 3.3.3. Xây dựng communication style dựa trên message:
##### 3.3.3.1. Request/Response and Async Request/Response:
- Sự khác biệt chính của 2 kiểu communication này là:
  * Request/Response có thể client bị block để chờ response.
  * Async Request/Response không chờ response, client không bị block.
- Message channel sử dụng ở đây là point-to-point.
- Bằng cách exchange các message, chỉ rõ command cần thực hiện và các tham số cần truyền vào đến request channel của service, service xử lý xong thì gửi kết quả về response channel của client.
- Client phải chỉ rõ location response channel bằng cách đính kèm thông tin location vào header của request và message id gửi đi, service xử lý thì đính kèm thêm **correlation id** để match response message với request nào bằng cách so sánh với message id.
##### 3.3.3.2. One-way-notification:
- Client chỉ gửi request đến point-to-point message channel và service lấy chúng xử lý mà không cần phản hồi lại.
##### 3.3.3.3. Publish/subscribe:
- Client publish message lên publish-subscribe channel và được đọc bởi các consumers.
- Thường được sử dụng kiểu message là event, nhận biết sự thay đổi của domain objects.
##### 3.3.3.4. Publish/Async response:
- Kết hợp các thành phần publish/subscribe và request/response.
- Client thực hiện publish message chỉ định rõ reply channel trong header rồi gửi tới publish/subscribe channel. Consumer thực hiện gửi response kèm theo **correlation id** để match với từng request.
### 3.3.4. Message broker:
- Sender sẽ thực hiển đẩy message vào message broker, message broker chuyển message tới receiver.
- Lợi ích:
  * Sender không cần biết location của receiver và các message vẫn được buffer cho đến khi được đọc bởi receiver.
  * Loose coupling: client chỉ cần push request vào đúng message broker mà không cần biết tới service instance nào.
  * Flexible: có thể trao đổi nhiều kiểu message khác nhau.
  * Quá trình giao tiếp rõ ràng.
- Nhược điểm:
  * Có thể bị thắt cổ chai.
  * Quá tải tại điểm nút.
  * Hệ thống trở nên phức tạp hơn
