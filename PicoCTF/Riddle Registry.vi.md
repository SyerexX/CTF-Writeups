### **Riddle Registry của Prince Niyonshuti N.**

  * **Tác giả:** SyerexX
  * **Ngày:** 19/10/2025
  * **Thể loại:** Forensics

Sau khi chinh phục các thử thách trước, mình tiếp tục với một bài "Easy" khác trong danh mục Forensics. Thật bất ngờ khi mình nhận ra tác giả của thử thách này lại là **Prince Niyonshuti N.**, người đã tạo ra bài "M1n10n'5\_53cr37" khá hóc búa. Điều này làm mình rất hứng thú xem thử thách lần này sẽ có "bẫy" gì.

-----

### **1. Phân tích & Trinh sát**

Thử thách cung cấp một file duy nhất: `confidential.pdf`.

Đúng như tên gọi "confidential" (bí mật), khi mở file PDF lên, nội dung của rối rắm vô khá vô nghĩa. Đây là một dấu hiệu rõ ràng rằng thông tin không nằm ở bề mặt và Hint của tác giả cũng đã chứng minh điều đó!

Với một thử thách Forensics liên quan đến file, reflex đầu tiên của mình là kiểm tra "siêu dữ liệu" (metadata) - những thông tin ẩn đính kèm với file.

-----

### **2. Tìm kiếm Manh mối**

Mình sử dụng công cụ `exiftool` quen thuộc để "soi" file PDF.

```bash
# Di chuyển đến thư mục chứa file
cd /mnt/c/Users/Admin/Downloads

# Dùng exiftool để đọc metadata
exiftool confidential.pdf
```

Kết quả trả về một danh sách các thông tin của file. Và ngay lập tức, một dòng gây chú ý:

```
Author : cGljb0NURntwdXp6bDNkX20zdGFkYXRhX2YwdW5kIV8wZTJkZTVhMX0=
```

Trường `Author` (Tác giả) chứa một chuỗi ký tự dài. Dựa vào các dấu hiệu đã quá quen thuộc (chữ hoa, chữ thường, số, và kết thúc bằng dấu `=`), mình nhận ra ngay đây chính là mã hóa **Base64**.

-----

### **3. Giải mã & Kết quả**

Không còn nghi ngờ gì nữa, mình sử dụng ngay chuỗi lệnh `echo` và `pipe` để giải mã chuỗi Base64 này.

```bash
echo 'cGljb0NURntwdXp6bDNkX20zdGFkYXRhX2YwdW5kIV8wZTJkZTVhMX0=' | base64 -d
```

Và flag cuối cùng đã hiện ra một cách gọn gàng:

**`picoCTF{puzzl3d_m3tadata_f0und!_0e2de5a1}`**

-----

### **4. Bài học rút ra**

  * **Sức mạnh của Phản xạ:** Thử thách này cho thấy khi đã có kinh nghiệm, bạn có thể hình thành "phản xạ có điều kiện". `File -> Metadata -> Chuỗi lạ -> Base64 -> Giải mã`. Toàn bộ quá trình diễn ra rất nhanh chóng.
  * **`exiftool` là người bạn thân:** Đây là một lần nữa khẳng định `exiftool` là một công cụ không thể thiếu trong bộ đồ nghề Forensics.
  * **Tự tin:** Điều tuyệt vời nhất là mình đã có thể tự mình áp dụng các kỹ năng đã học từ các bài trước để giải quyết bài này một cách trôi chảy mà không cần bất kỳ gợi ý nào mình rất vui vì cảm nhận rõ được sự tiến bộ của bản thân.
