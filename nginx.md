##### 1\. nginx là gì ?



 	+ Nginx là một phần mềm web server có khả năng phục vụ website và điều phối các request đến những dịch vụ khác như backend, API, v.v.



 	+ Nginx là web server mạnh mẽ, giúp phục vụ nội dung web, định tuyến request và quản lý hệ thống web một cách hiệu quả, bảo mật và nhanh chóng.

 



##### 2\. Tác dụng của nginx



######  \*\*Web server\*\*

\\ Trả file HTML, ảnh, JS cho trình duyệt theo request của người dùng. đây chính là tính năng cơ bản nhất của 1 web server







###### \*\* Reverse Proxy\*\*

( Proxy là một máy chủ trung gian giúp chuyển tiếp các yêu cầu từ client đến server và ngược lại.)



\\ Đóng vai trò là 1 máy chủ trung gian giữa client và server

\\ Bảo mật backend

\\ Chặn request xấu, ddos, limit request, ip xấu

\\ Ẩn cấu trúc nội bộ backend

\\ Chỉ expose ra 1 domain duy nhất như ở bên file docker đã giải thích nên sẽ không thể biết backend có gì





#####  \*\* Mở rộng so sánh proxy và reverse proxy



| Tiêu chí                        | \*\*Proxy (Forward Proxy)\*\*                   | \*\*Reverse Proxy\*\*                      |

| ------------------------------- | ------------------------------------------- | -------------------------------------- |

| \*\*Phục vụ\*\*                     | Client (người dùng cuối)                    | Server (hệ thống backend)              |

| \*\*Vị trí\*\*                      | Giữa \*\*client\*\* và \*\*internet/server\*\*      | Giữa \*\*client\*\* và \*\*server nội bộ\*\*   |

| \*\*Mục đích chính\*\*              | Giúp client truy cập ra ngoài               | Điều phối truy cập từ ngoài vào server |

| \*\*Client có biết proxy không?\*\* | Có                                          | Không                                  |

| \*\*Bảo mật\*\*                     | Ẩn IP client, lọc nội dung                  | Ẩn IP server, chống tấn công trực tiếp |

| \*\*Tính năng thường gặp\*\*        | Vượt tường lửa, ẩn danh, kiểm soát truy cập | Load balancing, cache, SSL termination |

| \*\*Ví dụ thực tế\*\*               | Dùng proxy Mỹ để vào website bị chặn        | Nginx đứng trước backend Express.js    |





###### 

######  \*\*Load balancing\*\*

\\ Phân chia yêu cầu đều cho nhiều backend (giúp web không bị nghẽn)

\\ Định tuyến thông minh server dựa theo vị trí địa lý

\\ Chia request đều ra cho các server để đem lại trải nghiệm tốt nhất cho người dùng





######  \*\*Caching\*\*

\\ Lưu tạm dữ liệu để tăng tốc độ phản hồi

\\ cache dữ liệu được yêu cầu nhiều

\\ Giả sử bạn request 1 yêu  cầu, tiếp theo người dùng tương tự cũng request. nginx sẽ cache kết quả từ request và trả về ngay lập tức chứ không cần phải gọi tới backend nữa





###### \*\*Security\*\*

\\ Quản lý HTTPS (chứng chỉ bảo mật)

\\ Thiết lập SSL Certificated để có https

\\ Giới hạn truy cập theo ip, domain

\\ thiết lập https

\\ encripted communication





\*\*Compression\*\* (Nén dữ liệu)

\\ Nén file trước khi gửi đến trình duyệt từ đó làm cho trải nghiệm người dùng mượt mà và nhanh chóng hơn





##### 

##### 3\. Ví dụ dễ hiểu



Giả sử bạn xây dựng một website xem phim, người dùng gõ:





 > Nếu bạn dùng NGINX:



Người dùng truy cập Nginx (qua domain)



Nginx:



Thấy người dùng cần trang web → trả về file HTML từ thư mục frontend



Thấy người dùng gọi API /api/movies → chuyển sang backend (Node.js) để lấy dữ liệu



Có HTTPS → Nginx xử lý luôn, backend không cần quan tâm



📌 Tóm lại: Người dùng chỉ tiếp xúc với Nginx, còn backend, frontend, database bên trong đều ẩn đi.





##### 4\. Cài đặt nginx cho window



 	 https://nginx.org/en/download.html



 	> Nhớ cài bản có .zip và window cho window để tránh lỗi





 	2. Sử dụng



 	cmd



 	> start nginx

 	> nginx.exe -s stop



 	powershell



 	> .\\nginx.exe

 	> .\\nginx.exe -s stop





##### 

##### 5\. Giải thích qua về file nginx.conf



\# user nobody;                          # user chạy Nginx (bỏ vì đa số để mặc định)

worker\_processes  1;                   # số tiến trình worker, nên đặt bằng số core CPU



\# pid logs/nginx.pid;                  # file lưu PID của Nginx (bỏ nếu không cần theo dõi)



events {

    worker\_connections  1024;         # số kết nối đồng thời tối đa mỗi worker có thể xử lý

}



http {

    include       mime.types;         # xác định MIME type cho các định dạng file

 				  # không có thì không thể áp dụng được file vào

    default\_type  application/octet-stream;  # MIME mặc định nếu không xác định được loại file



    sendfile        on;               # bật cơ chế gửi file hiệu quả, giảm tải CPU

    keepalive\_timeout  65;            # thời gian duy trì kết nối keep-alive với client



    server {

        listen       80;              # lắng nghe cổng 80 (HTTP)

        server name  localhost;       # tên domain/server name



        location / {

            root   html;              # thư mục gốc chứa file tĩnh

            index  index.html index.htm;  # ưu tiên các file index khi truy cập thư mục

        }



        error\_page   500 502 503 504  /50x.html;  # xử lý lỗi server nội bộ bằng trang 50x.html

        location = /50x.html {

            root   html;              # đường dẫn chứa trang lỗi

        }



        # Các cấu hình xử lý PHP hoặc từ chối .htaccess đã được comment sẵn

    }



    # Các server khác (cổng khác, SSL) đã được comment sẵn để tham khảo, không cần giữ nếu không dùng

}







##### 6\. Một số thuộc tính location 



###### &nbsp;	1. Cấu trúc cơ bản 



location /đường-con/ {

&nbsp;   # Các thuộc tính nằm ở đây

}





| Thuộc tính                | Chức năng chính                                               |

| ------------------------- | ------------------------------------------------------------- |

| `root`                    | Chỉ định thư mục gốc để tìm file                              |

| `alias`                   | Giống root nhưng xử lý URL khác biệt                          |

| `index`                   | Danh sách file ưu tiên hiển thị (thường là `index.html`)      |

| `try\_files`               | Cố gắng tìm file tĩnh, nếu không có thì fallback (ví dụ: SPA) |

| `proxy\_pass`              | Chuyển tiếp request đến server khác (reverse proxy)           |

| `rewrite`                 | Viết lại URL                                                  |

| `return`                  | Trả về mã trạng thái hoặc redirect                            |

| `add\_header`              | Thêm header tùy ý vào response                                |

| `expires`                 | Cấu hình cache header cho file tĩnh                           |

| `deny`, `allow`           | Giới hạn quyền truy cập                                       |

| `limit\_req`, `limit\_conn` | Giới hạn tốc độ hoặc số kết nối                               |

| `gzip`                    | Nén response (nếu có module gzip)                             |





###### &nbsp;	2. Giải thích chi tiết + ví dụ 





###### &nbsp;		1. root 



location /images/ {

&nbsp;   root /var/www/assets;

}



→ /images/logo.png sẽ tìm ở: /var/www/assets/images/logo.png





###### &nbsp;		2. alias



location /images/ {

&nbsp;   alias /var/www/assets/;

}



→ /images/logo.png sẽ tìm ở: /var/www/assets/logo.png (khác root!)



###### &nbsp;		3. index



location / {

&nbsp;   index index.html index.htm;

}



→ Khi truy cập /, Nginx sẽ tìm các file theo thứ tự



		

###### &nbsp;		4. try\_files



location / {

&nbsp;   root /var/www/myapp;

&nbsp;   try\_files $uri /index.html;

}



→ Nếu không tìm thấy file tĩnh, sẽ trả về index.html (dùng cho SPA)



###### &nbsp;		5. proxy\_pass





location /api/ {

&nbsp;   proxy\_pass http://localhost:3000;

}



→ Proxy tất cả /api/\* đến Express backend





###### &nbsp;		6. rewrite



location /blog {

&nbsp;   rewrite ^/blog$ /blog/ permanent;

}



→ Tự động redirect /blog → /blog/





###### &nbsp;		7. return



location /old-page {

&nbsp;   return 301 /new-page;

}



→ Redirect vĩnh viễn /old-page → /new-page





###### &nbsp;		8. add\_header



location / {

&nbsp;   add\_header X-Powered-By "Nginx" always;

}



→ Thêm custom header vào response



###### &nbsp;		9. expires – cache file tĩnh



location ~\* \\.(js|css|png|jpg|jpeg|gif)$ {

&nbsp;   expires 30d;

&nbsp;   add\_header Cache-Control "public";

}

→ Cache các file tĩnh 30 ngày







###### &nbsp;		10. deny, allow



location /admin {

&nbsp;   allow 192.168.1.0/24;

&nbsp;   deny all;

}



→ Chỉ cho IP nội bộ truy cập /admin





###### &nbsp;		Tổng hợp cụ thể 



location / {

&nbsp;   root /var/www/frontend/build;

&nbsp;   index index.html;

&nbsp;   try\_files $uri /index.html;

}



location /api/ {

&nbsp;   proxy\_pass http://localhost:3000;

&nbsp;   proxy\_set\_header Host $host;

&nbsp;   proxy\_set\_header X-Real-IP $remote\_addr;

&nbsp;   proxy\_http\_version 1.1;

&nbsp;   proxy\_set\_header Upgrade $http\_upgrade;

&nbsp;   proxy\_set\_header Connection 'upgrade';

}



location ~\* \\.(js|css|png|jpg|jpeg|gif)$ {

&nbsp;   expires 30d;

&nbsp;   add\_header Cache-Control "public";

}







##### 7\. Một số lưu ý khi sử dụng nginx 



###### ❌ Những giới hạn khi dùng bản Nginx cho Windows



| Vấn đề                          | Mô tả                                                                 |

| ------------------------------- | --------------------------------------------------------------------- |

| Hiệu suất thấp hơn Linux        | Nginx chạy tốt nhất trên hệ thống Unix                                |

| Không hỗ trợ module động đầy đủ | Không thể dùng module bên thứ ba như trên Linux (VD: `ngx\_pagespeed`) |

| Không phù hợp cho production    | Dùng tốt để học, test, dev local nhưng không nên deploy thật          |





###### ✅ nginx hoạt động tốt nhất khi là 1 mảnh ghép với những ứng dụng khác 



&nbsp;	+ Kết hợp nginx + docker compose trong các dự án fullstack với react + be



&nbsp;	+ Kết hợp Nginx + SSL (HTTPS)



&nbsp;	+ Kết hợp Nginx + Cloud (AWS, GCP, Vercel Edge, v.v.)



| Kết hợp với gì    | Mục đích chính                  | Sản phẩm cụ thể |

| ----------------- | ------------------------------- | --------------- |

| Node.js / Laravel | Reverse proxy, bảo mật backend  | Web API         |

| React / Vue       | Serve web tĩnh + proxy          | Web app         |

| Docker            | Gộp toàn bộ hệ thống, dễ deploy | DevOps, CI/CD   |

| SSL / HTTPS       | Bảo mật, production             | Web thương mại  |

| Load Balancer     | Cân bằng tải giữa nhiều backend | Hệ thống lớn    |





\*\* Giả cấu trúc 1 app có sử dụng nginx ]



my-app/

├── backend/        ← Express app (chạy port 3000)

│   └── index.js

├── frontend/       ← React app

│   └── build/      ← Kết quả của `npm run build`

├── nginx.conf      ← Cấu hình Nginx





##### 8\. SSL Certificated với nginx 



###### 

###### &nbsp;1. SSL Certificate là gì?



SSL Certificate là chứng chỉ số mã hóa dữ liệu giữa trình duyệt và server.



Khi bạn truy cập một trang có https://, nó đang dùng SSL.



Giúp chống nghe lén, giả mạo, và nâng cao độ tin cậy.



Ví dụ: Google, Facebook, ngân hàng đều bắt buộc dùng HTTPS.





###### 2\. Nginx dùng SSL Certificate như thế nào?



server {

&nbsp;   listen 443 ssl;

&nbsp;   server\_name example.com;



&nbsp;   ssl\_certificate /etc/nginx/ssl/example.com.crt;

&nbsp;   ssl\_certificate\_key /etc/nginx/ssl/example.com.key;



&nbsp;   location / {

&nbsp;       root /var/www/html;

&nbsp;       index index.html;

&nbsp;   }

}





| Dòng lệnh                  | Ý nghĩa                                              |

| -------------------------- | ---------------------------------------------------- |

| `listen 443 ssl;`          | Lắng nghe cổng HTTPS (443)                           |

| `server\_name example.com;` | Tên miền của bạn                                     |

| `ssl\_certificate`          | File chứa chứng chỉ public (đuôi `.crt` hoặc `.pem`) |

| `ssl\_certificate\_key`      | File chứa private key tương ứng                      |







###### 3\. Lấy SSL Certificate ở đâu?



✅ Miễn phí: Let’s Encrypt

Phổ biến nhất, hoàn toàn miễn phí.



Dùng certbot để cài đặt tự động.





###### 4\. Redirect từ HTTP sang HTTPS:



server {

&nbsp;   listen 80;

&nbsp;   server\_name example.com;



&nbsp;   return 301 https://$host$request\_uri;

}















&nbsp;	

