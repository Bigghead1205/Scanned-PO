# Scanned-PO
Automation for extracting and logging PO data from emails and arrange PDF files to folders
# 📦 Auto Scan PO – Version 1

Tự động hoá quy trình nhận và xử lý các file Purchase Order (PO) từ email, trích xuất dữ liệu, phân loại file và ghi log phục vụ kiểm soát hải quan nội bộ.

---

## 🛠️ Chức năng chính (v1)

1. Đọc email từ Outlook (folder: `ERP PO`)
2. Lưu file PDF đính kèm PO vào thư mục tạm
3. Trích xuất thông tin PO từ PDF:
   - PO Number, Buyer, Seller
   - VAT, Incoterm, UOM
   - Currency, End-User, Email
4. Ghi thông tin vào log `po_log.csv`
5. Phân loại file PO PDF vào thư mục theo Buyer
6. Tự động xác định PO có cần kiểm tra hàng hóa (Need_CDs)

---

## 📁 Cấu trúc thư mục project

```
99_Project/
├── 0_Run_Files/
│   ├── m00_parallel.py         # Chạy toàn bộ project song song (đa luồng)
│   ├── m01_email_reader.py     # Đọc email và lưu file PO PDF
│   └── m02_pdf_scan.py         # Trích thông tin từ file PO PDF
├── 1. PO_Filtered/             # Chứa các PO đã được phân loại theo Buyer
├── log/
│   ├── po_log.csv              # File log chính
│   └── error.txt               # Ghi lỗi khi xử lý
├── benchmark_parallel.py       # Đo hiệu suất ThreadPoolExecutor
└── README.md                   # Tài liệu mô tả project
```

---

## 🚀 Cách sử dụng

### Bước 1: Xử lý toàn bộ email và PO

```bash
python m00_parallel.py
```

### Bước 2: Kiểm tra file log `po_log.csv` trong thư mục `log/`

---

## ⚙️ Công nghệ & thư viện

- Python 3.10+
- `pywin32`: tương tác với Outlook
- `pdfplumber`: trích xuất văn bản từ PDF
- `concurrent.futures`: xử lý song song
- `pandas`: ghi log CSV

---

## 📌 Logic xác định `Need_CDs`

```python
if VAT == "0%" and Incoterm != "DAP":
    Need_CDs = "Yes"
elif VAT == "0%" and Incoterm == "DAP":
    if UOM == "UNIT":
        Need_CDs = "No"
    else:
        Need_CDs = "Check Manual"
elif "0%" in VAT:
    Need_CDs = "Check Manual"
else:
    Need_CDs = "No"
```

---

## ✅ Yêu cầu hệ thống

- Windows OS + Outlook đã đăng nhập tài khoản nội bộ
- Đã cài đặt thư viện:
  ```bash
  pip install pywin32 pdfplumber pandas
  ```

---

## 🧑‍💻 Tác giả

- Project Owner: Loki Long
- Assistant: ChatGPT (OpenAI)
- Version: `v1 - xử lý & ghi log PO tự động`
