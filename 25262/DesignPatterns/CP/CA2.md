Chào bạn. Bài tập CA2 này là một ví dụ vô cùng kinh điển và tuyệt vời để dạy về Observer Pattern, bởi nó mô phỏng chính xác kiến trúc Lập trình hướng sự kiện (Event-driven programming) mà các framework làm giao diện UI (như Android, Java Swing hay .NET WPF) đang sử dụng dưới nền tảng.
Dưới đây là sơ đồ lớp và mã nguồn Java, tôi đã bổ sung các comment giải thích cặn kẽ từng vai trò theo lý thuyết để sinh viên có thể ánh xạ trực tiếp từ nguyên lý sang code.
1. Sơ đồ lớp (PlantUML)
Sơ đồ này thể hiện rõ việc Button không hề biết Activity là ai. Nó chỉ giao tiếp thông qua một "bản hợp đồng" là interface OnClickListener.
@startuml
skinparam classAttributeIconSize 0

' --- Giao diện cốt lõi của Observer ---
interface OnClickListener {
    + onClick(): void
}

' --- Đóng vai trò Subject ---
class Button {
    - listeners: List<OnClickListener>
    + attach(observer: OnClickListener): void
    + click(): void
}

' --- Đóng vai trò Concrete Observer ---
class Activity {
    - soLanClick: int
    + Activity()
    + onClick(): void
}

' --- Mối quan hệ ---
' Button chứa danh sách những người lắng nghe nó
Button o--> "0..*" OnClickListener : notifies >

' Activity cam kết thực hiện đúng hợp đồng lắng nghe
Activity ..|> OnClickListener : implements

' Theo đề bài: Button nằm trong Activity
Activity *--> "1..*" Button : contains >

note right of Button: Đóng vai trò Subject\n(Nguồn phát sinh sự kiện)
note right of Activity: Đóng vai trò Observer\n(Người lắng nghe sự kiện)
@enduml

2. Mã nguồn Java minh họa (Có comment giải thích chi tiết)
Code này có thể chạy trực tiếp. Các comment được viết sát với ngôn ngữ giảng dạy để bạn dễ dàng giải thích cho sinh viên trên lớp.
import java.util.ArrayList;
import java.util.List;

// =================================================================
// BƯỚC 1: ĐỊNH NGHĨA GIAO DIỆN OBSERVER (NGƯỜI QUAN SÁT)
// =================================================================
/**
 * Interface OnClickListener đóng vai trò là "Observer" trong Mẫu thiết kế.
 * Bất kỳ lớp nào muốn nhận sự kiện click từ Button đều PHẢI implement interface này.
 */
interface OnClickListener {
    // Phương thức này sẽ được gọi khi có sự kiện xảy ra
    void onClick();
}

// =================================================================
// BƯỚC 2: XÂY DỰNG SUBJECT (NGUỒN PHÁT SỰ KIỆN)
// =================================================================
/**
 * Lớp Button đóng vai trò là "Subject" (Chủ thể phát sự kiện).
 * Chú ý: Button KHÔNG HỀ BIẾT Activity là gì. Nó chỉ giữ một danh sách 
 * những ai đang đăng ký lắng nghe nó (thông qua interface OnClickListener).
 */
class Button {
    // Danh sách các đối tượng Observer đang theo dõi Button này
    private List<OnClickListener> observers = new ArrayList<>();

    // Phương thức để một Observer đăng ký theo dõi (trong GoF gọi là Attach)
    // Thường trong UI hay gọi là setOnClickListener hoặc addOnClickListener
    public void attach(OnClickListener observer) {
        observers.add(observer);
    }

    // Hành động mô phỏng người dùng bấm vào nút
    public void click() {
        // Khi Subject có sự thay đổi (bị click), nó sẽ lặp qua danh sách
        // và thông báo (Notify) cho TẤT CẢ những người đang theo dõi.
        for (OnClickListener observer : observers) {
            observer.onClick(); 
        }
    }
}

// =================================================================
// BƯỚC 3: XÂY DỰNG CONCRETE OBSERVER (NGƯỜI QUAN SÁT CỤ THỂ)
// =================================================================
/**
 * Lớp Activity mô phỏng một màn hình ứng dụng, đóng vai trò "Concrete Observer".
 * Nó implement OnClickListener để có thể phản hồi lại khi Button bị click.
 */
class Activity implements OnClickListener {
    // Biến trạng thái nội bộ để lưu số lần đã click theo yêu cầu đề bài
    private int soLanClick = 0;
    
    // Activity chứa một Button bên trong nó (Composite)
    private Button myButton;

    public Activity() {
        // Khởi tạo giao diện (tạo nút)
        myButton = new Button();
        
        // QUAN TRỌNG NHẤT: Activity tự đăng ký chính nó (this) 
        // làm người lắng nghe (Observer) cho cái nút (Subject).
        myButton.attach(this);
    }

    // Cung cấp hàm để Main có thể tương tác giả lập click
    public void moPhongNguoiDungBamNut() {
        myButton.click();
    }

    // =============================================================
    // ĐÂY LÀ NƠI XỬ LÝ SỰ KIỆN (Thực thi hợp đồng từ interface)
    // =============================================================
    @Override
    public void onClick() {
        // Mỗi lần Subject (Button) gọi hàm này, ta tăng biến đếm lên 1
        soLanClick++;
        // In ra màn hình theo đúng yêu cầu đề bài
        System.out.println("Bạn click lần thứ " + soLanClick);
    }
}

// =================================================================
// BƯỚC 4: CHƯƠNG TRÌNH CHÍNH (KIỂM THỬ MÔ PHỎNG)
// =================================================================
public class Main {
    public static void main(String[] args) {
        // Khởi tạo một màn hình (Activity).
        // Ngầm định bên trong, Activity đã tạo Button và đăng ký Observer xong xuôi.
        Activity mainActivity = new Activity();

        // Mô phỏng người dùng liên tục bấm vào nút trên màn hình
        mainActivity.moPhongNguoiDungBamNut();
        mainActivity.moPhongNguoiDungBamNut();
        mainActivity.moPhongNguoiDungBamNut();
    }
}

Mẹo giảng dạy cho bài này: Điểm đắt giá nhất để sinh viên hiểu bài là chỉ cho các bạn thấy ở dòng số 45 trong hàm click() của Button (observer.onClick();). Bạn có thể hỏi sinh viên: "Lớp Button gọi hàm onClick(), nhưng nó có biết dòng chữ 'Bạn click lần thứ n' được in ra không?". Câu trả lời là Không. Nó chỉ biết là nó đang vỗ vai những thằng đăng ký theo dõi nó, còn những thằng đó làm gì (in ra màn hình, đổi màu chữ, hay phát nhạc) thì Button hoàn toàn không quan tâm. Đó chính là sự tách biệt (loose coupling) làm nên sức mạnh của Observer.
