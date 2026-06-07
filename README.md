# 🛒 Hệ Thống Đấu Giá Trực Tuyến (Online Auction System)
![Status](https://img.shields.io/badge/Status-Active-success?style=for-the-badge)
![Platform](https://img.shields.io/badge/Platform-Windows%20%7C%20Linux%20%7C%20macOS-lightgrey?style=for-the-badge)
![Java](https://img.shields.io/badge/Java-21-ED8B00?style=for-the-badge&logo=openjdk&logoColor=white)
![Spring Boot](https://img.shields.io/badge/Spring_Boot-4.0.5-6DB33F?style=for-the-badge&logo=spring-boot&logoColor=white)
![Reactor](https://img.shields.io/badge/Reactor-Reactive-6DB33F?style=for-the-badge)
![JavaFX](https://img.shields.io/badge/JavaFX-21.0.2-1565C0?style=for-the-badge)
![Gradle](https://img.shields.io/badge/Gradle-Build-02303A.svg?style=for-the-badge&logo=Gradle&logoColor=white)
![Maven](https://img.shields.io/badge/Maven-3.11.0-C71A36?style=for-the-badge&logo=Apache%20Maven&logoColor=white)
![SQLite](https://img.shields.io/badge/SQLite-Database-003B57.svg?style=for-the-badge&logo=sqlite&logoColor=white)
[![Spring Boot CI](https://github.com/dhlw123/auctionbackendCS1/actions/workflows/ci.yml/badge.svg)](https://github.com/dhlw123/auctionbackendCS1/actions/workflows/ci.yml)
## 1. Mô tả ngắn gọn bài toán và phạm vi hệ thống
Dự án là một hệ thống đấu giá trực tuyến hoạt động theo mô hình Client-Server [3], cho phép nhiều người dùng cùng tham gia cạnh tranh giá để mua sản phẩm trong một khoảng thời gian xác định [4, 5]. 
Hệ thống phân quyền người dùng với 3 vai trò chính:
*   **Admin:** Quản lý toàn bộ hệ thống, kiểm duyệt và cấm/mở khóa người dùng [6-8].
*   **Seller:** Đăng bán vật phẩm, thiết lập giá khởi điểm và thời gian phiên đấu giá [6].
*   **Bidder:** Tham gia đặt giá (Bid) cạnh tranh, hỗ trợ các tính năng như đấu giá tự động (Auto-bidding) [8, 9].

## 2. Công nghệ sử dụng và Yêu cầu cài đặt
*   **Backend (Máy chủ):** Ngôn ngữ Java, Framework **Spring Boot**, Spring Data JPA, bảo mật JWT [8, 10-16]. Sử dụng công cụ build là **Gradle** [16].
*   **Frontend (Máy khách):** Ngôn ngữ Java, **JavaFX** (sử dụng `.fxml` cho UI), thư viện Retrofit (`ApiClient`) để gọi RESTful API [10, 17-22]. Sử dụng công cụ build là **Maven** [22].
*   **Cơ sở dữ liệu:** SQLite [10].
*   **Yêu cầu cài đặt:** Cài đặt Java Development Kit (JDK) phiên bản 17 trở lên.

## 3. Cấu trúc thư mục và Modules chính
Dự án được chia làm hai phần tách biệt: Backend và Frontend.

**Phần Backend (`/src/main/java/com/auction`)** [8, 11-16]:
*   `admin`, `users`, `auth`: Quản lý người dùng, xác thực JWT và phân quyền hệ thống.
*   `items`, `itemstatus`: Quản lý vòng đời sản phẩm (OPEN, RUNNING, FINISHED).
*   `bids`, `auctionorchestration`: Xử lý logic đặt giá và đấu giá tự động.
*   **Real-time Sinks:** `ItemPricesSink.java` và `UserBalanceSink.java` phát luồng dữ liệu thời gian thực (SSE) [13, 16].

**Phần Frontend (`/src/main/java/` và `/src/main/resources/`)** [7, 18-25]:
*   `controller`: Chứa các controller UI của JavaFX (`AdminPage`, `BidderViewPage`, `SellerViewPage`,...) [7, 23].
*   `network`: Xử lý kết nối, giao tiếp qua API RESTful (`ApiClient`) và duy trì luồng dữ liệu (`PriceStreamListener`, `BalanceStreamListener`) [18].
*   `service`: Xử lý callback và nghiệp vụ từ luồng.
*   `resources/fxml`: Các file giao diện người dùng.

## 4. Hướng dẫn khởi chạy chương trình
**Lưu ý quan trọng:** Cần phải khởi chạy máy chủ (Server) trước, sau đó mới khởi chạy máy khách (Client) [2].

### Bước 1: Khởi chạy Server (Backend)
Mở terminal / command prompt tại thư mục gốc của backend và chạy lệnh sau:
*   **Trên Linux / MacOS:** 
    ```bash
    ./gradlew bootRun
    ```
*   **Trên Windows:**
    ```cmd
    gradlew.bat bootRun
    ```

### Bước 2: Khởi chạy Client (Frontend)
Mở một terminal / command prompt khác tại thư mục gốc của frontend và chạy lệnh:
*   **Trên tất cả các hệ điều hành (Linux / MacOS / Windows):**
    ```bash
    mvn javafx:run
    ```
*(Để kiểm thử tính năng tranh giá đồng thời, hãy mở nhiều terminal khác nhau và chạy lặp lại lệnh trên để tạo nhiều cửa sổ Client cùng lúc [2])*

## 5. Danh sách chức năng đã hoàn thành
Hệ thống đã hoàn thiện xuất sắc toàn bộ các chức năng bắt buộc và các chức năng nâng cao theo đúng barem điểm [26-28]:
*   ✅ **Chức năng Bắt buộc:**
    *   Quản lý đăng ký / đăng nhập (JWT), cấp quyền Admin/Seller/Bidder [6].
    *   Quản lý sản phẩm đấu giá (Thêm, sửa, xóa, thiết lập thời gian) [6, 29].
    *   Tự động khóa phiên đấu giá khi hết giờ, xác định người thắng [29, 30].
    *   Xử lý các ngoại lệ (lỗi kết nối, đặt giá thấp hơn giá hiện hành) [30, 31].
    *   Giao diện đồ họa (GUI) đầy đủ bằng JavaFX cho từng loại người dùng [31].
*   🔥 **Chức năng Kỹ thuật & Nâng cao:**
    *   **Xử lý Đấu giá Đồng thời (Concurrent Bidding):** Tận dụng luồng của Spring Boot và Database Lock để giải quyết bài toán tranh chấp (ngăn chặn lost update, loại bỏ race condition) [9, 26, 32].
    *   **Cập nhật Thời gian thực (Realtime Update):** Sử dụng SSE (Server-Sent Events) kết hợp thiết kế Observer Pattern nâng cao để tự động đẩy luồng giá/số dư trực tiếp đến Client mà không sử dụng polling gây quá tải [26, 33, 34].
    *   **Auto-Bidding:** Hỗ trợ thiết lập giá thầu tối đa (`maxBid`) và bước giá (`increment`), hệ thống tự động tranh giá thay mặt người dùng [8, 9, 27].
    *   **Lịch sử Giá (Bid History):** Ghi nhận và lưu lại luồng thời gian thực của các lượt bid [24, 28, 35].

## 6. Tài nguyên Báo cáo & Video Demo
*(Giảng viên vui lòng truy cập các đường dẫn dưới đây để xem báo cáo kiến trúc và video chạy thực tế các chức năng phức tạp của hệ thống)* [2]:

*   **Báo cáo PDF:** `[Chèn Link Google Drive chứa file báo cáo PDF (tối đa 6 trang) vào đây]`
*   **Video Demo hệ thống (Dưới 4 phút):** `[Chèn Link Google Drive chứa file Video vào đây]`
