[![Open in Visual Studio Code](https://classroom.github.com/assets/open-in-vscode-2e0aaae1b6195c2367325f4f02e2d04e9abb55f0b24a779b69b11b9e10269abc.svg)](https://classroom.github.com/online_ide?assignment_repo_id=23573976&assignment_repo_type=AssignmentRepo)

# Day 10 Lab: Data Pipeline & Data Observability

**Student Email:** 26ai.trungnv@vinuni.edu.vn
**Name:** Nguyễn Việt Trung

---

## Mo ta

Bai lab xay dung mot ETL Pipeline tu dong gom 4 buoc: Extract du lieu tu file JSON, Validate de loai bo cac record khong hop le (gia <= 0 hoac category rong), Transform de chuan hoa du lieu (Title Case, tinh gia giam 10%, them timestamp), va Load ket qua ra file CSV. Ngoai ra, bai lab con co phan Stress Test so sanh hieu qua cua Agent khi su dung du lieu sach va du lieu "rac", qua do chung minh tam quan trong cua chat luong du lieu doi voi he thong AI.

---

## Cach chay (How to Run)

### Prerequisites

```bash
pip install pandas
```

### Chay ETL Pipeline

```bash
python solution.py
```

### Chay Agent Simulation (Stress Test)

```bash
# Buoc 1: Tao file garbage_data.csv
python generate_garbage.py

# Buoc 2: Chay Agent voi ca Clean data va Garbage data
python agent_simulation.py
```

---

## Cau truc thu muc

```
├── solution.py              # ETL Pipeline script (Extract, Validate, Transform, Load)
├── raw_data.json            # Du lieu dau vao goc
├── processed_data.csv       # Output cua pipeline sau khi xu ly
├── agent_simulation.py      # Script mo phong Agent (RAG-like)
├── generate_garbage.py      # Script tao du lieu "rac" de stress test
├── garbage_data.csv         # Du lieu "rac" dung cho stress test
├── experiment_report.md     # Bao cao ket qua thi nghiem Clean vs Garbage
└── README.md                # File nay
```

---

## Ket qua

**ETL Pipeline:**
- Tong so records doc vao: 5
- Records hop le (processed): 3
- Records bi loai (dropped): 2
  - 1 record gia am (Mystery Box, price = -10)
  - 1 record category rong (Phone, category = "")
- Output: `processed_data.csv` voi cac cot `discounted_price` (giam 10%) va `processed_at` (timestamp)

**Stress Test:**
- Clean data → Agent tra loi chinh xac: *"Laptop at $1200"* (Accuracy: 9/10)
- Garbage data → Agent tra loi sai: *"Nuclear Reactor at $999,999"* do outlier cuc doan (Accuracy: 1/10)
- Ket luan: Chat luong du lieu anh huong truc tiep den do chinh xac cua Agent.
