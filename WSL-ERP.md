# Hướng Dẫn Cài Đặt WSL cho ERP

## 1. Cài đặt Docker Desktop

-   Tải và cài đặt Docker Desktop từ trang chính thức: [Docker](https://www.docker.com/).
-   Mở Docker sau khi cài đặt thành công.

## 2. Cài đặt Ubuntu trên WSL

-   Mở **Microsoft Store** và tìm kiếm **Ubuntu**.
-   Cài đặt phiên bản Ubuntu mong muốn.
-   Sau khi cài đặt, mở Ubuntu và thiết lập **username** cùng **password**.

## 3. Kiểm tra và cài đặt Docker trên Ubuntu

-   Kiểm tra Docker đã được cài đặt bằng lệnh:

    ```sh
    docker --version
    ```

-   Nếu Docker chưa được cài đặt, hãy cài đặt bằng lệnh sau:

    ```sh
    sudo apt update && sudo apt install -y docker.io
    ```

## 4. Cài đặt Git và Node.js

-   **Git:**

    ```sh
    sudo apt update && sudo apt install -y git
    git config --global user.name "Tên Của Bạn"
    git config --global user.email "email@example.com"
    ```

-   **Node.js:**

    ```sh
    curl -fsSL [https://deb.nodesource.com/setup_lts.x](https://deb.nodesource.com/setup_lts.x) | sudo -E bash -
    sudo apt install -y nodejs
    ```

## 5. Cấu hình Docker với WSL

-   Vào **Settings → Resources → WSL Integration**.
-   Kích hoạt **WSL Integration** cho Ubuntu đã cài đặt.
-   Nhấn **Apply & Restart**.

    ![WSL Integration in Docker Desktop](https://github.com/user-attachments/assets/d6869b8a-fb85-4db0-9870-0f8eeb068895)

## 6. Truy cập File Ubuntu WSL từ Windows

-   Sử dụng lệnh sau để map ổ đĩa WSL vào Windows:

    ```sh
    net use Z: \\wsl$\Ubuntu-{version}
    ```

    (Thay `{version}` bằng phiên bản Ubuntu của bạn, ví dụ: `Ubuntu-22.04`)

## 7. Clone Source Code và Cấu Hình (Từ máy ảo Ubuntu)

-   **Tạo Personal Access Token (PAT) trên GitHub:**
    -   Truy cập GitHub Developer Settings: [https://github.com/settings/tokens](https://github.com/settings/tokens).
    -   Chọn "Generate new token (classic)".
    -   Chọn quyền:
        -   ✅ `repo` (để truy cập private repository).
        -   ✅ `workflow` (nếu cần truy cập GitHub Actions).
    -   Nhấn "Generate token" và sao chép token.

-   **Clone repository:**

    ```sh
    git clone [https://github.com/thk-vn/erp.git](https://github.com/thk-vn/erp.git)
    ```

    -   Nhập username và sau đó nhập PAT khi được yêu cầu.

-   **Lưu PAT để không cần nhập lại:**

    ```sh
    git config --global credential.helper store
    ```

    -   Sau đó chạy `git pull` và nhập PAT một lần, Git sẽ lưu lại.

-   **Kiểm tra và xóa credential (nếu cần):**

    -   Kiểm tra xem token còn hạn không. Nếu hết hạn, sử dụng lệnh sau để xóa credential:

        ```sh
        git credential reject [https://github.com](https://github.com)
        ```

    -   Sau đó, cập nhật lại bằng PAT mới bằng cách chạy lại lệnh `git pull` và nhập PAT mới.

## 8. Xử lý File `artisan` và Chạy Docker Compose từ Windows

-   **Xử lý file `artisan` bị thay đổi khi commit:**

    -   Nếu bạn gặp lỗi liên quan đến file `artisan` khi commit, có thể do:
        -   Đã chạy lệnh `php artisan`, làm thay đổi metadata của file.
        -   WSL và Windows có khác biệt về quyền file, khiến Git nhận diện file `artisan` là đã bị sửa.
    -   Để khắc phục, hãy thực hiện các lệnh sau:

        ```sh
        git diff back-end/artisan
        git config core.fileMode false
        git status
        ```

        -   `git diff back-end/artisan`: Hiển thị sự khác biệt của file `artisan`.
        -   `git config core.fileMode false`: Bỏ qua việc kiểm tra quyền file.
        -   `git status`: Kiểm tra lại trạng thái các file.

-   **Chạy Docker Compose từ Windows:**

    -   Để chạy Docker Compose trực tiếp từ Windows, hãy thực hiện các bước sau:
        1.  Mở Command Prompt hoặc PowerShell.
        2.  Di chuyển đến thư mục `erp/back-end`:

            ```sh
            cd path/to/erp/back-end
            ```

            (Thay `path/to/erp` bằng đường dẫn thực tế đến thư mục `erp` của bạn)
        3.  Chạy lệnh sau để khởi động Docker Compose:

            ```sh
            docker compose up -d --build
            ```

    -   Sau khi chạy Docker Compose, có thể tiếp tục setup source code như bình thường.

**Lưu ý:**

* Việc chạy Docker Compose từ Windows có thể giúp bạn tiết kiệm tài nguyên hệ thống so với việc chạy từ WSL.
* Hãy đảm bảo bạn đã cài đặt Docker Desktop và cấu hình WSL Integration (bước 5) trước khi chạy Docker Compose từ Windows.
