#Chia sẻ cách sử dụng Cloudflare hiệu quả

## Cấu hình DNS

Quan trọng đầu tiên là phần cấu hình DNS để trỏ về hosting/server. Chỉ cần 2 record này thôi là quá đủ để sử dụng và cũng dễ thay đổi sau này, đó là:

Type | Name | Value
-----|------|------
A|@|Địa chỉ IP của hosting hoặc VPS
CNAME|www|@

![Cấu hình DNS Cloudflare]
(http://i.imgur.com/Gra7g2J.jpg)

Vậy là xong, sau này nếu cần thêm sub-domain cùng chung IP như trên thì chỉ việc tạo 1 CNAME, tên là tên của sub-domain, value thì cũng là @. Tại sao lại @ mà lại không dùng record A để trỏ cho tiện? Đơn giản là vì sau này đổi hosting hoặc IP, bạn chỉ việc cấu hình lại record A @ thôi, không cần thay đổi toàn bộ các record khác dễ gây nhầm lẫn.

Bật đám mây màu cam sẽ kích hoạt Cloudflare, nếu có đứt cáp diện rộng thì chuyển nó thành màu xám là được. Lúc này Cloudflare chỉ có vai trò phân giải IP và người dùng sẽ truy cập trực tiếp vào máy chủ của bạn.

Phần DNSSEC nếu có thể thì bạn hãy bật lên luôn nhé. Cloudflare sẽ hiển thị một chuỗi cấu hình (thực ra là 1 record) để bạn setup nên nếu không rành thì bạn có thể gửi cho nhà cung cấp TÊN MIỀN, họ sẽ cài đặt cho bạn.

## Kích hoạt nén resource

Dùng Cloudflare đa số là để tối ưu website dành cho những website lượng truy cập lớn, hoặc những website có băng thông hạn chế và nhiều lí do khác. Do vậy bước thứ 2 chúng ta sẽ chuyển sang tab Speed.

Check tất cả các mục JS, CSS, HTML ở mục Minify. Dùng để nén tất cả dữ liệu của website lại (minify) nên nếu bạn nào dùng WordPress thì không nên cài plugin nào có tính năng này nữa vì như vậy thì hơi phí tài nguyên. Cloudflare đã làm hết cho chúng ta rồi.

**Tính năng Railguns** mình thấy đa số hosting có memcache sẽ hỗ trợ, VPS cũng vậy. Nếu bạn thấy tính năng này có thể bật thì bật lên nhé.

**Tính năng Rocket Loader** nói đơn giản là load JS chỉ khi load xong tất cả các nội dung khác trên trang. Do vậy khi bật lên có thể bạn sẽ thấy slide hình hiện lên chậm hơn, một số plugin hoạt động không tốt. Vì vậy bạn nên check console của trình duyệt, dạo một vòng website xem có lỗi gì xảy ra không nhé. Mình thì mình hay tắt đi.

**Tính năng Accelerated Mobile Links (AMP)** vừa được bổ sung gần đây giúp cho website load nhanh hơn, ít tốn băng thông người đọc hơn khi tìm kiếm trên Google cũng khá hay. Tuy nhiên mình nghĩ bạn nên sử dụng các plugin có chức năng tương tự để có thể tùy chỉnh nhiều hơn thay vì dùng Cloudflare.

Vậy là xong rồi, 2 thứ quan trọng nhất đã được tùy chỉnh xong. Tiếp theo sẽ có một vài tinh chỉnh khá hay mà bạn có thể làm.

==============================================================================================

## Cấu hình Page Rules

Tính năng này khá hay nhưng ít người dùng đến. Bạn có thể yêu cầu Cloudflare cache những resource **theo ý mình** thay vì theo ý của Cloudflare. Mặc dù như trả lời của support thì Cloudflare đã hỗ trợ cache tất cả các resource mặc định như JS, CSS cả rồi (tất cả có thể xem [tại đây](https://support.cloudflare.com/hc/en-us/articles/200172516-Which-file-extensions-does-CloudFlare-cache-for-static-content-)) nhưng mình thấy cách này vẫn hiệu quả nên vẫn làm theo nếu có thời gian.

Bạn có thể thêm Page Rules cho website để quản lý cache tốt hơn, ví dụ bên dưới là theo cấu trúc WordPress, thử xem sao nhé :)

![Cấu hình Page Rules]
(http://i.imgur.com/Ajb6ot4.jpg)

## Cấu hình SSL

Hiện tại Google đã có nhiều cập nhật liên quan (và có thể là ưu tiên hơn) cho những website có sử dụng HTTPS. Tuy nhiên, nếu bạn vẫn chưa sẵn sàng để chuyển sang HTTPS, hãy tắt tính năng này đi ở tab Crypto, chuyển SSL thành Off. Việc này giúp Google không index website bạn dưới dạng HTTPS, nếu có tắt Cloudflare đi cũng không ảnh hưởng gì.
