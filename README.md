# DAST là gì
Dynamic Application Security Testing (DAST) chạy pentest tự động để tìm lỗ hổng trên webapp/API đang chạy.
DAST tự động mô phỏng các cuộc tấn công thật như XSS, SQLi, CSRF để tìm ra các lỗ hổng cũng như lỗi cấu hình của hệ thống.
# 1.  OWASP ZAP
- Tính năng:
  - Active Scan
  - Passive Scan
  - Spidering and Crawling
  - Fuzzer
  - Session Management
  - Reporting
- License
  - Apache 2.0
- Cách tích hợp
  - Thêm các repo github của zap vào trong CI/CD pipeline
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
          rules_file_name: '.zap/rules.tsv'
          cmd_options: '-a'
```
# 2. Nikto
- Tính năng:
  - Comprehensive Server Scanning
  - Multiple Hosts - Multiple Port Scanning
  - Scan through proxy
  - Bypass ratelimiting
  - Addon plugins
  - Reporting
- License
  - GPL-2.0 
- Cách tích hợp
  - Thêm repo github của nikto vào CI/CD pipeline
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
# 3. Arachni
- Tính năng:
  - Scan
  - Crawler
  - Reporting
  - Distributed scanning
- License
  - Arachni Public Source License Version 1.0
- Cách tích hợp

```yml
jobs:
  scan:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Set up Arachni
        run: sudo apt-get install -y arachni

      - name: Run Arachni Scan
        run: |
          arachni http://example.com --report-save-path=arachni_report.afr
          arachni_reporter arachni_report.afr --reporter=html:output=arachni_report.html
        continue-on-error: true

      - name: Upload Arachni Report
        uses: actions/upload-artifact@v4
        with:
          name: arachni-report
          path: arachni_report.html

```
# 4. OpenVAS
- Tính năng:
  - Scan
  - Updated feed
  - Reporting
  - API
- License
  - GPL-2.0 và AGPL-3.0
- Cách tích hợp
  - Dựng server OpenVAS
  - Từ trong CI/CD pipeline, gọi api để quét 

# 5. Gitlab
- Sử dụng Gitlab DAST, base là owasp zap.
- Tích hợp vào CI/CD pipeline
