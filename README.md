# Tìm hiểu về DAST
DAST là phương pháp testing black-box để tìm lỗ hổng trên webabb/API đang chạy.
DAST tìm lỗ hổng bằng cách  phân tích app đang chạy từ frontend và mô phỏng tấn công giống như hacker làm.
### Quy trình hoạt động
Quy trình hoạt động của DAST thường gồm các bước:
- Quét để xác định các form, API, URL và gửi request
- Phân tích response để tìm các bất thường 
- Giả lập tấn công vào các thành phần tìm được ở b2
- report
### Một số lỗ hổng phổ biến mà DAST có thể phát hiện
- Authentication and authorization flaws 
- Server misconfiguration
- Code injection
- SQL injection
- Cross-site scripting
- Cross-site request forgery
- IDOR
### Ưu
- DAST có thể chạy trong ci/cd pipeline, đặt lịch hoặc chạy thủ công.
- Test black-box -> không cần source code -> Có thể test app viết bằng mọi ngôn ngữ 
- Test được trên mọi môi trường
- Giảm false positive so với SAST
- Phát hiện được nhiều lỗ hổng
### Nhược
- Cần ứng dụng đang chạy để quét
- Khó setup, cấu hình 
- Tìm được lỗ hổng nhưng không tìm được đoạn code nào gây ra lỗ hổng đó
- Không test được tất cả các thành phần của app
# Các tool
## 1.  OWASP ZAP
- Tính năng:
  - Active Scan
  - Passive Scan
  - Spidering and Crawling
  - Brute force scanner
  - Fuzzer
  - Session Management
  - Port scanner
  - Scan websocket
  - Reporting
- Ưu
  - Dễ sử dụng
  - Nhiều tính năng
  - Cộng đồng lớn
  - Có API/CLI
- Nhược
  - Cần cấu hình để có kq tốt
- License
  - Apache 2.0
- Cách tích hợp
  - Thêm các action github của zap vào trong CI/CD pipeline
  - https://github.com/zaproxy/action-baseline
  - https://github.com/zaproxy/action-full-scan
  - https://github.com/zaproxy/action-api-scan
  - https://github.com/zaproxy/action-af
  - Ví dụ
```yml
jobs:
  zap_scan:
    runs-on: ubuntu-latest
    name: Scan the webapplication
    steps:
      - name: Checkout
        uses: actions/checkout@v4
      - name: ZAP Scan
        uses: zaproxy/action-full-scan@v0.12.0
        with:
          token: ${{ secrets.GITHUB_TOKEN }}
          docker_name: 'ghcr.io/zaproxy/zaproxy:stable'
          target: 'https://www.example.org/'
          cmd_options: '-a'
```
## 2. Nikto
- Tính năng:
  - Comprehensive Server Scanning
  - Multiple Hosts - Multiple Port Scanning
  - Scan through proxy
  - Bypass ratelimiting
  - Addon plugins
  - Reporting
- Ưu
  - Vul DB lớn
  - Có API/CLI
- Nhược
  - Tạo nhiều request cùng 1 lúc -> giảm hiệu năng server
  - Không quét sâu
- License
  - GPL-2.0 
- Cách tích hợp
  - Thêm action github của nikto vào CI/CD pipeline
  - https://github.com/thereisnotime/Action-nikto
  - Ví dụ
```yml
jobs:
  nikto:
    runs-on: ubuntu-latest
    name: SCAN | nikto DAST
    steps:
      - name: Scan with nikto
        uses: thereisnotime/action-nikto@master
        with:
          url: "https://example.com"
          additional_args: "-ssl"
```
## 3. Arachni
- Tính năng:
  - Scan
  - Crawler
  - Reporting
  - Distributed scanning
- Ưu
  - Có API/CLI và GUI
- Nhược
  - Discontinued
- License
  - Arachni Public Source License Version 1.0
## 4. OpenVAS
- Tính năng:
  - Scan
  - Updated feed
  - Reporting
  - API
  - Network scanning
- Ưu
  - Quét toàn diện
  - Vul DB cập nhật liên tục
  - Report chi tiết
  - Cộng đồng lớn
- Nhược
  - Khó dùng API/CLI
  - Tốn tài nguyên
  - Quét lâu
  - Không focus vào web app mà focus vào Full system vulnerability scanning
- License
  - GPL-2.0 và AGPL-3.0
- Cách tích hợp
  - Dựng server OpenVAS
  - Gọi API để quét

## 5. Gitlab
- Sử dụng Gitlab DAST, base là OWASP ZAP.
- Tích hợp vào CI/CD pipeline
# Kết luận
- OpenVAS không thích hợp vì quét lâu và scope lớn
- Arachni không thích hợp vì Discontinued
- Nikto và ZAP phù hợp, có thể tích hợp vào gitea bằng cách
  - Sử dụng image docker trong CI/CD Pipeline
  - Chỉnh sửa code action để phù hợp với gitea
