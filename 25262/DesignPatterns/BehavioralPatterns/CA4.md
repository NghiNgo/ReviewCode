Chào bạn. Bài tập CA4 này tiến thêm một bước nữa trong việc ứng dụng Observer Pattern: Xử lý trạng thái của các Observer.
Điểm hay nhất của bài này để nhấn mạnh với sinh viên là sự khác biệt giữa hai loại Observer:
 * Thành viên A (Stateless): Không nhớ gì cả, ai bảo gì thì in ra cái đó (giống như việc nhận thông báo đẩy - Push Notification trên điện thoại rồi vuốt bỏ).
 * Thành viên B (Stateful): Tự duy trì một "cơ sở dữ liệu" nội bộ (danh sách tin riêng). Khi Subject có thay đổi, Observer này không chỉ báo cáo mà còn phải xử lý đồng bộ dữ liệu cục bộ của mình (giống như ứng dụng email tải thư về máy).
Dưới đây là thiết kế và mã nguồn bám sát tuyệt đối vào mô tả của đề bài.
1. Sơ đồ lớp (PlantUML)
Sơ đồ thể hiện rõ Topic (Subject) quản lý một danh sách ThanhVien (Observer). ThanhVienB có thêm một mối quan hệ tự chứa TinTuc để lưu trữ dữ liệu riêng.
@startuml
skinparam classAttributeIconSize 0

' --- Dữ liệu truyền tải ---
class TinTuc {
    - id: int
    - noiDung: String
    + TinTuc(id: int, noiDung: String)
    + getId(): int
    + setNoiDung(noiDung: String): void
}

' --- Giao diện Observer ---
interface ThanhVien {
    + nhanThongBao(tin: TinTuc, laTaoMoi: boolean): void
}

' --- Subject ---
class Topic {
    - danhSachThanhVien: List<ThanhVien>
    - danhSachTin: List<TinTuc>
    - tinCounter: int
    + dangKy(tv: ThanhVien): void
    + huyDangKy(tv: ThanhVien): void
    + taoTinMoi(noiDung: String): void
    + capNhatTin(id: int, noiDungMoi: String): void
    - thongBaoChoThanhVien(tin: TinTuc, laTaoMoi: boolean): void
}

' --- Các Concrete Observer ---
class ThanhVienA {
    - ten: String
    + nhanThongBao(tin: TinTuc, laTaoMoi: boolean): void
}

class ThanhVienB {
    - ten: String
    - tinDaNhan: List<TinTuc>
    + nhanThongBao(tin: TinTuc, laTaoMoi: boolean): void
}

' --- Mối quan hệ ---
Topic o--> "0..*" ThanhVien : "thông báo"
ThanhVienA ..|> ThanhVien : "implements"
ThanhVienB ..|> ThanhVien : "implements"
Topic *--> "0..*" TinTuc : "lưu trữ"
ThanhVienB *--> "0..*" TinTuc : "lưu trữ cục bộ"

note right of ThanhVienA: Stateless Observer:\nChỉ in tin ra màn hình
note bottom of ThanhVienB: Stateful Observer:\nTự duy trì danh sách tin riêng
@enduml

2. Mã nguồn Java minh họa
Code được chia làm các khối rõ ràng. Hàm nhanThongBao sử dụng thêm cờ laTaoMoi (boolean) để báo cho Observer biết đây là tin mới hay tin cập nhật, giúp các Observer có cách ứng xử phù hợp.
import java.util.ArrayList;
import java.util.List;

// =================================================================
// 1. LỚP DỮ LIỆU CỐT LÕI
// =================================================================
class TinTuc {
    public int id;
    public String noiDung;

    public TinTuc(int id, String noiDung) {
        this.id = id;
        this.noiDung = noiDung;
    }
}

// =================================================================
// 2. GIAO DIỆN OBSERVER
// =================================================================
interface ThanhVien {
    // Nhận tin và một cờ để biết đó là hành động Tạo Mới (true) hay Cập Nhật (false)
    void nhanThongBao(TinTuc tin, boolean laTaoMoi);
}

// =================================================================
// 3. LỚP SUBJECT (NGUỒN PHÁT TIN)
// =================================================================
class Topic {
    private List<ThanhVien> danhSachThanhVien = new ArrayList<>();
    private List<TinTuc> danhSachTin = new ArrayList<>();
    private int tinCounter = 1; // Dùng để tự tăng ID cho tin tức

    // Các thành viên tự đăng ký hoặc hủy đăng ký
    public void dangKy(ThanhVien tv) {
        danhSachThanhVien.add(tv);
    }

    public void huyDangKy(ThanhVien tv) {
        danhSachThanhVien.remove(tv);
    }

    // Yêu cầu: "Tạo thông tin mới, lưu trữ, chuyển tiếp cho các thành viên"
    public void taoTinMoi(String noiDung) {
        TinTuc tinMoi = new TinTuc(tinCounter++, noiDung);
        danhSachTin.add(tinMoi);
        System.out.println("\n[TOPIC] Vừa đăng một tin mới: " + noiDung);
        thongBaoChoThanhVien(tinMoi, true); 
    }

    // Yêu cầu: "Cập nhật tin, tự động chuyển cho thành viên"
    public void capNhatTin(int id, String noiDungMoi) {
        for (TinTuc tin : danhSachTin) {
            if (tin.id == id) {
                tin.noiDung = noiDungMoi;
                System.out.println("\n[TOPIC] Vừa cập nhật tin số " + id);
                thongBaoChoThanhVien(tin, false);
                return;
            }
        }
    }

    // Hàm gọi Broadcast đến tất cả Observer
    private void thongBaoChoThanhVien(TinTuc tin, boolean laTaoMoi) {
        for (ThanhVien tv : danhSachThanhVien) {
            tv.nhanThongBao(tin, laTaoMoi);
        }
    }
}

// =================================================================
// 4. CÁC LỚP CONCRETE OBSERVER (THỰC THI NGHIỆP VỤ KHÁC NHAU)
// =================================================================

// Yêu cầu: "Chỉ in tin mới nhận hoặc cập nhật ra màn hình"
class ThanhVienA implements ThanhVien {
    @Override
    public void nhanThongBao(TinTuc tin, boolean laTaoMoi) {
        String hanhDong = laTaoMoi ? "TIN MỚI" : "TIN CẬP NHẬT";
        System.out.println(" -> Thành viên A nhận được [" + hanhDong + "]: " + tin.noiDung);
    }
}

// Yêu cầu: "In tất cả tin đã nhận kèm số thứ tự. Khi cập nhật thì tự update danh sách cục bộ"
class ThanhVienB implements ThanhVien {
    // Duy trì một cơ sở dữ liệu (danh sách) nội bộ biệt lập với Topic
    private List<TinTuc> tinDaNhan = new ArrayList<>();

    @Override
    public void nhanThongBao(TinTuc tin, boolean laTaoMoi) {
        if (laTaoMoi) {
            // Thêm một bản sao (hoặc lưu refernce) vào danh sách riêng
            tinDaNhan.add(new TinTuc(tin.id, tin.noiDung));
            
            System.out.println(" -> Thành viên B nhận TIN MỚI. Danh sách hòm thư hiện tại:");
            for (int i = 0; i < tinDaNhan.size(); i++) {
                System.out.println("    + Số thứ tự " + (i + 1) + ": " + tinDaNhan.get(i).noiDung);
            }
        } else {
            // Xử lý logic cập nhật tin cục bộ
            for (TinTuc tinCucBo : tinDaNhan) {
                if (tinCucBo.id == tin.id) {
                    tinCucBo.noiDung = tin.noiDung; // Cập nhật nội dung
                    System.out.println(" -> Thành viên B đã cập nhật thành công tin ID " + tin.id + ". Nội dung mới: " + tinCucBo.noiDung);
                    break;
                }
            }
        }
    }
}

// =================================================================
// 5. HÀM MAIN CHẠY THỬ
// =================================================================
public class Main {
    public static void main(String[] args) {
        Topic topicCNTT = new Topic();

        ThanhVienA tvA = new ThanhVienA();
        ThanhVienB tvB = new ThanhVienB();

        topicCNTT.dangKy(tvA);
        topicCNTT.dangKy(tvB);

        // Kịch bản 1: Đăng tin lần 1
        topicCNTT.taoTinMoi("Lịch thi giữa kỳ môn Design Pattern");

        // Kịch bản 2: Đăng tin lần 2
        topicCNTT.taoTinMoi("Thông báo nộp bài tập CA4");

        // Kịch bản 3: Cập nhật thông tin của tin số 1
        topicCNTT.capNhatTin(1, "Lịch thi giữa kỳ môn Design Pattern (Dời sang tuần sau)");
    }
}

Mẹo phân tích code cho sinh viên: * Ở lớp ThanhVienB (dòng 85), tôi cố ý dùng lệnh new TinTuc(...) để nhân bản dữ liệu ra (Cloning) thay vì chỉ lưu con trỏ (Reference) trực tiếp từ Topic đưa sang. Việc này để mô phỏng tính chất vật lý thực: Topic (Server) có database của nó, Thành viên B (Client) có database của mình. Khi Server đổi (hàm capNhatTin), Thành viên B phải tự đi móc trong database của mình ra để sửa lại (vòng lặp ở dòng 95) đúng như yêu cầu đề bài "cập nhật tin trong danh sách tin nhận của mình". Lối viết code này thể hiện tư duy thiết kế hệ thống phân tán (Distributed System) rất chuẩn mực!
