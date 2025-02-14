## Báo cáo lỗ hổng bảo mật: Rò rỉ cơ sở dữ liệu ##
__Người phát hiện:__ Dương Quang Hào, Đinh Thái Sơn 

__Ngày phát hiện:__ 12/11/2024

### 1.Tóm tắt 
Trong quá trình kiểm thử bảo mật, tôi đã phát hiện một số các subdomain của hocmai.vn. Trong  đó có subdomain: live-api.hocmai.vn. Trang web này có bật chế độ PHP Debugbar, có chứa  version, các log bao gồm URL, địa chỉ IP, thời gian,.. 

### 2.Mức độ nghiêm trọng: Cao 
- Tác động: Truy cập trái phép vào cơ sở dữ liệu và có khả năng đọc/ghi dữ liệu. 
- Khả năng khai thác: Dễ dàng khai thác bởi các hacker chỉ cần sử dụng các công cụ tự động  hoặc các lệnh cơ bản. 

### 3.Mô tả chi tiết lỗ hổng 
Lỗ hổng bắt nguồn từ chế độ debug, từ các log trên, tôi truy cập URL https://live api.hocmai.vn/api_hocmai/app/rate/current_rate mới method GET.
Khi cuộn chuột xuống dưới sẽ thấy các thông tin env như sau:


Tiến hành fuzz trang https://live-api.hocmai.vn tìm thấy được path /pma (Tương ứng  PhpMyAdmin) 

Sau đó sử dụng cred vào trang web này để đăng nhập, vì tài khoản này là user không có quyền  root nên không thể leo quyền tuy nhiên vẫn có thể ảnh hưởng đến Database bằng việc thêm xóa  sửa (ảnh hưởng nghiêm trọng hệ thống)  

Dưới đây là 2 file SQL đã được leak về (Sẽ được xóa sau khi mail này được xác nhận từ phía  hocmai.vn đồng thời được vá)


Trong report này sẽ chỉ cập nhật về việc leak Database thông qua Debug mode của Laravel, tuy nhiên còn xuất hiện 1 số nguy cơ RCE vào hệ thống để chiếm quyền kiểm soát và có nguy cơ lộ lọt cookie của Admin dẫn đến có thể truy cập được vào /admin/dashboard. 
Cảm ơn quản trị viên đã đọc report này, nếu có thắc mắc gì vui lòng liên hệ qua email!
