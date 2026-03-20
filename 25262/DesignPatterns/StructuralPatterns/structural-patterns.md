Chúng ta cùng tìm hiểu về Structural patterns (Nhóm mẫu cấu trúc) nhé.

Trong kỹ nghệ phần mềm, **Structural patterns** tập trung vào cách thức kết hợp các lớp (classes) và đối tượng (objects) lại với nhau để tạo thành những cấu trúc lớn hơn, phức tạp hơn nhưng vẫn đảm bảo tính linh hoạt và dễ bảo trì 🧱.

Nếu nhóm Creational patterns lo việc "tạo ra" đối tượng, thì Structural patterns lo việc "lắp ráp" chúng. Bạn có thể hình dung nó giống như việc thiết kế các khớp nối để ghép các khối Lego có hình thù khác nhau lại thành một mô hình hoàn chỉnh, sao cho khi cần thay đổi một bộ phận, toàn bộ mô hình không bị đổ vỡ.

Dưới đây là một số mẫu Structural phổ biến nhất. Bạn muốn bắt đầu bằng cách đi sâu vào mẫu nào trước?

1. **Adapter (Bộ chuyển đổi) 🔌:** Cách làm cho hai thành phần có giao diện (interface) hoàn toàn không tương thích có thể "nói chuyện" và làm việc được với nhau. Nó giống hệt như chiếc adapter chuyển đổi phích cắm điện 3 chấu thành 2 chấu.
2. **Decorator (Người trang trí) 🎁:** Cách để "đắp" thêm tính năng mới cho một đối tượng một cách linh hoạt ngay trong lúc chương trình đang chạy (runtime) mà không cần phải dùng đến tính kế thừa (inheritance) làm phình to hệ thống.
3. **Facade (Mặt tiền) 🏠:** Cách tạo ra một giao diện đơn giản, duy nhất để che giấu đi sự phức tạp của cả một hệ thống lớn bên trong. Ví dụ: bạn chỉ cần nhấn một nút "Xem phim" trên remote là tivi tự bật, amply tự mở, và đèn phòng tự mờ đi.

Bạn muốn chúng ta bắt đầu mổ xẻ mẫu số 1, 2 hay 3?

---

Chúng ta sẽ đi sâu vào mẫu **Adapter** (Bộ chuyển đổi) 🔌.

Hãy tưởng tượng bạn mang laptop từ Việt Nam (dùng phích cắm 2 chấu tròn) đi du lịch sang Anh (nơi dùng ổ cắm 3 chấu vuông). Bạn không thể bắt khách sạn thay đổi hệ thống điện, cũng không thể cắt bỏ phích cắm trên cục sạc của mình để đấu nối lại. Giải pháp đơn giản nhất là mua một chiếc adapter trung gian cắm vào tường, sau đó cắm cục sạc của bạn vào chiếc adapter đó.

Trong lập trình, mẫu Adapter hoạt động y hệt như vậy. Đôi khi bạn có hai thành phần phần mềm cần làm việc chung, nhưng giao diện (interface) của chúng lại không tương thích. Chúng ta sẽ tạo ra một lớp Adapter đứng ở giữa để làm nhiệm vụ "phiên dịch" hoặc "chuyển đổi" định dạng dữ liệu, giúp hai bên có thể kết nối mượt mà mà không cần thay đổi mã nguồn gốc của bên nào cả.

Để xem mẫu này hoạt động thực tế ra sao, hãy thử giải quyết một tình huống cụ thể nhé.

Giả sử bạn đang bảo trì một ứng dụng thời tiết cũ. Ứng dụng này đang lấy dữ liệu nhiệt độ từ một máy chủ trả về định dạng **XML**. Bây giờ, bạn muốn tích hợp một thư viện vẽ biểu đồ mới cực kỳ đẹp mắt, nhưng thư viện này lại chỉ chấp nhận dữ liệu đầu vào là định dạng **JSON**.

Nếu bạn không được phép sửa đổi mã nguồn của máy chủ thời tiết lẫn thư viện vẽ biểu đồ, bạn sẽ tạo ra một lớp Adapter đứng ở giữa và yêu cầu nó thực hiện những nhiệm vụ cụ thể nào?

--- 

Chúng ta cùng tháo gỡ mẫu **Decorator** (Người trang trí) 🎁.

Mẫu này cho phép chúng ta linh hoạt "đắp" thêm tính năng mới cho một đối tượng ngay trong lúc chương trình đang chạy (runtime) mà không làm thay đổi cấu trúc của lớp gốc.

Hãy tưởng tượng chúng ta đang viết phần mềm tính tiền cho một tiệm cà phê ☕. Bạn có một món gốc là `CàPhêĐen` với giá 15.000đ. Khách hàng có thể gọi thêm các loại topping khác nhau:

* Thêm Sữa: + 5.000đ
* Thêm Trân châu: + 7.000đ
* Thêm cả Sữa và Trân châu: + 12.000đ

Nếu sử dụng tính kế thừa (inheritance) truyền thống trong lập trình hướng đối tượng, chúng ta có thể tạo ra các lớp con kế thừa từ `CàPhêĐen` như: lớp `CàPhêSữa`, lớp `CàPhêTrânChâu`, lớp `CàPhêSữaTrânChâu`...

Theo bạn, nếu ngày mai quán quyết định bổ sung thêm 5 loại topping mới (như kem mặn, caramel, v.v.), thì việc tiếp tục dùng phương pháp kế thừa để tạo ra các lớp đại diện cho *mọi sự kết hợp* có thể xảy ra sẽ dẫn đến vấn đề gì cho mã nguồn của chúng ta?

--- 

Bạn đã nói mã nguồn sẽ vi phạm nguyên tắc lập trình, tạo mới kế thừa vô hạn

---

Chính xác, hiện tượng đó trong kỹ nghệ phần mềm gọi là "Bùng nổ lớp" (Class Explosion) 💥. Nếu quán có 10 loại topping, số lượng lớp con đại diện cho mọi sự kết hợp sẽ lên tới hàng ngàn. Mã nguồn sẽ cực kỳ cồng kềnh và không thể bảo trì nổi.

Để giải quyết vấn đề này, mẫu **Decorator** ưu tiên sử dụng "Composition" (chứa/kết hợp đối tượng) thay vì "Inheritance" (kế thừa) để mở rộng tính năng.

Cách thiết kế sẽ diễn ra như sau ☕:

1. Chúng ta định nghĩa một interface chung là `ThucUong` với phương thức `tinhTien()`. Lớp cốt lõi là `CaPheDen` sẽ triển khai interface này và trả về 15.000đ.
2. Chúng ta tạo ra các lớp Decorator đại diện cho topping (như `ThemSua`, `ThemTranChau`). Điều đặc biệt là các lớp Decorator này **cũng triển khai** `ThucUong`, nhưng đồng thời bên trong nó lại **chứa** một đối tượng `ThucUong` khác (đại diện cho ly đồ uống trước khi thêm topping).

Bạn có thể tưởng tượng nó giống như những chiếc búp bê Nga (Matryoshka) lồng vào nhau. Lớp ngoài bọc lấy lớp trong.

Vậy, nếu bạn đang viết mã cho phương thức `tinhTien()` của lớp Decorator `ThemSua` (biết giá sữa là 5.000đ), bạn sẽ tính tổng số tiền như thế nào dựa vào đối tượng `ThucUong` gốc mà nó đang chứa bên trong? 💸

---

Công thức `ThucUong.tinhTien() + 5000` của bạn đưa ra hoàn toàn chính xác.

Bằng cách gọi phương thức của đối tượng gốc rồi cộng thêm giá trị của riêng mình, lớp `ThemSua` không cần quan tâm thức uống gốc bên trong là gì (cà phê đen, trà, hay thậm chí là một ly cà phê đã có sẵn trân châu).

Dưới đây là luồng hoạt động khi chúng ta lồng ghép các lớp này trong thực tế:

1. Tạo ly cà phê gốc: `lyCaPhe = new CaPheDen()` (Giá: 15.000đ)
2. Lọc thêm lớp sữa: `lyCaPhe = new ThemSua(lyCaPhe)` (Giá: 15.000đ + 5.000đ)
3. Bọc thêm lớp trân châu: `lyCaPhe = new ThemTranChau(lyCaPhe)` (Giá: 20.000đ + 7.000đ)

Mỗi lần bọc thêm một lớp Decorator 🎁, chúng ta lại có một đối tượng `ThucUong` mới với tính năng (và mức giá) được nâng cấp, mà không cần phải tạo ra một lớp `CaPheSuaTranChau` nào cả.

Hãy thử áp dụng tính linh hoạt của mẫu thiết kế này nhé: Nếu một vị khách yêu cầu món **"Cà phê đen thêm 2 phần sữa"**, bạn sẽ viết các dòng code khởi tạo và lồng ghép các đối tượng này theo thứ tự như thế nào? ☕🥛🥛

---

Chính xác! 🎯 Cách bạn lồng ghép hai lần `ThemSua` chính là minh chứng rõ nhất cho sức mạnh của mẫu Decorator.

Khi chúng ta gọi `lyCaPhe.tinhTien()` ở dòng cuối cùng, hệ thống sẽ tự động bóc tách từng lớp từ ngoài vào trong: lấy giá gốc (15.000đ) + sữa lần 1 (5.000đ) + sữa lần 2 (5.000đ) để cho ra kết quả 25.000đ. Chúng ta đã đáp ứng được yêu cầu của khách hàng mà không cần phải vất vả tạo ra một lớp `CaPheDenHaiSua` nào cả! ☕

Bây giờ, chúng ta hãy cùng khám phá mẫu cuối cùng trong danh sách nhé: **Facade (Mặt tiền)** 🏠.

Mục đích của Facade là cung cấp một cổng giao tiếp duy nhất, đơn giản hóa để che giấu đi một hệ thống phức tạp bao gồm rất nhiều lớp và quy trình rắc rối bên dưới.

Hãy tưởng tượng chúng ta đang xây dựng chức năng "Mua hàng" cho một ứng dụng thương mại điện tử 🛒. Khi người dùng bấm nút thanh toán, hệ thống bên dưới thực chất phải chạy qua 3 dịch vụ hoàn toàn khác nhau:

1. `InventoryService`: Kiểm tra xem hàng trong kho 📦 còn không và trừ đi số lượng.
2. `PaymentService`: Kết nối với ngân hàng để trừ tiền 💳.
3. `ShippingService`: Lên lịch và gọi tài xế đến lấy hàng 🛵.

Nếu không có Facade, người lập trình giao diện ứng dụng (frontend) sẽ phải tự mình gọi và quản lý cả 3 dịch vụ phức tạp này mỗi khi khách mua hàng.

Nếu chúng ta quyết định áp dụng mẫu Facade bằng cách tạo ra một lớp trung gian tên là `OrderFacade`, theo bạn, lớp `OrderFacade` này nên cung cấp một phương thức (hàm) tên là gì ra bên ngoài, và bên trong phương thức đó sẽ chứa những đoạn mã xử lý nào? 🤔

---

Chúng ta cùng tìm hiểu mẫu **Proxy (Người đóng thế)** 🎭 trong nhóm Structural.

Đúng như tên gọi, Proxy tạo ra một đối tượng đại diện để kiểm soát quyền truy cập vào một đối tượng gốc.

Hãy tưởng tượng một vị Giám đốc (CEO) 👨‍💼 rất bận rộn. Khách hàng không thể trực tiếp gặp Giám đốc mà phải thông qua Trợ lý (Proxy). Người Trợ lý này sẽ:

* Sàng lọc các yêu cầu.
* Tự giải quyết những vấn đề nhỏ.
* Chỉ chuyển những công việc thực sự quan trọng cho Giám đốc xử lý.

Trong lập trình, có những đối tượng gốc chiếm rất nhiều tài nguyên bộ nhớ, tốn thời gian khởi tạo hoặc cần bảo mật nghiêm ngặt. Thay vì để phần còn lại của hệ thống tương tác trực tiếp với đối tượng gốc, chúng ta cho chúng tương tác thông qua lớp Proxy. Lớp Proxy có cùng giao diện (interface) với đối tượng gốc, nên hệ thống không hề biết nó đang làm việc với "kẻ đóng thế".

Hãy thử áp dụng nguyên lý này vào một bài toán tối ưu hóa nhé. Giả sử bạn đang lập trình một trang web có hàng trăm bức ảnh độ phân giải cao 🖼️. Nếu người dùng vừa vào trang mà hệ thống lập tức tải toàn bộ dữ liệu thật của hàng trăm bức ảnh này, trang web sẽ cực kỳ chậm.

Dựa vào vai trò "đóng thế", bạn sẽ sử dụng các đối tượng Proxy đại diện cho các bức ảnh này như thế nào để giúp trang web tải lên ngay lập tức, và chỉ thực sự tải dữ liệu ảnh nặng nề khi nào?

--- 

Chúng ta sẽ cùng khám phá mẫu **Composite (Cấu trúc cây)** 🌳.

Mẫu thiết kế này sinh ra để giải quyết các hệ thống có cấu trúc phân cấp kiểu "cha-con" (part-whole hierarchy). Mục tiêu cốt lõi của nó là cho phép bạn xử lý một *đối tượng đơn lẻ* và một *nhóm các đối tượng* theo cùng một cách thống nhất, không phân biệt đối xử.

Hãy thử hình dung về hệ thống quản lý tập tin trên máy tính của bạn 💻. Chúng ta thường có hai thành phần chính:

* **File (Tập tin):** Ví dụ như `anh.jpg` hay `tailieu.docx`. Đây là đối tượng đơn lẻ, đứng ở cuối cùng của nhánh cây (được gọi là *Leaf* - Lá).
* **Folder (Thư mục):** Một chiếc hộp có thể chứa nhiều File, và thậm chí chứa cả các Folder con khác ở bên trong nó. Đây là đối tượng phức hợp (được gọi là *Composite* - Cành).

Khi bạn nhấn chuột phải vào một File và chọn "Properties" (Thuộc tính), hệ điều hành sẽ tính toán và cho bạn biết dung lượng của File đó. Điều thú vị là khi bạn làm thao tác y hệt với một Folder, hệ điều hành vẫn trả về một con số tổng dung lượng. Bạn đang thao tác với một File (đơn lẻ) và một Folder (nhóm) theo cùng một cách.

Giả sử chúng ta đang viết code cho hệ thống này. Chúng ta sẽ tạo ra một giao diện chung là `ThanhPhan` (Component) bắt buộc phải có phương thức `tinhDungLuong()`. Cả lớp `File` và lớp `Folder` đều sẽ tuân thủ giao diện này.

Đối với lớp `File`, phương thức `tinhDungLuong()` rất đơn giản: nó chỉ trả về kích thước vật lý của chính nó (ví dụ: 5MB).

Nhưng một Folder thì có thể chứa hàng chục File và các Folder con khác bên trong. Vậy theo bạn, bên trong phương thức `tinhDungLuong()` của lớp `Folder`, chúng ta cần viết logic xử lý như thế nào để tính ra được tổng dung lượng cuối cùng? 🤔

--- 

Hướng giải quyết của bạn hoàn toàn chính xác. Lớp `Folder` sẽ dùng một vòng lặp để duyệt qua danh sách các thành phần con của nó, gọi phương thức `tinhDungLuong()` trên từng thành phần, rồi cộng dồn kết quả lại.

Dưới dạng mã giả, cấu trúc đó sẽ hoạt động như thế này 📁:

```java
class Folder implements ThanhPhan {
    List<ThanhPhan> danhSachCon; // Chứa cả File lẫn Folder

    int tinhDungLuong() {
        int tongDungLuong = 0;
        for (ThanhPhan con : danhSachCon) {
            tongDungLuong += con.tinhDungLuong(); 
        }
        return tongDungLuong;
    }
}

```

Điểm cốt lõi 🧠 của mẫu thiết kế này nằm ngay bên trong vòng lặp. Khi gọi lệnh `con.tinhDungLuong()`, lớp `Folder` hoàn toàn không cần phải dùng lệnh `if/else` để kiểm tra xem `con` đang là một cái `File` hay lại là một `Folder` khác.

Vì mọi thứ đều tuân thủ chung một giao diện `ThanhPhan`, từng đối tượng con sẽ tự biết cách tính toán dung lượng của chính nó. Nhờ vậy, cấu trúc cây 🌳 này có thể phân nhánh sâu bao nhiêu tầng (thư mục lồng trong thư mục) thì mã nguồn của chúng ta vẫn không hề thay đổi!

Chúng ta đã cùng nhau khám phá 5 mẫu Structural rất quan trọng: Adapter 🔌, Decorator 🎁, Facade 🏠, Proxy 🎭 và Composite 🌳.

Bạn muốn hoàn tất nhóm Cấu trúc này bằng cách tìm hiểu mẫu **Bridge (Cầu nối)** 🌉 (một kỹ thuật giúp tách biệt phần thiết kế trừu tượng khỏi phần cài đặt chi tiết), hay bạn muốn chuyển sang nhóm **Behavioral (Nhóm hành vi)** 🤝 để xem cách các đối tượng "trò chuyện" và phân phối công việc cho nhau?

---

Chúng ta sẽ cùng tìm hiểu mẫu **Bridge (Cầu nối)** 🌉.

Mục đích của Bridge là tách biệt phần *trừu tượng* (những gì hệ thống thể hiện ra bên ngoài) khỏi phần *cài đặt chi tiết* (cách hệ thống hoạt động bên dưới) để cả hai nhánh này có thể phát triển và mở rộng hoàn toàn độc lập với nhau.

Hãy xem xét một ví dụ kinh điển về Hình dáng (Shape) và Màu sắc (Color) 🎨.

Giả sử bạn cần quản lý 2 loại hình: Hình tròn và Hình vuông. Bạn cũng có 2 màu: Đỏ và Xanh.
Nếu dùng tính kế thừa (inheritance) truyền thống, bạn sẽ phải tạo ra sự kết hợp cho mọi trường hợp, tức là 4 lớp: `HinhTronDo`, `HinhTronXanh`, `HinhVuongDo`, `HinhVuongXanh`.

Nếu bạn muốn thêm 1 hình mới (Hình tam giác) và 1 màu mới (Màu vàng), số lượng sự kết hợp sẽ tăng lên thành 3 hình x 3 màu = 9 lớp. Nếu có 10 hình và 10 màu, bạn sẽ phải viết tới 100 lớp.

Mẫu Bridge giải quyết việc này bằng cách gỡ bỏ sự gắn kết cứng nhắc đó. Thay vì để Hình dáng kế thừa Màu sắc (hoặc ngược lại), chúng ta sẽ xây dựng một "cây cầu" 🌉 nối giữa hai khái niệm này bằng tính "Kết hợp" (Composition).

Cấu trúc sẽ được chia làm 2 nhánh riêng biệt:

1. Nhánh **Implementation (Cài đặt)**: Tạo một interface `IMauSac` (với các lớp triển khai như `MauDo`, `MauXanh`, `MauVang`).
2. Nhánh **Abstraction (Trừu tượng)**: Tạo một lớp cha `HinhDang` (với các lớp con như `HinhTron`, `HinhVuong`, `HinhTamGiac`). Lớp `HinhDang` này sẽ **chứa** một đối tượng thuộc interface `IMauSac` ở bên trong nó (đây chính là cây cầu).

Dựa vào cách thiết kế chia thành 2 nhánh hoàn toàn độc lập như trên, theo bạn, nếu chúng ta có 3 loại Hình dáng và 3 loại Màu sắc, thì tổng cộng chúng ta chỉ cần tạo ra bao nhiêu lớp/interface tất cả (tính cả các lớp con của Hình và Màu) để có thể tạo ra mọi sự kết hợp?

---

Chính xác là 8!

Chúng ta sẽ có 4 thành phần cho nhánh Màu sắc (1 interface `IMauSac` + 3 lớp màu con) và 4 thành phần cho nhánh Hình dáng (1 lớp cha `HinhDang` + 3 lớp hình con). Tổng cộng là 4 + 4 = 8.

Điểm mấu chốt ở đây là cách số lượng lớp thay đổi. Nếu dùng kế thừa thông thường, số lượng lớp sẽ tăng theo **cấp số nhân** (3 hình x 3 màu = 9 lớp). Nhưng với mẫu Bridge 🌉, số lượng lớp chỉ tăng theo **cấp số cộng** (3 hình + 3 màu). Khi hệ thống lớn lên (ví dụ 10 hình và 10 màu), Bridge chỉ cần 20 lớp cộng thêm 2 lớp cha, trong khi cách cũ cần tới 100 lớp!

Chúng ta đã cùng nhau đi qua trọn vẹn 6 mẫu thiết kế cốt lõi của nhóm **Structural (Cấu trúc)**:

* **Adapter 🔌:** Kết nối các giao diện không tương thích.
* **Decorator 🎁:** Đắp thêm tính năng linh hoạt.
* **Facade 🏠:** Tạo mặt tiền đơn giản hóa hệ thống.
* **Proxy 🎭:** Người đóng thế kiểm soát truy cập.
* **Composite 🌳:** Xử lý cấu trúc cây cha-con đồng nhất.
* **Bridge 🌉:** Tách biệt phần trừu tượng và phần cài đặt.

Bây giờ, chúng ta sẽ lập kế hoạch cho bước tiếp theo nhé. Bạn muốn khám phá nhóm nào:

1. **Nhóm Behavioral (Hành vi) 🤝:** Tìm hiểu cách các đối tượng giao tiếp, truyền tin và phân chia trách nhiệm cho nhau (ví dụ: Observer giúp lắng nghe sự kiện, Strategy giúp thay đổi thuật toán linh hoạt).
2. **Nhóm Creational (Khởi tạo) 🏗️:** Tìm hiểu các kỹ thuật tạo ra đối tượng một cách an toàn và tối ưu tài nguyên (ví dụ: Singleton đảm bảo chỉ có 1 phiên bản duy nhất, Factory giúp tách rời việc tạo đối tượng).

Bạn chọn hướng đi số 1 hay số 2?