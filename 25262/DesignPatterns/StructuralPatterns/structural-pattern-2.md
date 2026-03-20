Mẫu thiết kế (Design Pattern) là gì?
Trong kỹ thuật phần mềm, Design Pattern (mẫu thiết kế) là một giải pháp tổng thể, có thể tái sử dụng để giải quyết các vấn đề thường xuyên lặp lại trong quá trình thiết kế và xây dựng phần mềm.
Nó không phải là một đoạn mã (code) hoàn chỉnh để bạn sao chép và dán trực tiếp vào dự án. Thay vào đó, nó giống như một bản thiết kế (blueprint) hoặc một khuôn mẫu logic, cung cấp cách tiếp cận tối ưu để giải quyết một vấn đề cụ thể trong một ngữ cảnh nhất định.
Các nhóm Mẫu thiết kế chính
Dựa trên cuốn sách kinh điển của nhóm Gang of Four (GoF), 23 mẫu thiết kế cốt lõi được chia thành 3 nhóm chính dựa trên mục đích sử dụng:
 * Nhóm Khởi tạo (Creational Patterns)
   * Mục đích: Cung cấp các cơ chế khởi tạo đối tượng linh hoạt và an toàn, giúp giấu đi logic tạo đối tượng phức tạp thay vì khởi tạo trực tiếp bằng từ khóa new.
   * Các mẫu tiêu biểu: * Singleton: Đảm bảo một class chỉ có duy nhất một instance (thể hiện) và cung cấp một điểm truy cập toàn cầu tới nó.
     * Factory Method: Cung cấp một interface để tạo đối tượng, nhưng để các class con quyết định class nào sẽ được khởi tạo.
     * Builder: Tách biệt quá trình xây dựng một đối tượng phức tạp khỏi biểu diễn của nó.
 * Nhóm Cấu trúc (Structural Patterns)
   * Mục đích: Giải quyết cách lắp ráp các class và object thành những cấu trúc lớn hơn, phức tạp hơn, trong khi vẫn giữ cho các cấu trúc này linh hoạt và hiệu quả.
   * Các mẫu tiêu biểu:
     * Adapter: Cho phép các interface không tương thích có thể làm việc cùng nhau (đóng vai trò như một bộ chuyển đổi).
     * Decorator: Cho phép thêm các hành vi mới vào đối tượng một cách linh hoạt (động) mà không làm thay đổi cấu trúc của class.
     * Facade: Cung cấp một interface đơn giản, thống nhất để giao tiếp với một hệ thống phức tạp gồm nhiều thành phần.
 * Nhóm Hành vi (Behavioral Patterns)
   * Mục đích: Tập trung vào việc phân công trách nhiệm và cách các đối tượng giao tiếp, tương tác với nhau trong hệ thống.
   * Các mẫu tiêu biểu:
     * Observer: Định nghĩa mối phụ thuộc một-nhiều, khi một đối tượng thay đổi trạng thái, tất cả các đối tượng phụ thuộc sẽ được thông báo tự động (thường dùng trong xử lý sự kiện).
     * Strategy: Đóng gói các thuật toán vào các class riêng biệt để chúng có thể thay đổi hoặc thay thế lẫn nhau khi chương trình đang chạy (runtime).
     * Command: Đóng gói một yêu cầu dưới dạng một đối tượng, giúp bạn có thể truyền yêu cầu như các tham số, xếp hàng các yêu cầu hoặc ghi log chúng.
Bản chất cơ bản của Design Pattern
 * Là kinh nghiệm được đúc kết (Best Practices): Nó là bộ giải pháp đã được chứng minh hiệu quả qua thực tiễn từ hàng ngàn kỹ sư phần mềm đi trước. Sử dụng nó giúp bạn tránh việc "phát minh lại bánh xe" và hạn chế các lỗi thiết kế tiềm ẩn.
 * Là một ngôn ngữ giao tiếp chung (Common Vocabulary): Thay vì giải thích dài dòng cách hệ thống hoạt động, các lập trình viên chỉ cần dùng tên của pattern. Khi bạn nói "Tôi dùng Factory kết hợp với Singleton ở module này", một lập trình viên khác sẽ lập tức hiểu kiến trúc và ý đồ của bạn.
 * Thúc đẩy tính mở rộng và bảo trì (Maintainability & Extensibility): Design Pattern bản chất là việc áp dụng triệt để các nguyên lý thiết kế hướng đối tượng tốt (như SOLID). Nó khuyến khích việc lập trình hướng tới Interface thay vì Implementation, và ưu tiên Composition (chứa ghép) hơn Inheritance (kế thừa), giúp code dễ bảo trì, dễ test và dễ mở rộng.
Bạn có muốn tìm hiểu sâu hơn về một Pattern cụ thể nào đó (ví dụ Singleton hay Observer) kèm theo cách triển khai thực tế bằng mã nguồn (như C++) không?
---
Nhóm Khởi tạo (Creational Patterns) tập trung vào việc giải quyết bài toán: Làm thế nào để khởi tạo đối tượng (object) một cách linh hoạt, an toàn và dễ tái sử dụng nhất?
Trong lập trình căn bản, chúng ta thường tạo đối tượng bằng từ khóa new (ví dụ: Car myCar = new Car()). Tuy nhiên, khi hệ thống phình to, việc rải rác từ khóa new khắp nơi sẽ làm code bị "trói chặt" (tight coupling) vào các class cụ thể. Nếu bạn muốn đổi từ Car sang Truck, bạn phải đi tìm và sửa code ở vô số chỗ.
Nhóm Khởi tạo giải quyết vấn đề này bằng cách che giấu đi logic tạo đối tượng. Bạn có thể hình dung nó giống như việc bạn vào nhà hàng gọi món: bạn chỉ cần yêu cầu "Cho tôi một tô phở" (thông qua interface/hàm khởi tạo), còn việc chọn nguyên liệu, nấu nước dùng thế nào là việc của đầu bếp (design pattern che giấu), bạn không cần tự tay làm.
Dưới đây là 5 mẫu thiết kế kinh điển thuộc nhóm Khởi tạo (theo chuẩn GoF):
1. Singleton (Độc thể)
 * Mục đích: Đảm bảo rằng một class chỉ có duy nhất một phiên bản (instance) được tạo ra trong suốt vòng đời của ứng dụng, đồng thời cung cấp một điểm truy cập toàn cục (global access point) để lấy phiên bản đó.
 * Ví dụ thực tế: Một quốc gia chỉ có thể có một Tổng thống đương nhiệm. Bất kể ai cần báo cáo công việc cho Tổng thống, họ đều phải gặp đúng người đó, chứ không thể "tạo ra" một Tổng thống mới.
 * Ứng dụng trong code: Rất phổ biến khi quản lý các tài nguyên dùng chung và đắt đỏ như: Bộ kết nối cơ sở dữ liệu (Database Connection Pool), bộ ghi log (Logger), hoặc class lưu trữ cấu hình hệ thống (Configuration/Settings).
2. Factory Method (Phương thức nhà máy)
 * Mục đích: Cung cấp một giao diện (interface) hoặc một phương thức (method) để tạo ra đối tượng, nhưng giao quyền quyết định khởi tạo class nào cho các class con (subclasses).
 * Ví dụ thực tế: Một công ty logistics ban đầu chỉ giao hàng bằng XeTải. Sau này họ mở rộng giao hàng bằng đường biển với TàuThủy. Thay vì sửa lại toàn bộ hệ thống quản lý, họ tạo ra một "Nhà máy" chuyên cấp phương tiện. Tùy vào loại đơn hàng, nhà máy sẽ tự động xuất xưởng XeTải hoặc TàuThủy mà người điều phối không cần quan tâm chi tiết.
 * Ứng dụng trong code: Khi bạn không biết trước chính xác loại đối tượng nào sẽ cần được tạo ra cho đến khi chương trình chạy, ví dụ: một phần mềm đọc tài liệu có thể tạo ra PDFReader, WordReader hoặc ExcelReader tùy vào đuôi file người dùng tải lên.
3. Abstract Factory (Nhà máy trừu tượng)
 * Mục đích: Cung cấp một giao diện để tạo ra các họ đối tượng (families of objects) có liên quan hoặc phụ thuộc lẫn nhau, mà không cần chỉ định rõ các lớp cụ thể của chúng. (Đây là phiên bản nâng cấp của Factory Method).
 * Ví dụ thực tế: Một xưởng sản xuất nội thất. Bạn yêu cầu bộ bàn ghế theo phong cách "Hiện đại", xưởng sẽ trả về Bàn hiện đại và Ghế hiện đại. Nếu bạn chọn phong cách "Cổ điển", xưởng trả về Bàn cổ điển và Ghế cổ điển. Mẫu này đảm bảo bạn không bao giờ nhận được một cái bàn hiện đại đi kèm với một cái ghế cổ điển.
 * Ứng dụng trong code: Rất hữu ích khi phát triển phần mềm đa nền tảng. Ví dụ: hệ thống cần render giao diện (UI) cho cả Windows và macOS. WindowsFactory sẽ tạo ra WinButton và WinCheckbox, còn MacFactory tạo ra MacButton và MacCheckbox.
4. Builder (Người xây dựng)
 * Mục đích: Tách biệt quá trình khởi tạo một đối tượng phức tạp khỏi lớp của nó, cho phép bạn tạo ra các đối tượng có cấu hình khác nhau thông qua cùng một quy trình xây dựng.
 * Ví dụ thực tế: Đặt một chiếc bánh Pizza hoặc lắp ráp một chiếc máy tính PC. Bạn có một quy trình chung, nhưng ở mỗi bước, bạn có thể tùy chọn khác nhau (chọn đế bánh mỏng/dày, thêm phô mai, chọn CPU Core i7/i9, chọn RAM 16GB/32GB). Kết quả là cùng một nhà máy lắp ráp nhưng cho ra hàng ngàn cấu hình sản phẩm khác nhau.
 * Ứng dụng trong code: Khi một object cần quá nhiều tham số truyền vào khi khởi tạo (anti-pattern gọi là Telescoping Constructor - ví dụ hàm khởi tạo có tới 10 tham số, rất dễ truyền nhầm vị trí). Builder giúp bạn gọi từng hàm rõ ràng như setCPU(), setRAM(), rồi cuối cùng gọi build().
5. Prototype (Nguyên mẫu)
 * Mục đích: Cho phép sao chép (clone) các đối tượng hiện có để tạo ra đối tượng mới mà không làm code của bạn bị phụ thuộc vào các lớp của chúng.
 * Ví dụ thực tế: Quá trình phân bào (từ một tế bào nhân đôi thành hai tế bào giống hệt nhau). Hoặc đơn giản hơn là chức năng Copy-Paste một đoạn văn bản phức tạp đã được định dạng sẵn trong Word.
 * Ứng dụng trong code: Cực kỳ hữu dụng khi việc khởi tạo một đối tượng từ đầu tốn rất nhiều tài nguyên (chẳng hạn như phải chọc vào Database hoặc gọi API để lấy dữ liệu). Thay vì tạo mới, bạn lấy một "nguyên mẫu" đã có dữ liệu sẵn trong RAM và "clone" nó ra, sau đó chỉ việc sửa đổi một vài thông số nhỏ. Rất hay dùng trong việc sinh ra quái vật (monster) liên tục trong lập trình Game.
Bạn có muốn xem thử cách triển khai một mẫu Khởi tạo (ví dụ như Singleton hoặc Factory Method) bằng mã nguồn C++ để thấy rõ hơn cách chúng hoạt động trong thực tế không?
---
Nhóm Cấu trúc (Structural Patterns) tập trung vào việc giải quyết bài toán: Làm thế nào để kết hợp các lớp (classes) và các đối tượng (objects) lại với nhau thành những cấu trúc lớn hơn, phức tạp hơn, nhưng vẫn đảm bảo tính linh hoạt, dễ thay đổi và hiệu quả?
Bạn có thể hình dung nhóm này giống như kỹ thuật xếp hình Lego. Thay vì tạo ra một khối nhựa khổng lồ (khó sửa chữa và tái sử dụng), bạn lắp ghép các mảnh nhỏ lại với nhau theo những cách thông minh để tạo ra một cấu trúc vững chắc.
Dưới đây là 7 mẫu thiết kế kinh điển thuộc nhóm Cấu trúc (theo chuẩn GoF), được giải thích cụ thể:
1. Adapter (Người chuyển đổi)
 * Mục đích: Giúp các lớp có giao diện (interface) không tương thích có thể làm việc cùng nhau. Nó "bọc" một đối tượng lại để chuyển đổi giao diện của đối tượng đó thành một giao diện mà client mong đợi.
 * Ví dụ thực tế: Bạn có một phích cắm điện 3 chấu (hàng Mỹ) nhưng ổ cắm trên tường ở Việt Nam chỉ có 2 lỗ. Bạn cần một cục adapter chuyển đổi ở giữa.
 * Ứng dụng trong code: Khi bạn muốn tích hợp một thư viện của bên thứ 3 (third-party) hoặc một module cũ (legacy code) vào hệ thống hiện tại, nhưng tên hàm và kiểu dữ liệu của chúng không khớp với giao diện hệ thống của bạn.
2. Bridge (Cây cầu)
 * Mục đích: Tách biệt phần trừu tượng (Abstraction) ra khỏi phần thực thi (Implementation) của nó, để cả hai có thể thay đổi và phát triển độc lập với nhau. Nó giúp giải quyết vấn đề "bùng nổ số lượng lớp con".
 * Ví dụ thực tế: Hãy tưởng tượng bạn có 2 loại thiết bị (TV, Radio) và 2 loại điều khiển từ xa (Remote cơ bản, Remote thông minh). Thay vì tạo ra 4 lớp (TVRemoteCoBan, TVRemoteThongMinh, RadioRemoteCoBan...), bạn tách Remote (Abstraction) và Thiết bị (Implementation) ra riêng. Remote sẽ chứa một tham chiếu đến Thiết bị.
 * Ứng dụng trong code: Hữu ích khi hệ thống của bạn cần hỗ trợ đa nền tảng (ví dụ: giao diện UI chạy trên cả Windows và macOS) hoặc đa cơ sở dữ liệu.
3. Composite (Hợp thể)
 * Mục đích: Cho phép bạn tổ chức các đối tượng theo cấu trúc cây (tree) để biểu diễn hệ thống phân cấp "toàn thể - bộ phận". Điểm mấu chốt là nó cho phép client xử lý các đối tượng đơn lẻ và các nhóm đối tượng (compositions) theo cùng một cách thống nhất.
 * Ví dụ thực tế: Cấu trúc thư mục trên máy tính. Một Thư mục có thể chứa các Tập tin (File) đơn lẻ hoặc chứa các Thư mục khác. Dù là bấm xóa một File hay xóa một Thư mục gốc, hệ điều hành đều thực hiện lệnh "xóa" tương tự nhau.
 * Ứng dụng trong code: Xây dựng giao diện người dùng (UI). Một Window chứa các Panel, Panel chứa các Button hoặc TextBox. Tất cả đều kế thừa chung một interface UIComponent có hàm draw().
4. Decorator (Người trang trí)
 * Mục đích: Cho phép gán thêm các trách nhiệm/hành vi mới cho một đối tượng tại thời điểm chạy (runtime) mà không làm thay đổi cấu trúc của lớp đó (thay thế linh hoạt cho việc dùng kế thừa - inheritance).
 * Ví dụ thực tế: Bạn đặt một cốc trà sữa. Sau đó bạn "decorate" (thêm) trân châu, rồi thêm thạch, thêm kem cheese. Cốc trà sữa ban đầu được bọc qua từng lớp topping nhưng bản chất vẫn là cốc đồ uống đó, chỉ là giá tiền và hương vị được cộng dồn lên.
 * Ứng dụng trong code: Thêm tính năng nén (compression) hoặc mã hóa (encryption) vào một luồng đọc/ghi dữ liệu (data stream) một cách tự do, hoặc thêm thanh cuộn (scrollbar) vào một cửa sổ UI.
5. Facade (Mặt tiền)
 * Mục đích: Cung cấp một giao diện đơn giản, duy nhất và cấp cao hơn để che giấu sự phức tạp của một hệ thống con (subsystem) gồm nhiều lớp bên trong.
 * Ví dụ thực tế: Bộ phận tổng đài viên (Customer Service). Thay vì bạn phải tự chạy đi tìm bộ phận kho, bộ phận vận chuyển, bộ phận kế toán để xử lý một đơn hàng lỗi, bạn chỉ cần gọi tổng đài viên (Facade), họ sẽ làm việc với các bộ phận kia giúp bạn.
 * Ứng dụng trong code: Bạn sử dụng một thư viện xử lý Video rất phức tạp (cần cấu hình codec, audio, độ phân giải...). Bạn tạo ra một class VideoConverter có đúng một hàm convertVideo(file, format) để cả dự án gọi đến, che đi hàng chục bước setup rườm rà bên dưới.
6. Flyweight (Hạng ruồi)
 * Mục đích: Tối ưu hóa việc sử dụng bộ nhớ (RAM) bằng cách chia sẻ chung trạng thái (state) giữa các đối tượng thay vì mỗi đối tượng lưu trữ toàn bộ dữ liệu của riêng nó.
 * Ví dụ thực tế: Thiết kế game trồng rừng. Nếu bạn có 1 triệu cái cây, việc tạo 1 triệu object chứa đầy đủ lưới 3D (mesh) và kết cấu (texture) sẽ làm tràn RAM. Thay vào đó, bạn tạo 1 object TreeModel duy nhất, và 1 triệu cây kia chỉ lưu trữ tọa độ (X, Y) tham chiếu đến model đó.
 * Ứng dụng trong code: Caching, xử lý văn bản (ví dụ: mỗi ký tự trong Word không tự lưu font chữ, mà tham chiếu đến một đối tượng font chung), phát triển game (Particle systems).
7. Proxy (Người đại diện)
 * Mục đích: Cung cấp một đối tượng thế thân/đại diện cho một đối tượng khác để kiểm soát quyền truy cập vào đối tượng gốc.
 * Ví dụ thực tế: Thẻ tín dụng là một Proxy cho tài khoản ngân hàng của bạn. Nó cung cấp một cách truy cập giống hệt như tiền mặt nhưng an toàn hơn và có kiểm soát.
 * Ứng dụng trong code: * Virtual Proxy: Trì hoãn việc tải một đối tượng quá nặng (ví dụ: ảnh độ phân giải cao) cho đến khi thực sự cần hiển thị.
   * Protection Proxy: Kiểm tra quyền (Authorization) xem người dùng có được phép gọi hàm của đối tượng thực hay không.
   * Remote Proxy: Đại diện cục bộ cho một đối tượng đang nằm ở server khác (như gRPC, REST).
Bạn có muốn xem thử cách triển khai một trong các mẫu này (ví dụ như Adapter hay Facade) bằng ngôn ngữ C++ để thấy rõ hơn cách chúng hoạt động trong mã nguồn không?
