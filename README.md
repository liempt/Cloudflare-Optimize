![Logo Cloudflare] (https://www.cloudflare.com/img/logo-cloudflare-dark.svg)

[Cloudflare](https://cloudflare.com) là một dịch vụ DNS trung gian nâng cao, có nghĩa là ngoài nhiệm vụ phân giải IP để trình duyệt truy cập vào website, Cloudflare còn nhiều tính năng vui vẻ khác, nổi bật là ẩn địa chỉ IP gốc của máy chủ, tối ưu tốc độ website, WAF (tường lửa),… Tuy nhiên có nhiều tính năng mà đa số anh em ít khi quan tâm tới nên mình chia sẻ để anh em sử dụng Cloudflare một cách hiệu quả nhất.

## Cấu hình

### Cấu hình DNS

Quan trọng đầu tiên là phần cấu hình DNS để trỏ về hosting/server. Chỉ cần 2 record này thôi là quá đủ để sử dụng và cũng dễ thay đổi sau này, đó là:

Type | Name | Value
-----|------|------
A|@|Địa chỉ IP của hosting hoặc VPS
CNAME|www|@

![Cấu hình DNS Cloudflare]
(http://i.imgur.com/vt8qDkl.png)

Vậy là xong, sau này nếu cần thêm sub-domain cùng chung IP như trên thì chỉ việc tạo 1 CNAME, tên là tên của sub-domain, value thì cũng là @. Tại sao lại @ mà lại không dùng record A để trỏ cho tiện? Đơn giản là vì sau này đổi hosting hoặc IP, bạn chỉ việc cấu hình lại record A @ thôi, không cần thay đổi toàn bộ các record khác dễ gây nhầm lẫn.

Bật đám mây màu cam sẽ kích hoạt Cloudflare, nếu có đứt cáp diện rộng thì chuyển nó thành màu xám là được. Lúc này Cloudflare chỉ có vai trò phân giải IP và người dùng sẽ truy cập trực tiếp vào máy chủ của bạn.

Phần DNSSEC nếu có thể thì bạn hãy bật lên luôn nhé. Cloudflare sẽ hiển thị một chuỗi cấu hình (thực ra là 1 record) để bạn setup nên nếu không rành thì bạn có thể gửi cho nhà cung cấp TÊN MIỀN, họ sẽ cài đặt cho bạn.

### Kích hoạt nén resource

Dùng Cloudflare đa số là để tối ưu website dành cho những website lượng truy cập lớn, hoặc những website có băng thông hạn chế và nhiều lí do khác. Do vậy bước thứ 2 chúng ta sẽ chuyển sang tab Speed.

Check tất cả các mục JS, CSS, HTML ở mục Minify. Dùng để nén tất cả dữ liệu của website lại (minify) nên nếu bạn nào dùng WordPress thì không nên cài plugin nào có tính năng này nữa vì như vậy thì hơi phí tài nguyên. Cloudflare đã làm hết cho chúng ta rồi.

**Tính năng Railguns** mình thấy đa số hosting có memcache sẽ hỗ trợ, VPS cũng vậy. Nếu bạn thấy tính năng này có thể bật thì bật lên nhé.

**Tính năng Rocket Loader** nói đơn giản là load JS chỉ khi load xong tất cả các nội dung khác trên trang. Do vậy khi bật lên có thể bạn sẽ thấy slide hình hiện lên chậm hơn, một số plugin hoạt động không tốt. Vì vậy bạn nên check console của trình duyệt, dạo một vòng website xem có lỗi gì xảy ra không nhé. Mình thì mình hay tắt đi.

**Tính năng Accelerated Mobile Links (AMP)** vừa được bổ sung gần đây giúp cho website load nhanh hơn, ít tốn băng thông người đọc hơn khi tìm kiếm trên Google cũng khá hay. Tuy nhiên mình nghĩ bạn nên sử dụng các plugin có chức năng tương tự để có thể tùy chỉnh nhiều hơn thay vì dùng Cloudflare.

Vậy là xong rồi, 2 thứ quan trọng nhất đã được tùy chỉnh xong. Tiếp theo sẽ có một vài tinh chỉnh khá hay mà bạn có thể làm.

==============================================================================================

### Cấu hình Page Rules

Tính năng này khá hay nhưng ít người dùng đến. Bạn có thể yêu cầu Cloudflare cache những resource **theo ý mình** thay vì theo ý của Cloudflare. Mặc dù như trả lời của support thì Cloudflare đã hỗ trợ cache tất cả các resource mặc định như JS, CSS cả rồi (tất cả có thể xem [tại đây](https://support.cloudflare.com/hc/en-us/articles/200172516-Which-file-extensions-does-CloudFlare-cache-for-static-content-)) nhưng mình thấy cách này vẫn hiệu quả nên vẫn làm theo nếu có thời gian.

Bạn có thể thêm Page Rules cho website để quản lý cache tốt hơn, ví dụ bên dưới là theo cấu trúc WordPress, thử xem sao nhé :)

![Cấu hình Page Rules]
(http://i.imgur.com/kxxobTN.jpg)

### Cấu hình SSL

Hiện tại Google đã có nhiều cập nhật liên quan (và có thể là ưu tiên hơn) cho những website có sử dụng HTTPS. Tuy nhiên, nếu bạn vẫn chưa sẵn sàng để chuyển sang HTTPS, hãy tắt tính năng này đi ở tab Crypto, chuyển SSL thành Off. Việc này giúp Google không index website bạn dưới dạng HTTPS, nếu có tắt Cloudflare đi cũng không ảnh hưởng gì.

## Kiểm tra và sửa lỗi

### Làm thế nào để biết Cloudflare đã hoạt động trên website?

Có rất rất nhiều cách, trong đó, bạn có thể kiểm tra với công cụ [Pingdoms Tools](https://tools.pingdom.com), nếu header của website trả về có các dữ liệu sau đây thì chắc chắn website bạn đã cài đặt Cloudflare thành công. Nếu bạn chưa thấy thay đổi đáng kể về tốc độ thì có thể dùng phép ping như mình nói bên dưới, xem đã thay đổi sang IP của Cloudflare chưa.

![Header Cloudflare trả về]
(http://i.imgur.com/DRat91E.png)

Ngoài ra, một cách cổ điển hơn là ping tới tên miền của bạn, nếu ra một IP lạ hoắc, có thể là 104.xxx.xxx.xxx thì cũng đã hoàn tất rồi nhé.

### Bị lỗi net::ERR_TOO_MANY_REDIRECTS khi bật SSL

Well. Với trường hợp này, khi bạn đang sử dụng WordPress do chưa cài đặt plugin hỗ trợ của Cloudflare nên phát sinh lỗi. Để sửa thì làm theo các bước hướng dẫn sau nhé.

1. Tắt SSL của Cloudflare đi (tiện thì làm, không thì bỏ qua cũng được)
2. Chỉnh sửa tập tin wp-config.php, chèn đoạn code bên dưới vào, bên dưới, ngay sau <?php. Nhớ thay example.com thành tên miền của bạn. Bước này giúp bạn có thể truy cập trang admin của WordPress.

```
define('WP_HOME', 'http://example.com');
define('WP_SITEURL', 'http://example.com');
```

3. Cài đặt plugin [Cloudflare Flexible SSL](https://vi.wordpress.org/plugins/cloudflare-flexible-ssl/) ngay và luôn.
4. Chỉnh sửa tập tin wp-config.php. Từ hai đoạn code ở trên, sửa http:// thành https://
5. Done!

### Bị DDoS thì làm gì?

Đối với Free plan, Cloudflare vẫn hỗ trợ chúng ta chống DDoS nhẹ bằng cách bật chế độ I’m Under Attack ở tab Overview. Khách hàng khi truy cập website sẽ bị dừng khoảng 5 giây để Cloudflare kiểm tra mới có thể truy cập tiếp, tuy nhiên vẫn tốt hơn là không thể truy cập được.

Tuy nhiên, bạn nên kiểm tra vấn đề thật sự đang diễn ra vì đôi khi không phải do DDoS. Mình từng có một website bị gửi request liên tục, dạng tìm kiếm trên WordPress (tốn nhiều tài nguyên, không cache). Phải chặn ở Page Rules thì mới truy cập bình thường.

### Cập nhật CSS, JS nhưng trang không thay đổi gì

Đơn giản là Cloudflare đã cache các request vào các resource này rồi. Bạn chuyển sang tab **Caching**, chọn một trong hai chế độ Purge all cache hoặc Purge invidual files (tất cả, không chừa thứ gì hoặc một số file nhất định mà chúng ta vừa chỉnh sửa)

Trong trường hợp các bạn chỉ đang xây dựng website, còn thay đổi nhiều về nội dung và mã nguồn thì mình khuyên nên tắt đám mây cam đi hoặc bật Development mode ở tab Overview nhé.
