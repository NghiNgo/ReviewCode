Chào bạn. Bài tập CA6 này là một bài toán cực kỳ kinh điển trong lập trình Game, và đặc biệt câu hỏi số 2 là một "câu hỏi bẫy" mang tính sư phạm rất cao để kiểm tra tư duy thiết kế mở rộng (Open/Closed Principle) của sinh viên.
Góc nhìn sư phạm cho Câu 2:
Nhiều sinh viên khi làm câu 1 sẽ thiết kế hàm cập nhật kiểu: capNhat(int thoiGian, int countdown, int grade).
Nhưng khi sang câu 2, nếu thêm level và mucThuong, sinh viên sẽ phải đi sửa lại Interface, sửa lại hàm capNhat, dẫn đến phá vỡ kiến trúc cũ.
-> Giải pháp (Design Pattern): Vẫn là Observer Pattern, nhưng chúng ta áp dụng mô hình "Truyền chính đối tượng Subject (Pull Model / Context Object)". Nghĩa là hàm cập nhật chỉ nhận vào duy nhất một tham số là object PlayerData. Khi PlayerData bổ sung thêm 100 thuộc tính mới trong tương lai, cái "bản hợp đồng" Interface hoàn toàn không bị ảnh hưởng!
Dưới đây là sơ đồ và mã nguồn giải quyết trọn vẹn cả 2 câu:
1. Sơ đồ lớp (PlantUML)
Sơ đồ này thể hiện rõ việc Dashboard phụ thuộc vào IObserver, và khi nhận cập nhật, nó nhận toàn bộ cục dữ liệu PlayerData để tự bóc tách thông tin nó cần.
@startuml
skinparam classAttributeIconSize 0

' --- Giao diện Observer ---
interface IObserver {
    + update(data: PlayerData): void
}

' --- Subject (Nguồn dữ liệu Game) ---
class PlayerData {
    - observers: List<IObserver>
    ' Thuộc tính cơ bản (Câu 1)
    - thoiGian: int
    - countdown: int
    - grade: int
    ' Thuộc tính mở rộng (Câu 2)
    - level: int
    - mucThuong: int

    + attach(obs: IObserver): void
    + notifyObservers(): void
    + setGameData(thoiGian: int, countdown: int, grade: int): void
    + setLevelData(level: int, mucThuong: int): void
    ' Các Getter...
}

' --- Concrete Observer (Giao diện hiển thị) ---
class Dashboard {
    + update(data: PlayerData): void
}

' --- Mối quan hệ ---
PlayerData o--> "0..*" IObserver : "thông báo"
Dashboard ..|> IObserver : "implements"
Dashboard ..> PlayerData : "đọc dữ liệu (Pull)"

note right of PlayerData: Subject:\nChứa logic thay đổi điểm số, thời gian...
note bottom of Dashboard: Observer:\nNhận nguyên object PlayerData\nđể tương thích với mọi bản cập nhật sau này.
@enduml

2. Mã nguồn Java minh họa (Giải quyết cả Câu 1 & 2)
import java.util.ArrayList;
import java.util.List;

// =================================================================
// BƯỚC 1: ĐỊNH NGHĨA GIAO DIỆN OBSERVER
// =================================================================
/**
 * BÍ QUYẾT GIẢI CÂU 2: Thay vì truyền từng tham số (thoiGian, grade...),
 * ta truyền hẳn đối tượng PlayerData vào hàm update. 
 * Nhờ vậy, khi PlayerData thêm thuộc tính mới (level, mucThuong...), 
 * interface này KHÔNG BỊ THAY ĐỔI (Đảm bảo Open/Closed Principle).
 */
interface IObserver {
    void update(PlayerData data);
}

// =================================================================
// BƯỚC 2: LỚP SUBJECT (PLAYER DATA)
// =================================================================
class PlayerData {
    // Danh sách các màn hình đang theo dõi dữ liệu người chơi
    private List<IObserver> observers = new ArrayList<>();

    // --- Thuộc tính ban đầu (Câu 1) ---
    private int thoiGian;
    private int countdown;
    private int grade;

    // --- Thuộc tính bổ sung sau này (Câu 2) ---
    private int level;
    private int mucThuong;

    public void attach(IObserver obs) {
        observers.add(obs);
    }

    // Hàm thông báo cho toàn bộ các Dashboard
    private void notifyObservers() {
        for (IObserver obs : observers) {
            obs.update(this); // Truyền CHÍNH NÓ (this) đi
        }
    }

    // Mô phỏng sự kiện trong game làm thay đổi dữ liệu cơ bản
    public void setGameData(int thoiGian, int countdown, int grade) {
        this.thoiGian = thoiGian;
        this.countdown = countdown;
        this.grade = grade;
        notifyObservers(); // Dữ liệu đổi -> Báo cho Dashboard cập nhật
    }

    // Mô phỏng sự kiện chuyển Level (Thêm dữ liệu mới)
    public void setLevelData(int level, int mucThuong) {
        this.level = level;
        this.mucThuong = mucThuong;
        notifyObservers(); // Dữ liệu đổi -> Báo cho Dashboard cập nhật
    }

    // --- Các hàm Getter để Dashboard lấy dữ liệu hiển thị ---
    public int getThoiGian() { return thoiGian; }
    public int getCountdown() { return countdown; }
    public int getGrade() { return grade; }
    public int getLevel() { return level; }
    public int getMucThuong() { return mucThuong; }
}

// =================================================================
// BƯỚC 3: LỚP CONCRETE OBSERVER (DASHBOARD)
// =================================================================
class Dashboard implements IObserver {
    
    @Override
    public void update(PlayerData data) {
        System.out.println("=== [DASHBOARD] CẬP NHẬT MÀN HÌNH ===");
        System.out.println("Thời gian còn lại : " + data.getThoiGian() + "s");
        System.out.println("Số lượt còn lại   : " + data.getCountdown());
        System.out.println("Điểm số (Grade)   : " + data.getGrade());
        
        // Hiển thị thêm các thuộc tính mở rộng nếu game đã lên Level
        if (data.getLevel() > 0) {
            System.out.println(">> Level hiện tại : " + data.getLevel());
            System.out.println(">> Mức thưởng     : " + data.getMucThuong());
        }
        System.out.println("=====================================\n");
    }
}

// =================================================================
// BƯỚC 4: HÀM MAIN CHẠY THỬ
// =================================================================
public class Main {
    public static void main(String[] args) {
        // 1. Khởi tạo dữ liệu người chơi và giao diện
        PlayerData player = new PlayerData();
        Dashboard mainDashboard = new Dashboard();

        // 2. Giao diện đăng ký lắng nghe sự thay đổi từ dữ liệu
        player.attach(mainDashboard);

        System.out.println("--- GAME BẮT ĐẦU (Câu 1) ---");
        // Khi setGameData được gọi, Dashboard sẽ tự động in ra màn hình
        player.setGameData(120, 3, 500);

        System.out.println("--- NGƯỜI CHƠI NHẶT ĐƯỢC VẬT PHẨM ---");
        player.setGameData(100, 3, 650);

        System.out.println("--- QUA MÀN! CHUYỂN LEVEL MỚI (Câu 2) ---");
        // Gọi hàm cập nhật tính năng mới, kiến trúc Observer không hề bị phá vỡ
        player.setLevelData(2, 1000);
    }
}

Với cấu trúc như trên, bạn vừa giúp sinh viên hoàn thành bài tập, vừa "tiện tay" dạy luôn cho các bạn ấy khái niệm Push Model vs. Pull Model trong Observer Pattern.
Bạn có muốn tiếp tục với một bài tập nào khác, hay chuyển sang một Pattern mới như Decorator hoặc Strategy không?
