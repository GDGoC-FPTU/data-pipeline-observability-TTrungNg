# Experiment Report: Data Quality Impact on AI Agent

**Student ID:** AI20K-00244
**Name:** Nguyễn Việt Trung
**Date:** 2026-04-15

---

## 1. Ket qua thi nghiem

Chay `agent_simulation.py` voi 2 bo du lieu va ghi lai ket qua:

| Scenario | Agent Response | Accuracy (1-10) | Notes |
|----------|----------------|-----------------|-------|
| Clean Data (`processed_data.csv`) | "Based on my data, the best choice is Laptop at $1200." | 9 | Ket qua chinh xac: Laptop la san pham electronics co gia cao nhat ($1200) trong du lieu sach da qua ETL pipeline. |
| Garbage Data (`garbage_data.csv`) | "Based on my data, the best choice is Nuclear Reactor at $999999." | 1 | Ket qua sai hoan toan: Agent chon "Nuclear Reactor" vi gia $999,999 la outlier cuc doan, khong phai lua chon thuc te. |

---

## 2. Phan tich & nhan xet

### Tai sao Agent tra loi sai khi dung Garbage Data?

Du lieu "rac" trong `garbage_data.csv` chua nhieu van de nghiem trong anh huong truc tiep den ket qua cua Agent:

1. **Extreme Outlier (Gia tri bat thuong):** Ban ghi "Nuclear Reactor" co gia $999,999 la mot outlier cuc doan khong co y nghia thuc te. Agent su dung logic `idxmax()` de tim san pham co gia cao nhat, nen no lap tuc chon gia tri nay, dan den mot khuyen nghi phi thuc te va sai lech hoan toan.

2. **Duplicate ID:** Ban ghi co `id = 1` xuat hien 2 lan (Laptop va Banana), gay nham lan trong viec dinh danh san pham. Neu Agent can truy vet ban ghi goc, no se khong biet chon ban ghi nao la dung.

3. **Wrong Data Type (Sai kieu du lieu):** Ban ghi "Broken Chair" co truong `price = "ten dollars"` la chuoi van ban thay vi so. Neu Agent thuc hien tinh toan tren cot `price`, no se gap loi hoac tao ra ket qua khong du doan truoc duoc.

4. **Null Values (Gia tri null):** Ban ghi "Ghost Item" co `id = None` va `category = None`. Cac gia tri null nay co the khien cac phep loc va so sanh cua Agent hoat dong khong dung, cho phep du lieu khong hop le lam anh huong den ket qua.

Ket hop lai, nhung van de nay "dau doc" co so kien thuc cua Agent, khien no dua ra khuyen nghi co ve hop ly ve mat co phap nhung hoan toan vo nghia trong thuc te.

---

## 3. Ket luan

**Quality Data > Quality Prompt?** Dong y.

Du co mot Agent duoc thiet ke tot va cau hoi (prompt) chinh xac, ket qua van hoan toan sai neu du lieu dau vao chua nhieu loi. Trong thi nghiem nay, ca hai lan chay deu dung cung mot cau query "What is the best electronic product?", nhung ket qua trai nguoc nhau hoan toan. Dieu nay chung minh rang: **chat luong du lieu la nen tang** quyet dinh do chinh xac cua bat ky he thong AI nao. Mot ETL pipeline voi buoc validate va transform nghiem ngat (nhu trong `solution.py`) la dieu kien tien quyet truoc khi cho Agent tiep can du lieu.
