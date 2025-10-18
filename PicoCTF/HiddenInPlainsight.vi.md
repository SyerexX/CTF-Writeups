Tác giả: SyerexX Ngày: 18/10/2025 Thể loại: Forensics
Thử thách này thuộc danh mục Forensics ở mức độ dễ, yêu cầu người chơi tìm kiếm một flag được giấu bên trong một file ảnh .jpg.

Hôm nay mình sẽ KHÔNG sử dụng máy ảo để giải đề nữa (Không khuyến cáo vì giải đề CTF về cơ bản là tải corrupted file hoặc Virus vào máy rồi việc này khá nguy hiểm)
Các bước mà mình thực hiện trước khi giải đề:
 * Mình vào Command Prompt dưới quyền Admin sau đó nhập lệnh 'wsl --install' khi đã hoàn thành bạn có thể sử dụng Microsoft store để cài đặt Ubuntu hay Kali linux và sử dụng nó như terminal của máy ảo bình thường!
 * Hôm nay ta sẽ giải một đề Easy khác tên là Hidden in plainsight của Yahaya Meddy:

### 1. Phân tích file:
 * Thử thách cho tôi tải xuống 1 file image với tên là img.jpg
 * Khi bạn mở file lên thì nó chỉ là một bức ảnh bình thường như bao ảnh khác
 * Ở hint mà tác giả cung cấp là "Download the jpg image and read its metadata" dựa trên gợi ý, bước đầu tiên là kiểm tra metadata của file ảnh.
  
### 2. Giải đề:
 * Mình giải bằng cách sử dụng công cụ xem metadata của ảnh như exiftool:
   + để xem metadata của ảnh bằng công cụ exiftool ta đầu tiên cài đặt nó bằng hai lệnh sau
  'sudo apt update'
  'sudo apt install libimage-exiftool-perl -y'
   + Khi đã cài đặt thành công ta sử dụng lệnh exiftool "ten_file_anh.jpg" Vd: exiftool img.jpg
   + Khi đã hoàn thành ta sẽ được toàn bộ thông tin về file ảnh ta sẽ tìm thấy một chuỗi ký tự lạ, có định dạng của mã hóa Base64: c3RlZ2hpZGU6Y0VGNmVuZHZjbVE9
 * Sau khi có được chuỗi mã hóa ta sử dụng lệnh echo để giải mã lớp đầu tiên:
   'echo c3RlZ2hpZGU6Y0VGNmVuZHZjbVE9 | base64 -d
   Bạn sẽ được kết quả là: steghide:cEF6endvcmQ=
   Giải thích câu lệnh:
   echo: Chính xác thì nó là cout của c++ nhiệm vụ chính của nó rất đơn giản: in ra (print) hoặc lặp lại bất cứ thứ gì bạn đưa cho nó.
   | : hay còn gọi là pipe nó giúp chuyển kết quả của lệnh trước làm đầu vào cho lệnh sau nó.
   base64 -d: là lệnh yêu cầu máy tính "Hãy lấy đoạn văn bản mã hóa Base64 này và dịch ngược nó lại thành dữ liệu gốc ban đầu."
 * Giải mã lớp thứ hai để tìm mật khẩu
   + Sau khi có mã steghide:cEF6endvcmQ= ta tiếp tục sử dụng lệnh echo để giải tiếp đoạn mã hóa định dạng base64:
   'echo cEF6endvcmQ= | base64 -d'
   + Sau khi giải ta được dòng chữ sau pAzzword đây là mật khẩu mà ta đang tìm!

### 3. Trích xuất File ẩn:
 * Ta sẽ sử dụng công cụ steghide với mật khẩu vừa tìm được để trích xuất dữ liệu bị giấu từ file ảnh gốc.
 * steghide extract -sf <tên-file-ảnh.jpg> VD: steghide extract -sf img.jpg
 * Khi được hỏi Enter passphrase: ta nhập mật khẩu đã giải: pAzzword
### 4. Kết quả:
 * steghide tạo ra một file text mới thường là hidden.txt hoặc flag.txt
 * Ta sử dụng lệnh cat để đọc nội dung file này và tìm thấy flag cuối cùng
'cat flag.txt'
 * Ta sẽ có được câu trả lời cuối cùng: picoCTF{h1dd3n_1n_1m4g3_1c55ccd0}
### 4. Những gì mình học được:
 * Thử thách này giúp mình biết được cách tải và sử dụng các công cụ chuyên dụng cho một mục đích vd như exiftool và steghide biết được chức năng của echo, đổng thời ôn lại cách sử dụng của pipe |.
 * Thử thách này cũng là một ví dụ tuyệt vời về giấu tin đa lớp (multi-layer). Nó không chỉ yêu cầu người chơi biết cách dùng một công cụ, mà còn phải biết xâu chuỗi các manh mối, giải mã nhiều bước để tìm ra chìa khóa cuối cùng.
