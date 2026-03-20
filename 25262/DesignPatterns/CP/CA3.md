Chào bạn. Bài toán CA3 này lại là một kịch bản vô cùng xuất sắc khác để minh họa cho Observer Pattern. Nếu bài CA2 mô phỏng sự kiện UI (UI Events), thì bài CA3 này mô phỏng luồng dữ liệu thời gian thực (Real-time Data Streaming) – một bài toán cực kỳ phổ biến trong các hệ thống tài chính, chứng khoán hay IoT.
Điểm "ăn tiền" nhất của bài này để nhấn mạnh với sinh viên là: Dịch vụ tỉ giá không cần biết nhà đầu tư dùng chiến lược gì. Dịch vụ chỉ việc "hét lên" tỉ giá mới, phần còn lại (mua/bán/chờ đợi) là việc nội bộ của từng nhà đầu tư.
Dưới đây là thiết kế chi tiết:
1. Sơ đồ lớp (PlantUML)
Sơ đồ này làm nổi bật việc DichVuTiGia giữ một danh sách các nhà đầu tư thông qua giao diện chung NhaDauTu. Các nhà đầu tư cụ thể (NhaDauTuA, NhaDauTuB) sẽ tự triển khai logic của riêng mình.
@startuml
skinparam classAttributeIconSize 0

' --- Giao diện Observer ---
interface NhaDauTu {
    + capNhatTiGia(tiGiaMoi: double): void
}

' --- Chủ thể (Subject) ---
class DichVuTiGia {
    - tiGiaHienTai: double
    - danhSachNhaDauTu: List<NhaDauTu>
    + dangKy(ndt: NhaDauTu): void
    + huyDangKy(ndt: NhaDauTu): void
    + thongBaoTatCa(): void
    + thayDoiTiGia(tiGiaMoi: double): void
}

' --- Các Concrete Observer (Nhà đầu tư cụ thể) ---
class NhaDauTuA {
    + capNhatTiGia(tiGiaMoi: double): void
}

class NhaDauTuB {
    + capNhatTiGia(tiGiaMoi: double): void
}

' --- Mối quan hệ ---
DichVuTiGia o--> "0..*" NhaDauTu : thông báo >
NhaDauTuA ..|> NhaDauTu : implements
NhaDauTuB ..|> NhaDauTu : implements

note right of DichVuTiGia: Nguồn phát dữ liệu (Subject).\nChỉ quan tâm việc báo tin.
note right of NhaDauTuA: Mỗi NĐT có chiến lược\nmua/bán độc lập (Observer).
@enduml

2. Mã nguồn Java minh họa (Có comment sư phạm)
Bạn có thể sử dụng đoạn mã này để giảng dạy. Các phần bình luận được thiết kế để sinh viên thấy rõ sự tách biệt trách nhiệm (Separation of Concerns).
import java.util.ArrayList;
import java.util.List;

// =================================================================
// BƯỚC 1: ĐỊNH NGHĨA GIAO DIỆN OBSERVER (NGƯỜI QUAN SÁT)
// =================================================================
/**
 * Giao diện chung cho mọi nhà đầu tư muốn nhận thông báo tỉ giá.
 * Bất kỳ ai muốn nghe ngóng thị trường đều phải có hàm capNhatTiGia().
 */
interface NhaDauTu {
    void capNhatTiGia(double tiGiaMoi);
}

// =================================================================
// BƯỚC 2: XÂY DỰNG SUBJECT (NGUỒN PHÁT DỮ LIỆU)
// =================================================================
/**
 * Lớp DichVuTiGia đóng vai trò Subject. Nó quản lý trạng thái (tỉ giá)
 * và danh sách những người đang theo dõi nó.
 */
class DichVuTiGia {
    private double tiGiaHienTai;
    // Danh sách các nhà đầu tư (chỉ giao tiếp qua Interface, không dùng class cụ thể)
    private List<NhaDauTu> danhSachNhaDauTu = new ArrayList<>();

    // Thêm nhà đầu tư vào danh sách nhận tin (Attach)
    public void dangKy(NhaDauTu ndt) {
        danhSachNhaDauTu.add(ndt);
    }

    // Xóa nhà đầu tư khỏi danh sách (Detach)
    public void huyDangKy(NhaDauTu ndt) {
        danhSachNhaDauTu.remove(ndt);
    }

    // Hành động kinh doanh: Cập nhật tỉ giá mới từ thị trường
    public void thayDoiTiGia(double tiGiaMoi) {
        System.out.println("\n[SYSTEM] Tỉ giá USD/VND vừa thay đổi thành: " + tiGiaMoi);
        this.tiGiaHienTai = tiGiaMoi;
        thongBaoTatCa(); // Có biến là phải báo cho anh em ngay!
    }

    // Lặp qua danh sách và gửi thông tin cho từng người (Notify)
    private void thongBaoTatCa() {
        for (NhaDauTu ndt : danhSachNhaDauTu) {
            ndt.capNhatTiGia(tiGiaHienTai);
        }
    }
}

// =================================================================
// BƯỚC 3: XÂY DỰNG CONCRETE OBSERVER (CÁC NHÀ ĐẦU TƯ CỤ THỂ)
// =================================================================
/**
 * Nhà đầu tư A có chiến lược: Mua vào khi giá thấp (< 24.000)
 */
class NhaDauTuA implements NhaDauTu {
    @Override
    public void capNhatTiGia(double tiGiaMoi) {
        if (tiGiaMoi < 24000) {
            System.out.println(" - Nhà đầu tư A: Tỉ giá " + tiGiaMoi + " đang rất rẻ. QUYẾT ĐỊNH MUA VÀO!");
        } else {
            System.out.println(" - Nhà đầu tư A: Giá cao quá, tiếp tục chờ đợi.");
        }
    }
}

/**
 * Nhà đầu tư B có chiến lược ngược lại: Bán ra chốt lời khi giá cao (> 25.000)
 */
class NhaDauTuB implements NhaDauTu {
    @Override
    public void capNhatTiGia(double tiGiaMoi) {
        if (tiGiaMoi > 25000) {
            System.out.println(" - Nhà đầu tư B: Tỉ giá " + tiGiaMoi + " quá ngon. QUYẾT ĐỊNH BÁN RA CHỐT LỜI!");
        } else {
            System.out.println(" - Nhà đầu tư B: Giá chưa đạt đỉnh, giữ nguyên tài sản.");
        }
    }
}

// =================================================================
// BƯỚC 4: HÀM MAIN CHẠY THỬ (MÔ PHỎNG HỆ THỐNG)
// =================================================================
public class Main {
    public static void main(String[] args) {
        // 1. Khởi tạo trung tâm phát dữ liệu
        DichVuTiGia trungTamTiGia = new DichVuTiGia();

        // 2. Tạo các nhà đầu tư tham gia thị trường
        NhaDauTu ndtA = new NhaDauTuA();
        NhaDauTu ndtB = new NhaDauTuB();

        // 3. Các nhà đầu tư đăng ký nhận thông báo
        trungTamTiGia.dangKy(ndtA);
        trungTamTiGia.dangKy(ndtB);

        // 4. Bắt đầu mô phỏng thị trường biến động
        // Trung tâm chỉ cần Set giá trị mới, hệ thống tự động chạy logic của từng nhà đầu tư.
        
        // Kịch bản 1: Giá giảm sâu
        trungTamTiGia.thayDoiTiGia(23800); 

        // Kịch bản 2: Giá ổn định ở mức giữa
        trungTamTiGia.thayDoiTiGia(24500);

        // Kịch bản 3: Giá tăng vọt
        trungTamTiGia.thayDoiTiGia(25200);
    }
}

Sự tương phản trong hàm capNhatTiGia() của NhaDauTuA và NhaDauTuB chính là điểm mấu chốt để bạn làm rõ yêu cầu "quyết định việc bán ra hay mua vào theo chiến lược của riêng mình" mà đề bài đã nêu. Khối mã này cũng đã đảm bảo sự ngắn gọn, không có mã dư thừa gây nhiễu.
Bạn có cần hỗ trợ xuất các sơ đồ thiết kế này thành định dạng slide hoặc chuẩn bị tiếp cho các Design Pattern khác trong chương trình học không?
