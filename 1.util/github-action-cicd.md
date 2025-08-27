1. ##### CI/CD là gì ?



&nbsp;	+ CI/CD với GitHub Actions là một quy trình tự động hóa toàn bộ vòng đời phát triển của một website, từ lúc viết code cho đến khi đưa sản phẩm đến tay người dùng. Hãy tưởng tượng nó như một dây chuyền sản xuất thông minh và hiệu quả cho trang web của bạn.





Trong đó:



###### CI (Continuous Integration - Tích hợp liên tục):

&nbsp;	+  Đây là giai đoạn tự động hợp nhất code từ nhiều lập trình viên vào một kho chứa chung. Mỗi khi có sự thay đổi, hệ thống sẽ tự động xây dựng (build) và kiểm thử (test) để đảm bảo code mới không gây ra lỗi và vẫn hoạt động tốt với phần còn lại của website.



###### CD (Continuous Delivery/Deployment - Giao hàng/Triển khai liên tục): 

&nbsp;	+ Sau khi giai đoạn CI thành công, CD sẽ tự động hóa việc đưa phiên bản mới của website đến môi trường kiểm thử hoặc trực tiếp đến người dùng cuối.



###### &nbsp;	Continuous Delivery (Giao hàng liên tục): 

&nbsp;		+ Tự động triển khai đến một môi trường chờ (staging) để kiểm tra lần cuối trước khi quyết định "nhấn nút" đưa lên phiên bản chính thức.



###### &nbsp;	Continuous Deployment (Triển khai liên tục): 

&nbsp;		+ Tự động hoàn toàn, sau khi vượt qua tất cả các bài kiểm tra, code sẽ được triển khai trực tiếp lên website mà không cần sự can thiệp của con người.







##### 2\. Github Actions



&nbsp;	+  là công cụ của GitHub cho phép bạn thiết lập và tùy chỉnh các quy trình CI/CD này ngay trên nền tảng quản lý code của mình. Bạn có thể định nghĩa các "hành động" (actions) để tự động hóa các công việc như: xây dựng website, chạy các bài kiểm thử, đóng gói tài nguyên và triển khai lên máy chủ.





	**Vai trò của nó đối với website**



	+ Tăng tốc độ phát triển và ra mắt tính năng mới 



&nbsp;	+ Nâng cao chất lượng và giảm thiểu lỗi



	+ Giảm rủi ro và thời gian chết (downtime)



&nbsp;	+ Tăng hiệu suất làm việc cho đội ngũ





##### 3\. CI/CD web với github action





###### &nbsp;	1. Phải có project trên repo



###### &nbsp;	2. Vào phần actions và chọn môi trường phù hợp, VD: web next > Nodejs 



###### &nbsp;	3. Configure -> commit change vào dự án 

&nbsp;	

&nbsp;		+ Sau đó sẽ tạo ra 1 folder .github/workflows 



###### &nbsp;	4. Sẽ có 1 file xuất hiện 





###### Mục tiêu của file này



Chạy CI (Continuous Integration) cho dự án Node.js mỗi khi bạn push code hoặc tạo pull request lên branch main.



###### Tóm tắt quy trình chạy



Khi push hoặc PR vào main, GitHub Actions khởi động.



Tải code về (checkout).



Cài Node.js với từng version trong \[18, 20, 22].



Cache npm để lần sau nhanh hơn.



Cài dependencies (npm ci).



Build dự án nếu có lệnh build.



Chạy test.



Nếu mọi thứ thành công → workflow được đánh dấu ✅.





###### &nbsp;	5. Giải thích qua về file 



\# Workflow này dùng để cài Node.js, cache dependencies, build và test dự án

\# Link docs: https://docs.github.com/en/actions/automating-builds-and-tests/building-and-testing-nodejs



name: Node.js CI  # Tên workflow, sẽ hiện trong tab Actions



on:

&nbsp; push:

&nbsp;   branches: \[ "main" ]  # Chạy khi push code vào nhánh main

&nbsp; pull\_request:

&nbsp;   branches: \[ "main" ]  # Chạy khi tạo PR vào nhánh main



jobs:

&nbsp; build:  # Định nghĩa 1 job tên "build"



&nbsp;   runs-on: ubuntu-latest  # Chạy trên máy ảo Ubuntu mới nhất



&nbsp;   strategy:

&nbsp;     matrix:               # Chạy nhiều lần với các biến môi trường khác nhau

&nbsp;       node-version: \[18.x, 20.x, 22.x]  # Danh sách version Node sẽ test

&nbsp;       # Xem thêm về các version Node: https://nodejs.org/en/about/releases/



&nbsp;   steps:

&nbsp;   - uses: actions/checkout@v4  # Lấy code từ repo về máy ảo



&nbsp;   - name: Use Node.js ${{ matrix.node-version }}  # Đặt tên step, hiển thị version đang dùng

&nbsp;     uses: actions/setup-node@v4                   # Action để cài Node.js

&nbsp;     with:

&nbsp;       node-version: ${{ matrix.node-version }}    # Lấy version từ matrix ở trên

&nbsp;       cache: 'npm'                                # Cache thư mục npm để cài nhanh hơn



&nbsp;   - run: npm ci                 # Cài dependencies từ package-lock.json (nhanh và chuẩn xác hơn npm install)

&nbsp;   - run: npm run build --if-present  # Build dự án nếu có script "build" trong package.json

&nbsp;   - run: npm test               # Chạy test từ script "test" trong package.json







###### &nbsp;	6. Tác dụng của việc làm này



&nbsp;		+ Đảm bảo dự án tương thích với nhiều phiên bản node

&nbsp;		+ Bắt lỗi sớm trước khi deploy

&nbsp;		+ Tự động hóa kiểm tra chất lượng code 

&nbsp;		+ Hỗ trợ dự án mã nguồn mở 



Mục đích là để dự án của bạn được test “đa môi trường” → tránh bug do khác biệt version Node, tăng độ tin cậy trước khi đưa vào production.



&nbsp;		+ Việc sử dụng github actions chính là đang thực hiện bước CI



&nbsp;		+ Ta hoàn toàn có thể custom những gì cần test qua phần - run: và package.json tương ứng 




4. CD với Github Pages

	1. CI/CD là gì khi dùng với GitHub Pages?

CI (Continuous Integration): Mỗi khi bạn push code mới hoặc merge pull request, GitHub sẽ tự chạy quy trình build (nếu cần).

CD (Continuous Deployment): Sau khi build xong, GitHub Pages tự động deploy bản mới lên trang web, không cần bạn làm thủ công.



	2. Quy trình hoạt động với github page 

Bạn viết code → Push lên branch main
       ↓
GitHub Actions trigger (CI)
       ↓
Chạy lệnh build (npm run build)
       ↓
Tạo folder build tĩnh
       ↓
Push folder build sang branch gh-pages (CD)
       ↓
GitHub Pages lấy gh-pages để hiển thị web


	3. Ưu điểm khi dùng cd github page 

| Ưu điểm                          | Lợi ích                                                    |
| -------------------------------- | ---------------------------------------------------------- |
| **Tự động**                      | Không cần build và upload thủ công.                        |
| **Nhanh**                        | Chỉ cần push code là web cập nhật sau vài chục giây.       |
| **Ít lỗi**                       | Build trên môi trường GitHub, không phụ thuộc máy cá nhân. |
| **Có thể test trước khi deploy** | Thêm bước test/lint trước khi build.                       |


	4. Cách sử dụng

		1. vào setting > pages > github action > configure > commit changes




	

	





