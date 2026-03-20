Chào bạn. Bài tập CA5 này mang đến một góc nhìn thực tế rất hay về Observer Pattern trong bài toán nghiệp vụ 1-1 (Một ATM phục vụ một Tài khoản tại một thời điểm), thay vì mô hình 1-N (một Subject phát cho nhiều Observer) như các bài trước.
Điểm sáng của bài này để bạn nhấn mạnh với sinh viên: ATM (Subject) không trực tiếp trừ tiền. ATM chỉ làm nhiệm vụ "ra lệnh" và "kiểm tra", còn việc cập nhật số dư (trừ tiền) là trách nhiệm tự quản của Tài Khoản (Observer) sau khi nhận được thông báo từ ATM. Đây là nguyên lý đóng gói (Encapsulation) vô cùng quan trọng.
Dưới đây là thiết kế và mã nguồn theo đúng yêu cầu tinh gọn của bạn:
1. Sơ đồ lớp (PlantUML)
Sơ đồ này thể hiện rõ việc ATM tương tác với TaiKhoan thông qua một giao diện chung. Mặc dù ở thực tế ATM chỉ gắn với 1 tài khoản mỗi phiên giao dịch, ta vẫn thiết kế đúng chuẩn Observer Pattern để đảm bảo tính lỏng lẻo (loose coupling).
@startuml
skinparam classAttributeIconSize 0

' --- Giao diện Observer (Bản hợp đồng) ---
interface IObserverTaiKhoan {
    + kiemTraSoDu(soTienRut: double): boolean
    + nhanThongBao(thanhCong: boolean, soTien: double): void
}

' --- Subject (Nguồn phát sinh giao dịch) ---
class ATM {
    - taiKhoanHienTai: IObserverTaiKhoan
    + duaTheVao(tk: IObserverTaiKhoan): void
    + layTheRa(): void
    + rutTien(soTien: double): void
}

' --- Concrete Observer (Người lắng nghe và xử lý nội bộ) ---
class TaiKhoan {
    - tenChuTaiKhoan: String
    - soDu: double
    + TaiKhoan(ten: String, soDuBD: double)
    + kiemTraSoDu(soTienRut: double): boolean
    + nhanThongBao(thanhCong: boolean, soTien: double): void
}

' --- Mối quan hệ ---
ATM o--> "0..1" IObserverTaiKhoan : giao tiếp qua interface >
TaiKhoan ..|> IObserverTaiKhoan : implements

note right of ATM: Đóng vai trò Subject.\nKích hoạt sự kiện rút tiền\nvà thông báo kết quả.
note bottom of TaiKhoan: Đóng vai trò Observer.\nTự quản lý và cập nhật\nsố dư của chính mình.
@enduml

2. Mã nguồn Java minh họa
Đoạn mã được tối giản hóa, tập trung hoàn toàn vào logic của Mẫu thiết kế và sử dụng các comment mang tính sư phạm để sinh viên dễ dàng theo dõi luồng sự kiện.
// =================================================================
// BƯỚC 1: ĐỊNH NGHĨA GIAO DIỆN OBSERVER
// =================================================================
/**
 * Interface giao tiếp cốt lõi. Bất kỳ loại tài khoản nào (VIP, Thường, Tiết kiệm)
 * muốn cắm vào ATM đều phải tuân thủ hợp đồng này.
 */
interface IObserverTaiKhoan {
    boolean kiemTraSoDu(double soTienRut);
    void nhanThongBao(boolean hopLe, double soTien);
}

// =================================================================
// BƯỚC 2: XÂY DỰNG CONCRETE OBSERVER (TÀI KHOẢN CỤ THỂ)
// =================================================================
/**
 * Lớp Tài Khoản. Nó tự nắm giữ và quản lý số dư của chính mình.
 * Tuyệt đối không cho phép ATM trừ tiền trực tiếp từ thuộc tính soDu.
 */
class TaiKhoan implements IObserverTaiKhoan {
    private String tenChuTaiKhoan;
    private double soDu;

    public TaiKhoan(String tenChuTaiKhoan, double soDuBanDau) {
        this.tenChuTaiKhoan = tenChuTaiKhoan;
        this.soDu = soDuBanDau;
    }

    // Logic kiểm tra tính hợp lệ của giao dịch
    @Override
    public boolean kiemTraSoDu(double soTienRut) {
        return this.soDu >= soTienRut;
    }

    // Nơi nhận kết quả từ Subject (ATM) và tự tiến hành tính toán lại số dư
    @Override
    public void nhanThongBao(boolean hopLe, double soTien) {
        System.out.println("--- [HỆ THỐNG TÀI KHOẢN: " + tenChuTaiKhoan + "] ---");
        if (hopLe) {
            this.soDu -= soTien; // Tự trừ tiền của mình
            System.out.println("Giao dịch THÀNH CÔNG. Đã trừ: " + soTien);
        } else {
            System.out.println("Giao dịch THẤT BẠI. Số dư không đủ để rút: " + soTien);
        }
        System.out.println("Số dư hiện tại: " + this.soDu + "\n");
    }
}

// =================================================================
// BƯỚC 3: XÂY DỰNG SUBJECT (MÁY ATM)
// =================================================================
/**
 * Lớp ATM. Nó không lưu trữ tiền của khách hàng, nó chỉ là nơi kích hoạt
 * sự kiện và gửi thông báo phản hồi lại cho Observer.
 */
class ATM {
    // ATM chỉ phục vụ 1 tài khoản (Observer) tại một thời điểm
    private IObserverTaiKhoan taiKhoanHienTai;

    // Tương đương hàm Attach() trong Observer Pattern
    public void duaTheVao(IObserverTaiKhoan tk) {
        this.taiKhoanHienTai = tk;
        System.out.println("[ATM] Đã tiếp nhận thẻ.\n");
    }

    // Tương đương hàm Detach() trong Observer Pattern
    public void layTheRa() {
        this.taiKhoanHienTai = null;
        System.out.println("[ATM] Đã nhả thẻ.\n");
    }

    // Hàm thực thi luồng nghiệp vụ
    public void rutTien(double soTien) {
        if (taiKhoanHienTai == null) {
            System.out.println("[ATM] Vui lòng đưa thẻ vào trước khi giao dịch.");
            return;
        }

        System.out.println("[ATM] Đang yêu cầu rút: " + soTien);
        
        // 1. Máy ATM hỏi Observer xem có đủ điều kiện không
        boolean kiemTra = taiKhoanHienTai.kiemTraSoDu(soTien);
        
        // 2. Máy ATM phát thông báo (Notify) về cho Observer tự xử lý
        taiKhoanHienTai.nhanThongBao(kiemTra, soTien);
    }
}

// =================================================================
// BƯỚC 4: HÀM MAIN (CHẠY THỬ NGHIỆM)
// =================================================================
public class Main {
    public static void main(String[] args) {
        // 1. Khởi tạo các thực thể
        ATM mayATM = new ATM();
        TaiKhoan taiKhoanNguyenVanA = new TaiKhoan("Nguyễn Văn A", 500000);

        // 2. Thiết lập mối quan hệ Subject - Observer (Cắm thẻ vào máy)
        mayATM.duaTheVao(taiKhoanNguyenVanA);

        // 3. Thực hiện giao dịch hợp lệ
        mayATM.rutTien(200000);

        // 4. Thực hiện giao dịch vượt quá số dư
        mayATM.rutTien(400000);
        
        // 5. Kết thúc
        mayATM.layTheRa();
    }
}

