Tác giả: SyerexX Ngày: 18/10/2025 Thể loại: Forensics
Thử thách này thuộc danh mục Forensics ở mức độ dễ, yêu cầu người chơi tìm kiếm một flag được giấu bên trong một file ảnh `jpg`.
Hôm nay mình sẽ **KHÔNG** sử dụng máy ảo để giải đề nữa (không khuyến cáo vì giải đề CTF về cơ bản là tải file bị hỏng hoặc virus về máy — việc này khá nguy hiểm).
Các bước mà mình thực hiện trước khi giải đề:
* Mình mở Command Prompt dưới quyền Admin rồi nhập lệnh `wsl --install`. Khi lệnh hoàn thành, bạn có thể dùng Microsoft Store để cài Ubuntu hoặc Kali Linux và sử dụng nó như terminal của máy ảo bình thường!
* Hôm nay ta sẽ giải một đề Easy khác tên là **Hidden in plainsight** của Yahaya Meddy.

### 1. Phân tích file:
* Thử thách cho mình tải xuống một file ảnh tên là **img.jpg**.
* Khi mở file lên thì nó chỉ là một bức ảnh bình thường như bao ảnh khác.
* Trong hint tác giả cung cấp: “Download the jpg image and read its metadata” — dựa trên gợi ý này, bước đầu tiên là kiểm tra metadata của file ảnh.

### 2. Giải đề:
* Mình giải bằng cách sử dụng công cụ xem metadata của ảnh như `exiftool`:
  * Để xem metadata bằng `exiftool` ta cài nó trước bằng hai lệnh:
    ```
    sudo apt update
    sudo apt install libimage-exiftool-perl -y
    ```
  * Sau khi cài xong, dùng:
    ```
    exiftool img.jpg
    ```
  * Trong kết quả metadata mình tìm thấy một chuỗi lạ có định dạng Base64:
    `c3RlZ2hpZGU6Y0VGNmVuZHZjbVE9`
* Sau khi có chuỗi mã hóa, mình dùng `echo` để giải mã lớp đầu:
  ```
  echo c3RlZ2hpZGU6Y0VGNmVuZHZjbVE9 | base64 -d
  ```
  Kết quả là: `steghide:cEF6endvcmQ=`
  * Giải thích:
    * `echo`: in ra (print) hoặc lặp lại bất cứ thứ gì bạn đưa cho nó.
    * `|` (pipe): chuyển kết quả của lệnh trước làm đầu vào cho lệnh sau.
    * `base64 -d`: giải mã Base64, tức “lấy đoạn văn bản mã hóa Base64 này và dịch ngược nó lại thành dữ liệu gốc”.
* Giải mã lớp thứ hai để tìm mật khẩu:
  ```
  echo cEF6endvcmQ= | base64 -d
  ```
  Kết quả: `pAzzword` — đây là mật khẩu cần tìm.

### 3. Trích xuất file ẩn:
* Dùng công cụ `steghide` với mật khẩu vừa tìm để trích xuất dữ liệu ẩn từ ảnh gốc:

  ```
  steghide extract -sf img.jpg
  ```
* Khi được hỏi “Enter passphrase:” thì nhập mật khẩu: `pAzzword`

### 4. Kết quả:
* `steghide` tạo ra một file text mới, thường là `hidden.txt` hoặc `flag.txt`.
* Dùng lệnh `cat` để đọc nội dung file và tìm flag:

  ```
  cat flag.txt
  ```
* Kết quả cuối cùng là: `picoCTF{h1dd3n_1n_1m4g3_1c55ccd0}`

### 5. Những gì mình học được:
* Thử thách này giúp mình biết cách tải và dùng các công cụ chuyên dụng cho mục đích cụ thể (ví dụ `exiftool` và `steghide`), hiểu chức năng của `echo`, và ôn lại cách sử dụng pipe `|`.
* Đây cũng là một ví dụ hay về giấu tin nhiều lớp (multi-layer): nó không chỉ yêu cầu biết một công cụ mà còn phải xâu chuỗi manh mối, giải mã qua nhiều bước để tìm ra chìa khóa cuối cùng.
