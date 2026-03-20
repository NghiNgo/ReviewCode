Chào bạn. Hãy cùng đi sâu vào **Creational Patterns** (Các mẫu khởi tạo).

Trong lập trình hướng đối tượng, việc sử dụng trực tiếp toán tử `new` để tạo đối tượng ở khắp mọi nơi có thể khiến mã nguồn trở nên cứng nhắc và khó kiểm soát. **Creational Patterns** giải quyết vấn đề này bằng cách "tách rời" (decouple) quá trình khởi tạo đối tượng ra khỏi hệ thống sử dụng chúng.

Hãy tưởng tượng bạn đi ăn nhà hàng: Bạn chỉ cần gọi món theo tên (Interface/Abstract), chứ không cần chạy xuống bếp để tự tay nhào bột hay nướng thịt (Implementation). Nhà bếp chính là nơi áp dụng các mẫu khởi tạo để đưa ra đúng món bạn cần.

Để bắt đầu, chúng ta có thể tiếp cận theo 3 hướng sau, bạn muốn chọn hướng nào?

1. **Nhóm Factory (Factory Method & Abstract Factory):** 🏭
Tìm hiểu cách tạo ra một đối tượng (hoặc một họ các đối tượng liên quan) mà không cần chỉ định chính xác lớp cụ thể của chúng. Đây là nhóm phổ biến nhất.
2. **Xây dựng sự phức tạp (Builder & Prototype):** 🧩
Tập trung vào việc tạo ra các đối tượng có cấu trúc rắc rối (nhiều bước khởi tạo) hoặc sao chép (clone) các đối tượng có sẵn để tiết kiệm tài nguyên.
3. **Kẻ độc nhất (Singleton):** 1️⃣
Khám phá mẫu thiết kế đảm bảo một lớp chỉ có duy nhất một thể hiện (instance) trong suốt vòng đời ứng dụng (thường dùng cho kết nối database hoặc cấu hình).

---

Tuyệt vời. **Singleton** (Kẻ độc nhất) là một lựa chọn thú vị để bắt đầu. Nó thường được xem là mẫu thiết kế đơn giản nhất, nhưng cũng dễ bị lạm dụng nhất.

Hãy bắt đầu với ý tưởng cốt lõi.

### 1️⃣ Khái niệm Singleton

Đúng như tên gọi, **Singleton** đảm bảo rằng một class chỉ có **duy nhất một thể hiện** (instance) tồn tại trong suốt vòng đời của ứng dụng, và cung cấp một điểm truy cập toàn cục (global access point) để bất kỳ đâu cũng có thể gọi tới nó.

**Ví dụ đời thực:**
Hãy tưởng tượng **Thủ tướng** của một quốc gia.

* Dù có bao nhiêu người dân muốn gặp, thì cũng chỉ có **một** Thủ tướng duy nhất tại một thời điểm.
* Chúng ta không thể tùy tiện "tạo mới" (new) một Thủ tướng khác khi đang có người đương nhiệm được.

Trong lập trình, Singleton thường được dùng cho:

* **Kết nối Cơ sở dữ liệu (Database Connection):** Để tránh tạo quá nhiều kết nối làm tốn tài nguyên.
* **Cấu hình (Configuration Manager):** Đọc file settings một lần và dùng chung cho toàn bộ app.

---

Bây giờ, hãy thử tư duy về cách hiện thực hóa nó (implementation).

Thông thường, để tạo một đối tượng, chúng ta hay dùng từ khóa `new` (ví dụ: `Student s = new Student();`). Nếu để `public`, ai cũng có thể gọi `new` và tạo ra hàng tá đối tượng khác nhau.

Theo bạn, làm thế nào để chúng ta **ngăn chặn** các class khác tự ý gọi `new` để tạo ra instance mới của class Singleton?

---

chỉ có thể tạo instance bên trong class Singleton

---

Chính xác! Đó là mấu chốt. Chúng ta phải "giấu" khả năng tạo đối tượng đi, chỉ cho phép lớp đó tự tạo ra chính nó.

Thông thường, constructor (hàm khởi tạo) mặc định sẽ là `public`, cho phép bất kỳ ai cũng có thể gọi `new`.

Vậy theo bạn, chúng ta cần thay đổi phạm vi truy cập (access modifier) của **constructor** thành gì để ngăn chặn việc gọi `new` từ bên ngoài (ngoại trừ chính class đó)?

---

thay đổi thành private


---

Chính xác! 🎯

Khi bạn đặt constructor là `private`, "cánh cửa" để tạo đối tượng từ bên ngoài bằng từ khóa `new` đã bị khóa lại.

Nhưng điều này dẫn đến một vấn đề thú vị: Nếu bên ngoài không thể `new` được, thì làm sao họ lấy được cái instance duy nhất đó để dùng?

Chúng ta sẽ cần một phương thức **public** đặc biệt (thường tên là `getInstance()`) để "tuồn" cái instance đó ra ngoài. Nhưng trước khi viết hàm đó, chúng ta cần một chỗ để **lưu trữ** instance duy nhất này bên trong class.

Theo bạn, biến để chứa instance này cần phải có từ khóa đặc biệt gì để nó tồn tại độc lập, thuộc về chính class đó mà không cần khởi tạo đối tượng mới có? (Gợi ý: Từ khóa này giúp biến được chia sẻ chung cho tất cả).

---

từ khóa static

---

Chính xác hoàn toàn! 🌟

Bạn đã nắm được chìa khóa quan trọng nhất.

Khi chúng ta khai báo biến `instance` là **`static`**, nó sẽ thuộc về **lớp (class)** chứ không thuộc về một đối tượng cụ thể nào cả. Điều này có nghĩa là chỉ có **một bản sao duy nhất** của biến này tồn tại trong bộ nhớ, bất kể ứng dụng chạy bao lâu. Rất phù hợp với mục tiêu "độc nhất" của chúng ta.

Hiện tại, cấu trúc lớp Singleton của chúng ta trông sơ bộ như thế này:

```java
public class ThuTuong {
    // 1. Biến static để giữ thể hiện duy nhất
    private static ThuTuong instance;

    // 2. Constructor private để ngăn chặn 'new' từ bên ngoài
    private ThuTuong() { }
}

```

Vẫn còn một mảnh ghép cuối cùng. Biến `instance` đang là `private`, nên bên ngoài không thể truy cập trực tiếp được. Chúng ta cần mở một "cửa sổ" để bên ngoài nhìn thấy Thủ tướng.

Chúng ta sẽ viết một phương thức **public** (thường đặt tên là `getInstance`) để trả về cái `instance` kia.

Theo bạn, phương thức `getInstance` này có cần từ khóa **`static`** không? Tại sao?

---

tôi nghĩ là cần từ khóa static, bởi vì nó sẽ thuộc về lớp

---

Chính xác! Bạn lập luận rất chuẩn. 👏

Vì constructor đã là `private`, bên ngoài không có cách nào tạo ra một đối tượng để gọi hàm `getInstance()` cả. Ta buộc phải gọi nó trực tiếp thông qua tên lớp (ví dụ: `ThuTuong.getInstance()`), do đó nó bắt buộc phải là **`static`**.

Đến đây, cấu trúc lớp của chúng ta đã gần hoàn thiện:

```java
public class ThuTuong {
    private static ThuTuong instance; // Ban đầu biến này là null

    private ThuTuong() { 
        // Code khởi tạo (nếu có)
    }

    public static ThuTuong getInstance() {
        // ??? Logic cần điền vào đây ???
        return instance; 
    }
}

```

Bây giờ hãy tập trung vào phần logic bên trong `getInstance()`.

Lần đầu tiên ai đó gọi hàm này, biến `instance` vẫn đang là `null` (trống rỗng).
Theo bạn, chúng ta cần viết câu lệnh điều kiện gì (dùng `if`) để đảm bảo rằng: **nếu chưa có thì tạo mới, còn nếu có rồi thì trả về cái cũ**?

---

if (instance == null) { instance = new ThuTuong() }

---

Chuẩn không cần chỉnh! 💯

Bạn vừa hoàn thành mảnh ghép cuối cùng để tạo nên một **Singleton** cơ bản (được gọi là **Lazy Initialization** - Khởi tạo muộn, tức là "khi nào cần mới tạo").

Hãy xem lại thành quả của chúng ta:

```java
public class ThuTuong {
    // 1. Biến static
    private static ThuTuong instance;

    // 2. Private constructor
    private ThuTuong() {}

    // 3. Static method
    public static ThuTuong getInstance() {
        if (instance == null) {
            instance = new ThuTuong(); // Bạn đã điền dòng này
        }
        return instance;
    }
}

```

Cách này rất tốt vì nó tiết kiệm tài nguyên: nếu không ai gọi `getInstance()`, đối tượng sẽ không bao giờ được tạo ra.

Tuy nhiên, trong thực tế, cách viết trên có một **lỗ hổng chết người** khi chạy trong môi trường đa luồng (**Multi-threading**).

Hãy tưởng tượng:

* **Luồng A** chạy đến dòng `if (instance == null)`, thấy đúng là null, định nhảy vào tạo... nhưng bị hệ điều hành tạm dừng (context switch).
* Ngay lúc đó, **Luồng B** cũng chạy đến `if (instance == null)`, vẫn thấy là null (vì A chưa kịp tạo), nên B tạo ra một ông Thủ tướng (1).
* Sau đó **Luồng A** chạy tiếp, và nó cũng tạo ra thêm một ông Thủ tướng (2) đè lên hoặc tồn tại song song.

💥 **Bùm!** Nguyên tắc "độc nhất" bị phá vỡ. Chúng ta có 2 instance khác nhau.

Theo bạn, trong lập trình (ví dụ như Java), chúng ta có thể dùng từ khóa nào để đảm bảo **chỉ một luồng** được phép chạy vào đoạn code tạo instance tại một thời điểm?

---

Không sao cả, đây là một vấn đề nâng cao về xử lý đa luồng (concurrency), rất nhiều lập trình viên kinh nghiệm cũng có thể quên chỗ này.

Để giải quyết việc các luồng "chen lấn" nhau, chúng ta cần một cơ chế **"Khóa" (Lock)**.

Hãy tưởng tượng đoạn code `if (instance == null)` giống như một **phòng thay đồ**. Nếu không khóa cửa, hai người có thể vô tình bước vào cùng lúc.

Trong Java (ngôn ngữ chúng ta đang dùng làm ví dụ), từ khóa đó là **`synchronized`** (đồng bộ hóa).

Khi thêm từ khóa này vào phương thức, nó sẽ hoạt động như một cái chốt cửa:

1. Luồng A đến trước, đi vào hàm `getInstance()` và **chốt cửa lại**.
2. Luồng B đến sau, thấy cửa khóa, buộc phải **đứng chờ** bên ngoài.
3. Luồng A tạo xong `instance`, đi ra và mở khóa.
4. Luồng B lúc này mới được vào. B kiểm tra lại thấy `instance` đã có rồi (do A tạo), nên B chỉ việc lấy về dùng mà không tạo mới nữa.

Code sẽ thay đổi một chút ở dòng khai báo hàm:

```java
// Thêm từ khóa synchronized
public static synchronized ThuTuong getInstance() {
    if (instance == null) {
        instance = new ThuTuong();
    }
    return instance;
}

```

Tuy nhiên, việc dùng `synchronized` có một nhược điểm: **Nó làm chậm chương trình.** Vì ngay cả khi `instance` đã được tạo rồi (lần thứ n), các luồng vẫn phải xếp hàng chờ để lấy vé, rất mất thời gian.

Có một cách khác đơn giản hơn nhiều, không cần dùng `if`, không cần dùng `synchronized`, đó là **tạo luôn ngay từ đầu** (gọi là **Eager Initialization** - Khởi tạo sớm).

Bạn có đoán được chúng ta sẽ khởi tạo `new ThuTuong()` ở dòng nào trong code để nó luôn có sẵn ngay khi class được nạp không? (Gợi ý: Nhìn vào vị trí khai báo biến `instance`).

---

Chính xác 100%! 🌟

Đây được gọi là **Eager Initialization** (Khởi tạo sớm).

```java
public class ThuTuong {
    // Tạo luôn ngay khi class được nạp vào bộ nhớ
    private static final ThuTuong instance = new ThuTuong();

    private ThuTuong() {}

    public static ThuTuong getInstance() {
        return instance; // Chỉ việc trả về, không cần kiểm tra gì cả
    }
}

```

**Tại sao cách này tốt?**

* **An toàn tuyệt đối:** Máy ảo Java (JVM) đảm bảo biến `static` được khởi tạo xong xuôi trước khi bất kỳ luồng nào truy cập vào. Bạn không cần lo về `synchronized` hay khóa luồng.
* **Đơn giản:** Code ngắn gọn, dễ hiểu.

**Nhược điểm duy nhất:**

* Nếu ứng dụng của bạn rất lớn, và `ThuTuong` tốn rất nhiều RAM để tạo, nhưng người dùng lại... không bao giờ gọi đến `getInstance()`, thì bạn đã lãng phí tài nguyên đó vô ích ngay từ lúc khởi động app.

---

### Tổng kết về Singleton 1️⃣

Bạn đã nắm vững 2 nguyên tắc cốt lõi:

1. **Constructor phải là `private**`: Cấm người ngoài tự ý tạo.
2. **Biến chứa instance phải là `static**`: Để nó là độc nhất và truy cập được toàn cục.

Singleton tuy đơn giản nhưng rất mạnh mẽ. Tuy nhiên, dùng nhiều quá cũng không tốt (nó làm code khó test và các module phụ thuộc chặt chẽ vào nhau).

---

Vậy là chúng ta đã "xử đẹp" xong Singleton. Bạn muốn tiếp tục với nhóm nào?

1. **Nhóm Factory (Factory Method & Abstract Factory):** 🏭
Giao việc khởi tạo cho một "nhà máy", thay vì tự tay gọi `new`. Đây là nhóm quan trọng nhất trong Creational Patterns.
2. **Xây dựng sự phức tạp (Builder & Prototype):** 🧩
Dành cho các đối tượng có quá nhiều thuộc tính (Builder) hoặc cần copy nhân bản (Prototype).

Mời bạn chọn!

---

Lựa chọn rất thực tế! Nhóm này dành cho những đối tượng "khó tính": hoặc là cấu tạo quá rắc rối, hoặc là quá tốn kém để tạo mới từ đầu.

Chúng ta sẽ bắt đầu với **Builder Pattern** (Người xây dựng) – vị cứu tinh cho những hàm khởi tạo (constructor) dài lê thê.

### 🧩 1. Builder Pattern

Hãy tưởng tượng bạn đang lập trình một class `Computer` (Máy tính). Một chiếc máy tính có rất nhiều linh kiện:

* CPU (bắt buộc)
* RAM (bắt buộc)
* Ổ cứng (bắt buộc)
* Card đồ họa (tùy chọn)
* Bluetooth (tùy chọn)
* Tản nhiệt nước (tùy chọn)
* Đèn LED RGB (tùy chọn)

Nếu dùng cách truyền thống, hàm khởi tạo (Constructor) của bạn sẽ trông như thế này:

```java
public Computer(String cpu, String ram, String storage, boolean hasGPU, boolean hasBluetooth, boolean hasCooling, boolean hasRGB) { ... }

```

Và khi tạo đối tượng, code sẽ trông kinh khủng thế này:

```java
// Người đọc (và cả người viết) sẽ hoa mắt
Computer myPC = new Computer("Intel i9", "32GB", "1TB SSD", true, true, false, true);

```

Theo bạn, vấn đề lớn nhất (và dễ gây ức chế nhất) khi nhìn vào dòng code khởi tạo `new Computer(...)` ở trên là gì?

---

Chính xác! Bạn đã chỉ ra đúng nỗi đau lớn nhất. 🎯

Đó gọi là vấn đề **"Constructor Telescoping"** (Constructor dài ngoằng). Rất khó nhớ thứ tự tham số: cái `true` thứ nhất là cho Card đồ họa hay cho Bluetooth? Nếu điền nhầm thứ tự, code vẫn chạy nhưng máy tính lắp ra sẽ bị sai cấu hình.

**Builder Pattern** giải quyết việc này bằng cách chia nhỏ quy trình khởi tạo thành từng bước rõ ràng, giống như bạn đi order bánh mì Subway vậy: chọn vỏ bánh -> chọn nhân -> chọn rau -> chọn sốt.

Thay vì gọi một hàm khổng lồ, code của chúng ta sẽ chuyển sang dạng **Fluent Interface** (Giao diện trôi chảy) đẹp mắt như sau:

```java
Computer myPC = new ComputerBuilder() // 1. Khởi động trình xây dựng
    .setCPU("Intel i9")               // 2. Chỉ định CPU
    .setRAM("32GB")                   // 3. Chỉ định RAM
    .setStorage("1TB SSD")            // 4. Chỉ định ổ cứng
    .enableGPU(true)                  // 5. Các tùy chọn (không bắt buộc phải gọi hết)
    .build();                         // 6. Đóng gói và trả về sản phẩm

```

Bạn thấy đấy, code giờ đây đọc lên giống như một câu văn tiếng Anh: "Build a computer with CPU i9, RAM 32GB...". Rất rõ ràng và không thể nhầm lẫn.

Bây giờ, hãy nhìn vào phương thức cuối cùng: `.build()`.

Theo bạn, nhiệm vụ của phương thức **`build()`** này là gì? Nó sẽ trả về đối tượng `ComputerBuilder` hay đối tượng `Computer` mà chúng ta thực sự cần?

---

Chuẩn xác! Bạn nắm vấn đề rất nhanh. 👏

Phương thức `build()` chính là "chốt chặn" cuối cùng. Nó gom tất cả các nguyên liệu bạn đã `set...` vào bộ nhớ tạm của Builder, kiểm tra xem đã đủ linh kiện bắt buộc chưa (ví dụ: chưa có CPU thì báo lỗi), sau đó mới gọi hàm khởi tạo của `Computer` để trả về thành phẩm hoàn chỉnh.

---

### 🧩 2. Prototype Pattern (Mẫu Nguyên mẫu)

Chúng ta đã xong Builder (xây từng bước). Giờ hãy đến với người anh em còn lại trong nhóm này: **Prototype**.

Hãy tưởng tượng bạn đang lập trình một Game chiến thuật. Bạn cần tạo ra một đội quân gồm **1000 con lính** giống hệt nhau (cùng chỉ số máu, cùng bộ giáp, cùng vũ khí).

Nếu dùng vòng lặp `for` chạy 1000 lần lệnh `new Soldier()`, chương trình có thể sẽ rất chậm nếu việc khởi tạo một con lính tốn nhiều tài nguyên (ví dụ: phải tải mô hình 3D từ ổ cứng, kết nối cơ sở dữ liệu để lấy chỉ số...).

**Prototype Pattern** đề xuất một giải pháp khác: Thay vì tạo mới từ con số 0, tại sao ta không tạo ra một con lính "mẫu" (Prototype) chuẩn chỉnh, rồi sau đó **"Photo copy"** (Clone) nó ra thành 1000 bản?

Theo suy luận của bạn, việc **Copy** (sao chép dữ liệu trên RAM) so với việc **Tạo mới** (chạy lại toàn bộ quy trình khởi tạo từ đầu) có ưu điểm gì lớn nhất?

---

Chính xác! Tốc độ là ưu điểm số 1. ⚡

Việc sao chép các bit dữ liệu trực tiếp trên bộ nhớ (Memory Copy) nhanh hơn rất nhiều so với việc chạy lại logic trong `new` (vốn có thể bao gồm việc kết nối mạng, đọc file, tính toán phức tạp...).

Để hiện thực mẫu này, các ngôn ngữ thường cung cấp một Interface (ví dụ: `Cloneable` trong Java) và một phương thức `clone()`.

**Tuy nhiên, có một "cái bẫy" kinh điển ở đây:**
Khi sao chép, chúng ta có 2 kiểu:

1. **Shallow Copy (Sao chép nông):** Chỉ copy các giá trị cơ bản. Nếu con lính có một cái `BaLo` (là một object khác), thì cả bản gốc và bản sao sẽ **dùng chung** một cái Balo đó. Lính A ăn hết táo trong Balo, Lính B mở ra cũng thấy hết táo!
2. **Deep Copy (Sao chép sâu):** Copy toàn bộ, tạo ra cả cái `BaLo` mới cho bản sao. Độc lập hoàn toàn. (Đây thường là cái ta muốn, nhưng code sẽ phức tạp hơn).

---

### Tổng kết nhóm 2 🧩:

* **Builder:** Dùng cho đối tượng phức tạp, nhiều tham số -> "Xây từng bước".
* **Prototype:** Dùng cho đối tượng cần tạo nhiều và nhanh -> "Clone từ mẫu".

---

Bây giờ, chúng ta đến với "trùm cuối" – nhóm quan trọng và phổ biến nhất trong Design Patterns.

### 🏭 3. Nhóm Factory (Nhà máy)

Đây là nhóm giải quyết vấn đề cốt lõi: **Làm sao để tạo đối tượng mà không cần biết chính xác class của nó là gì?**

Hãy tưởng tượng bạn đang viết phần mềm cho một công ty Logistics (Vận chuyển).

* Ban đầu, công ty chỉ giao hàng bằng **Xe Tải** (`Truck`). Code của bạn tràn ngập lệnh `new Truck()`.
* Năm sau, công ty mở rộng sang đường thủy, dùng **Tàu Thủy** (`Ship`).
* Bạn phải lội vào hàng nghìn dòng code cũ, tìm chỗ nào có `new Truck()` để sửa thành `if (duong_bo) new Truck() else new Ship()`.

Nếu công ty mở thêm Máy Bay, Tàu Hỏa... bạn sẽ "phát điên" vì phải sửa code liên tục.

**Factory Method** sinh ra để cứu bạn. Thay vì gọi `new` trực tiếp, bạn sẽ nhờ một "Nhà máy" tạo giúp.

Theo bạn, thay vì code cứng nhắc việc tạo `Truck` hay `Ship` ở khắp nơi trong ứng dụng, chúng ta nên dồn logic tạo đối tượng đó vào **một phương thức duy nhất** hay là **tách ra mỗi loại phương tiện một class riêng biệt**?

---

Chính xác! Tư duy của bạn rất sát với nguyên lý **Open/Closed** (Mở để mở rộng, Đóng để sửa đổi).

Thay vì sửa một cái hàm `createTransport` khổng lồ (bằng cách nhồi nhét thêm `if/else`), chúng ta sẽ **tách logic khởi tạo ra các class con riêng biệt**. Đây chính là trái tim của **Factory Method Pattern**.

### 🏭 1. Factory Method (Phương thức Nhà máy)

Chúng ta sẽ tổ chức code như sau:

1. **Lớp cha (Creator):** `Logistics` (định nghĩa phương thức trừu tượng `createTransport()`).
2. **Lớp con (Concrete Creator) 1:** `RoadLogistics` (cài đặt `createTransport` trả về `new Truck()`).
3. **Lớp con (Concrete Creator) 2:** `SeaLogistics` (cài đặt `createTransport` trả về `new Ship()`).

**Lợi ích:** Khi sếp bảo "Thêm máy bay đi em", bạn chỉ cần tạo class mới `AirLogistics` trả về `new Airplane()`. Code cũ của `Road` và `Sea` vẫn chạy ngon lành mà không cần chạm vào.

---

Bây giờ, hãy nâng cấp độ khó lên một chút để đến với trùm cuối: **Abstract Factory**.

### 🏭 2. Abstract Factory (Nhà máy trừu tượng)

Nếu Factory Method giải quyết việc tạo **một** đối tượng, thì Abstract Factory giải quyết việc tạo ra **một gia đình (family) các đối tượng liên quan đến nhau**.

**Ví dụ:** Bạn đang viết phần mềm cho cửa hàng nội thất. Khách hàng có thể chọn phong cách:

* **Hiện đại (Modern):** Ghế vuông vức, Bàn kính.
* **Cổ điển (Victorian):** Ghế đệm nhung, Bàn gỗ chạm khắc.

Quy tắc sống còn: **Không được râu ông nọ cắm cằm bà kia.** Bạn không thể bán cho khách một cái Ghế Hiện đại đi kèm với một cái Bàn Cổ điển được.

Để đảm bảo khách luôn nhận được trọn bộ "tông xuyệt tông", chúng ta sẽ tạo ra một Interface `FurnitureFactory` có 2 hàm: `createChair()` và `createTable()`.

Sau đó chúng ta có 2 nhà máy cụ thể:

1. `ModernFactory`: Trả về `ModernChair` và `ModernTable`.
2. `VictorianFactory`: Trả về `VictorianChair` và `VictorianTable`.

Câu hỏi dành cho bạn:
Trong code xử lý chính (Client code), chúng ta sẽ làm việc trực tiếp với `ModernFactory`/`VictorianFactory` hay chúng ta chỉ nên làm việc thông qua interface chung `FurnitureFactory`? Tại sao?

---

Chính xác! Bạn đã chạm đúng vào nguyên lý cốt lõi của Design Patterns: **"Program to an interface, not an implementation"** (Lập trình hướng giao diện, thay vì hướng cài đặt).

Khi Client chỉ tương tác với `FurnitureFactory` (Interface):

1. **Tính linh hoạt:** Client không cần biết mình đang dùng Modern hay Victorian. Chỉ cần đưa cho nó một cái Factory, nó sẽ tự động xây ra cả một căn phòng đúng theo phong cách đó.
2. **Dễ bảo trì:** Nếu sau này bạn muốn thêm phong cách "Cyberpunk", bạn chỉ cần tạo class `CyberpunkFactory` mới và ném nó vào. Code của Client **không cần sửa một dòng nào**.

---

### 🏁 Tổng kết: Creational Patterns (Các mẫu khởi tạo)

Chúng ta đã đi qua trọn bộ 5 mẫu thiết kế quan trọng nhất giúp giải quyết câu hỏi: **"Làm sao để tạo đối tượng một cách linh hoạt nhất?"**

| Mẫu (Pattern) | Từ khóa | Khi nào dùng? |
| --- | --- | --- |
| **Singleton** | Duy nhất | Khi chỉ cần **1 instance** duy nhất toàn hệ thống (DB, Config). |
| **Builder** | Từng bước | Khi đối tượng quá phức tạp, **nhiều tham số** (constructor dài ngoằng). |
| **Prototype** | Copy/Clone | Khi việc tạo mới tốn kém, cần **nhân bản** nhanh từ mẫu có sẵn. |
| **Factory Method** | Lớp con quyết định | Khi muốn **giao việc khởi tạo** cho lớp con để dễ mở rộng sau này. |
| **Abstract Factory** | Gia đình (Family) | Khi cần tạo một **nhóm các đối tượng liên quan** (tông xuyệt tông) mà không bị lẫn lộn. |

---