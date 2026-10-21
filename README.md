# 🎮 EOMM: An Engagement Optimized Matchmaking Framework
> **Tài liệu nghiên cứu khoa học gốc & Phân tích giải thuật ghép trận của Electronic Arts (EA) & Đại học UCLA**  
> *Kênh nghiên cứu: [@JunWooAOV](https://www.tiktok.com/@junwooaov) — Esports Lab*

---

## 📌 1. Nguồn Gốc Bài Báo Khoa Học (Paper Source)
* **Tên bài báo:** *EOMM: An Engagement Optimized Matchmaking Framework*
* **Tác giả:** 
  - Zhengxing Chen, Magy Seif El-Nasr (*Northeastern University*)
  - Su Xue, John Kolen, Navid Aghdaie, Kazi A. Zaman (*Electronic Arts Inc. - EA Research*)
  - Yizhou Sun (*University of California, Los Angeles - UCLA*)
* **Xuất bản:** *Proceedings of the 26th International Conference on World Wide Web (ACM WWW '17)*
* **File PDF gốc:** [`EOMM_Engagement_Optimized_Matchmaking_Framework.pdf`](./EOMM_Engagement_Optimized_Matchmaking_Framework.pdf) (Kèm trong repository này).
* **Link arXiv:** [arXiv:1702.06820](https://arxiv.org/abs/1702.06820)

---

## 🧠 2. Trả Lời Trực Diện Góc Nhìn Data Science (Case: @wanna sleep)

### Q1: "AI quét toàn server thấy ai thua 1-2 trận cũng tắt máy vài tiếng thì coi là Short-term leave chứ đâu phải Churn?"
👉 **Câu trả lời từ chính bài báo (Trang 3, Mục 3.1):**
> *"We use churn risk as a concrete metric of disengagement. The term 'churn' is used by convention, which actually represents a status of a player disengaging with the game within a specified subsequent time frame, not necessarily a permanent churn."*

**Nghĩa là:** Thuật toán EOMM **KHÔNG** đợi bạn xóa game 30 ngày mới tính là Churn. Nó định nghĩa Churn là **trạng thái ngừng chơi ngay trong phiên tiếp theo (Session Churn / Next-Session Dropout)**!

---

### Q2: "Business Rule chi phối mô hình ghép trận như thế nào?"
Trong các tựa game Games-as-a-Service (GaaS) như Liên Quân Mobile hay FIFA:
1. **Business Rule tối thượng:** Bảo vệ **DAU (Daily Active Users)** và **Daily Screen Time**.
2. **Mô hình rủi ro (Risk Transition):** Khi người chơi đang trong trạng thái `[Thua, Thua]`:
   - Nếu trận thứ 3 tiếp tục là `[Thua]` $\rightarrow$ Xác suất người chơi thoát game cả buổi tối hôm đó đạt đỉnh điểm.
   - Nếu chuỗi `[Thua, Thua, Thua]` lặp lại trong nhiều ngày liên tiếp $\rightarrow$ Nguy cơ chuyển hóa thành **Permanent Churn (Xóa game vĩnh viễn)** tăng theo hàm mũ.
3. **Hành vi bắt buộc của EOMM:** Để bảo vệ chỉ số giữ chân, Business Rule bắt buộc hệ thống phải can thiệp (Intervention) bằng cách xếp người chơi vào **Lobby Thắng** ở lần đăng nhập tiếp theo, nhằm kích hoạt trạng thái hưng phấn và kéo dài thời gian online.

---

## 📊 3. Ma Trận Churn Risk Theo Kết Quả 3 Trận Gần Nhất
*(Số liệu thực nghiệm từ cơ sở dữ liệu hàng triệu trận đấu của Electronic Arts trong bài báo)*

| Chuỗi 3 trận gần nhất | Tỷ lệ Rời bỏ (Churn Risk) | Đánh giá của Thuật toán |
| :--- | :---: | :--- |
| **Thắng - Thắng - Thắng** | `Thấp nhất` | Đang thăng hoa $\rightarrow$ Ghép trận thử thách |
| **Thắng - Thua - Thắng** | `Rất thấp` | Trạng thái lý tưởng để giữ chân lâu nhất |
| **Thua - Thắng - Thua** | `Trung bình` | Bắt đầu dao động tâm lý |
| **Thua - Thua - Cố cày tiếp** | `Cực cao` | Bị gán nhãn: *High-Tolerance Player (Cục tạ gánh team)* |
| **Thua - Thua - Tắt App 2-4h** | `Dị biệt (Outlier)` | Kích hoạt cờ *Session Churn Warning* $\rightarrow$ Lần sau nhét Lobby Thắng |

---

## 💻 4. Code Mô Phỏng Thuật Toán EOMM (Python Simulation)
Trong thư mục này có sẵn file mã nguồn Python:
👉 [`simulate_eomm_vs_sbmm.py`](./simulate_eomm_vs_sbmm.py)

Chạy thử nghiệm mô phỏng so sánh giữa **SBMM (Ghép theo trình độ kỹ năng)** và **EOMM (Ghép tối ưu giữ chân)**:
```bash
python simulate_eomm_vs_sbmm.py
```

---

## 🚀 5. Hướng Dẫn Sử Dụng Cho Game Thủ
1. **Quy tắc 2 trận thua:** Thua 2 trận rank liên tiếp $\rightarrow$ Đóng app hoàn toàn tối thiểu 2 - 4 tiếng.
2. **Không cố lấy MVP Bại:** Khi trận đã nát do AFK/phá game, đánh cầm chừng để hạ điểm đánh giá gánh đội ngầm.
3. **Reset nhịp tìm trận:** Quá 35s - 40s chưa thấy trận $\rightarrow$ Bấm HỦY và tìm lại để tránh bị ghép lệch bậc.
4. **Đi đôi/Đi ba:** Khóa cứng trục Rừng + Mid/Top để triệt tiêu biến số "xếp tạ" của hệ thống.

---
*© 2026 JunWooAOV. Bản quyền tài liệu thuộc về nhóm tác giả UCLA & Electronic Arts.*
