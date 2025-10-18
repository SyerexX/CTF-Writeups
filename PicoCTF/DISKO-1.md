Sau khi tìm hiểu và truy cập trang web https://play.picoctf.org/ tôi bắt đầu thấy những thử thách đầu tiên:
Các bước mà mình thực hiện trước khi giải đề:
1: Tải về VMWare + sử dụng máy ảo Linux ParrotOS.
2: Sử dụng mate Terminal trên máy ảo để bắt đầu giải đề.
Để bắt đầu cuộc hành trình tôi chọn thử thách Easy mang tên DISKO 1 của Darkraicg492:
Thử thách cho tôi tải xuống 1 file disk image với tên là disko-1.dd.gz
Tôi thấy ngay rằng file đã bị nén lại bằng định dạng .gz để giải nén tôi vào terminal truy cập ổ đĩa chứa file bằng lệnh cd Downloads
Bạn có thể sử dụng lệnh "ls" để tra xem file mình cần có nằm trong ổ đó không.
Sau đó sử dụng lện gunzip "Tên file cần giải nén" vd: gunzip disko-1.dd.gz khi đã hoàn thành máy sẽ trả về file đã giải nén vd: disko-1.dd (không còn .gz nữa) bạn có thể sử dụng ls để kiểm tra.
Đến đây mình có xem hint mà tác giả cung cấp trên PicoCTF là "Maybe Strings could help? If only there was a way to do that?" VẬY LÀ ĐÃ RÕ
Mình ngay lập tức sử dụng lệnh strings tên terminal cụ thể là:
strings disko-1.dd | grep pico
Giải thích về dòng lệnh này:
strings: Dùng strings để quét toàn bộ file disko-1.dd và trích xuất tất cả các chuỗi văn bản có thể đọc được.
grep: Dùng grep để lọc ra và chỉ hiển thị duy nhất dòng nào có chứa từ khóa đặc trưng của flag là pico.
| : Dùng ký tự pipe | để chuyển toàn bộ kết quả của strings làm đầu vào cho lệnh grep.
Kết quả ta nhận được nhiều dòng nhưng nổi bật nhất là: picoCTF{1t5_ju5t_4_5tr1n9_c63b02ef} là kết quả mà đề bài cần!
Thử thách này cho ta biết rõ hơn cách sử dụng các lệnh cơ bản trên linux như cd, ls, gunzip, strings, grep và |.
Bằng cách sử dụng strings và grep, chúng ta có thể nhanh chóng phân tích một file nhị phân lớn và tìm ra thông tin cần thiết mà không cần các thao tác phức tạp.
