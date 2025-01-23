# OWASP ZAP
- Tính năng:
  - Active Scan
  - Passive Scan
  - Spidering and Crawling
  - Brute force scanner
  - Fuzzer
  - Session Management
  - Port scanner
  - Scan websocket
- Ưu
  - Dễ sử dụng
  - Nhiều tính năng
  - Cộng đồng lớn
  - Có API/CLI
- Nhược
  - Cần cấu hình để có kq tốt
  - Nhiều cấu hình, phức tạp
- License
  - Apache 2.0
- Report format
  - HTML (high-level-report, modern, risk-confidence-html, traditional-html, traditional-html-plus)
  - JSON (traditional-json, traditional-json-plus, sarif-json)
  - MD (traditional-md)
  - PDF (traditional-pdf)
  - XML (traditional-xml, traditional-xml-plus)
  - [Templates](https://www.zaproxy.org/docs/desktop/addons/report-generation/templates/)
- Cách tích hợp
  - Dùng image Docker + [Automation framework](https://www.zaproxy.org/docs/automate/automation-framework/) để config
  - [Config template](config.yaml)
```yml
jobs:
  zap_scan:
    runs-on: ubuntu-latest
    name: Scan with docker and automation framework
    steps:
    - name: Checkout 
      uses: actions/checkout@v4
    - name: ZAP Scan
      run: |
        docker run -u zap \
          -v "${{ github.workspace }}:/zap/wrk:rw" \
          -t ghcr.io/zaproxy/zaproxy:stable \
          zap.sh -cmd -autorun /zap/wrk/config.yaml
    - name: Upload ZAP Report
      if: always()
      uses: actions/upload-artifact@v3
      with:
        name: zap-report
        path: |
          zap.out
          *.log
          *.xml
          *.html
```
  - Dùng API ([Documentation](https://www.zaproxy.org/docs/api/#overview))
  - Dùng các action github
    - https://github.com/zaproxy/action-baseline
    - https://github.com/zaproxy/action-full-scan
    - https://github.com/zaproxy/action-api-scan
    - https://github.com/zaproxy/action-af
  - Ví dụ
```yml
jobs:
  zap_scan:
    runs-on: ubuntu-latest
    name: Scan with action
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