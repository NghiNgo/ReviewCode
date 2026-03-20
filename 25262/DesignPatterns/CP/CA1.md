1. Sơ đồ lớp (PlantUML)
@startuml
skinparam classAttributeIconSize 0

interface Listener<T> {
    + onEvent(data: T): void
}

class Stream<T> {
    - listeners: List<Listener<T>>
    + addListener(l: Listener<T>): void
    + addEvent(t: T): void
}

class MonHoc {
    - tenMonHoc: String
    + MonHoc(ten: String)
    + toString(): String
}

class DataAccess {
    - {static} instance: DataAccess
    - dsMonHoc: List<MonHoc>
    - stream: Stream<List<MonHoc>>
    - DataAccess()
    + {static} getInstance(): DataAccess
    + getStream(): Stream<List<MonHoc>>
    + themMonHoc(mh: MonHoc): void
}

class Client {
    - tenClient: String
    + Client(ten: String)
    + onEvent(data: List<MonHoc>): void
}

Stream o--> "0..*" Listener : notifies
Client ..|> Listener : implements
DataAccess *--> "1" Stream : contains
DataAccess o--> "0..*" MonHoc : manages
@enduml

2. Mã nguồn Java minh họa
import java.util.ArrayList;
import java.util.List;

// --- DỮ LIỆU ---
class MonHoc {
    private String tenMonHoc;

    public MonHoc(String tenMonHoc) {
        this.tenMonHoc = tenMonHoc;
    }

    @Override
    public String toString() {
        return tenMonHoc;
    }
}

// --- OBSERVER PATTERN ---
interface Listener<T> {
    void onEvent(T data);
}

class Stream<T> {
    private List<Listener<T>> listeners = new ArrayList<>();

    public void addListener(Listener<T> l) {
        listeners.add(l);
    }

    public void addEvent(T t) {
        for (Listener<T> l : listeners) {
            l.onEvent(t);
        }
    }
}

// --- SINGLETON PATTERN & QUẢN LÝ DỮ LIỆU ---
class DataAccess {
    private static DataAccess instance;
    private List<MonHoc> dsMonHoc;
    private Stream<List<MonHoc>> stream;

    private DataAccess() {
        dsMonHoc = new ArrayList<>();
        stream = new Stream<>();
    }

    public static DataAccess getInstance() {
        if (instance == null) {
            instance = new DataAccess();
        }
        return instance;
    }

    public Stream<List<MonHoc>> getStream() {
        return stream;
    }

    public void themMonHoc(MonHoc mh) {
        dsMonHoc.add(mh);
        stream.addEvent(dsMonHoc); // Tự động thông báo khi có thay đổi
    }
}

// --- CONCRETE OBSERVER ---
class Client implements Listener<List<MonHoc>> {
    private String tenClient;

    public Client(String tenClient) {
        this.tenClient = tenClient;
        // Client đăng ký lắng nghe sự kiện từ Stream của DataAccess
        DataAccess.getInstance().getStream().addListener(this);
    }

    @Override
    public void onEvent(List<MonHoc> data) {
        System.out.println("[" + tenClient + "] Cập nhật danh sách môn học:");
        for (MonHoc mh : data) {
            System.out.println(" - " + mh.toString());
        }
        System.out.println("-------------------------");
    }
}

// --- HÀM MAIN CHẠY THỬ ---
public class Main {
    public static void main(String[] args) {
        // Khởi tạo các Client, chúng sẽ tự động kết nối vào Singleton DataAccess
        Client client1 = new Client("Client 1 (Web)");
        Client client2 = new Client("Client 2 (Mobile)");

        // Thao tác làm thay đổi dữ liệu
        DataAccess.getInstance().themMonHoc(new MonHoc("Toán rời rạc"));
        DataAccess.getInstance().themMonHoc(new MonHoc("Lập trình Java"));
    }
}

Bạn có muốn tôi xuất sơ đồ PlantUML này ra định dạng ảnh (PNG/SVG) để tải về không?
---
Chào bạn, tôi hoàn toàn đồng ý. Việc có một hàm main ngắn gọn, tập trung để chạy thử (runnable) là rất cần thiết để sinh viên mắt thấy tai nghe luồng dữ liệu thực tế chạy như thế nào, từ đó tin tưởng vào lý thuyết vừa học.
Dưới đây là phiên bản mã nguồn hoàn chỉnh, ghép nối tất cả các thành phần lại với nhau và có hàm main đủ tối giản để minh họa chính xác yêu cầu của đề bài (nhiều Client cùng lắng nghe 1 biến toàn cục DataAccess).
Mã nguồn Java hoàn chỉnh
Bạn có thể copy toàn bộ đoạn code này vào một file Main.java (hoặc tách ra các file riêng biệt tương ứng với tên class) để chạy trực tiếp trên lớp.
import java.util.ArrayList;
import java.util.List;

// 1. LỚP DỮ LIỆU ĐƠN GIẢN
class MonHoc {
    public String id;
    public String ten;

    public MonHoc(String id, String ten) {
        this.id = id;
        this.ten = ten;
    }
    
    @Override
    public String toString() {
        return id + " - " + ten;
    }
}

// 2. GIAO DIỆN LẮNG NGHE (Observer)
interface Listener<T> {
    void onEvent(T t);
}

// 3. LỚP STREAM (Subject - Nguồn phát sự kiện)
class Stream<T> {
    private List<Listener<T>> listeners = new ArrayList<>();

    // Dùng để đăng ký một client với Stream
    public void addListener(Listener<T> l) {
        listeners.add(l);
    }

    // Thêm một sự kiện/dữ liệu vào Stream, client sẽ tự động nghe thấy
    public void addEvent(T t) {
        for (Listener<T> l : listeners) {
            l.onEvent(t);
        }
    }
}

// 4. LỚP TRUY CẬP DỮ LIỆU (Áp dụng Singleton - Câu b)
class DataAccess {
    private static DataAccess instance;
    private List<MonHoc> dsMonHoc;
    
    // Chứa một stream để phát dữ liệu
    public Stream<List<MonHoc>> stream;

    // Private constructor để chặn việc tạo bằng từ khóa 'new' từ bên ngoài
    private DataAccess() {
        dsMonHoc = new ArrayList<>();
        stream = new Stream<>();
    }

    // Biến toàn cục, duy nhất
    public static DataAccess getInstance() {
        if (instance == null) {
            instance = new DataAccess();
        }
        return instance;
    }

    // Các thao tác CRUD: Sau khi thay đổi data, gọi addEvent để tự động báo cho Client
    public void themMonHoc(MonHoc mh) {
        dsMonHoc.add(mh);
        stream.addEvent(dsMonHoc);
    }

    public void xoaMonHoc(MonHoc mh) {
        dsMonHoc.remove(mh);
        stream.addEvent(dsMonHoc);
    }
}

// 5. LỚP CLIENT (Concrete Observer)
class Client implements Listener<List<MonHoc>> {
    private String tenClient;

    public Client(String tenClient) {
        this.tenClient = tenClient;
        // Client tự động tìm đến thể hiện duy nhất của DataAccess để đăng ký lắng nghe
        DataAccess.getInstance().stream.addListener(this);
    }

    // Hàm tự động kích hoạt khi DataAccess gọi addEvent()
    @Override
    public void onEvent(List<MonHoc> ds) {
        System.out.println("--- [" + tenClient + "] TỰ ĐỘNG CẬP NHẬT MÀN HÌNH ---");
        for (MonHoc mh : ds) {
            System.out.println("    + " + mh.toString());
        }
        System.out.println("-------------------------------------------------");
    }
}

// 6. CHƯƠNG TRÌNH CHÍNH (Chạy minh họa cho sinh viên)
public class Main {
    public static void main(String[] args) {
        // Giả sử có 2 Client (ví dụ: 1 App Mobile, 1 Web Dashboard) cùng chạy
        // Dù khởi tạo ở đâu, chúng đều trỏ về 1 DataAccess duy nhất nhờ Singleton
        Client client1 = new Client(("Màn hình App Mobile"));
        Client client2 = new Client(("Màn hình Web"));

        System.out.println(">>> THỰC HIỆN: Thêm môn Lập trình Java...");
        MonHoc mh1 = new MonHoc("MH01", "Lập trình Java");
        DataAccess.getInstance().themMonHoc(mh1);
        // Ngay sau dòng này, sinh viên sẽ thấy CẢ 2 CLIENT đều tự in ra kết quả

        System.out.println("\n>>> THỰC HIỆN: Thêm môn Cấu trúc dữ liệu...");
        MonHoc mh2 = new MonHoc("MH02", "Cấu trúc dữ liệu");
        DataAccess.getInstance().themMonHoc(mh2);
        
        System.out.println("\n>>> THỰC HIỆN: Xóa môn Lập trình Java...");
        DataAccess.getInstance().xoaMonHoc(mh1);
    }
}

Kết quả xuất ra màn hình (Output)
Khi bạn ấn Run trên IDE cho sinh viên xem, console sẽ in ra thế này, chứng minh cực kỳ rõ ràng tính năng "tự động cập nhật" (Data Binding / Observer) và "truy cập chung một đối tượng" (Singleton):
>>> THỰC HIỆN: Thêm môn Lập trình Java...
--- [Màn hình App Mobile] TỰ ĐỘNG CẬP NHẬT MÀN HÌNH ---
    + MH01 - Lập trình Java
-------------------------------------------------
--- [Màn hình Web] TỰ ĐỘNG CẬP NHẬT MÀN HÌNH ---
    + MH01 - Lập trình Java
-------------------------------------------------

>>> THỰC HIỆN: Thêm môn Cấu trúc dữ liệu...
--- [Màn hình App Mobile] TỰ ĐỘNG CẬP NHẬT MÀN HÌNH ---
    + MH01 - Lập trình Java
    + MH02 - Cấu trúc dữ liệu
-------------------------------------------------
--- [Màn hình Web] TỰ ĐỘNG CẬP NHẬT MÀN HÌNH ---
    + MH01 - Lập trình Java
    + MH02 - Cấu trúc dữ liệu
-------------------------------------------------

>>> THỰC HIỆN: Xóa môn Lập trình Java...
--- [Màn hình App Mobile] TỰ ĐỘNG CẬP NHẬT MÀN HÌNH ---
    + MH02 - Cấu trúc dữ liệu
-------------------------------------------------
--- [Màn hình Web] TỰ ĐỘNG CẬP NHẬT MÀN HÌNH ---
    + MH02 - Cấu trúc dữ liệu
-------------------------------------------------

Với cấu trúc file liền mạch này, bạn có thể copy-paste thẳng vào công cụ giảng dạy hoặc gửi cho sinh viên nghiệm thu ngay. Cần tôi xuất file PlantUML ra dạng ảnh hoặc text riêng biệt để bạn dễ chèn vào slide không?
