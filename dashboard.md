# OPERATING DASHBOARD — CaTối

**Loại mô hình:** B2B (SMB): chủ shop thời trang trả tiền và chính họ dùng; khách mua chỉ thấy Fanpage của shop; Haravan chỉ là kênh bán app · **Cập nhật:** 09/10/2026 · Nguyễn Văn Biển – 2A202602416

**NORTH STAR:** TTFV trung vị (kết nối xong → 50 hội thoại xử lý xong + 1 đơn nháp được xác nhận) — hiện tại **chưa có số** (pilot go-live 19/10) — mục tiêu **≤ 7 ngày**

### Đèn báo sớm (Leading — nhìn hằng ngày/tuần)

| Đèn | Hiện | 🟢 / 🟡 / 🔴 | Nguồn | Báo trước cho |
|---|---|---|---|---|
| TTFV ⭐ (ngày) | chưa có | ≤7 / 8–14 / >14 | [MH] pilot 28 ngày − 14 ngày dùng thật | POC→paid → CAC payback |
| Containment (% hội thoại AI xử lý xong) | chưa có (giả định 70%) | ≥70 / 55,3–69,9 / <55,3 (rev-share 20%: <69,1) | [MH] breakeven GM 60% | Cost/Job → GM ròng |
| Cost/Job thực **p95 theo shop** (chi phí AI) | chưa có (mô hình 1.232₫) | ≤1.355₫ / 1.356–1.560₫ / >1.560₫ | [MH] 40% × 3.900₫ | GM ròng (trước ~1 tháng) |

### Đèn vận hành (Operating — nhìn hằng tuần/tháng)

| Đèn | Hiện | 🟢 / 🟡 / 🔴 | Nguồn | Báo trước cho |
|---|---|---|---|---|
| POC → paid (% shop trả hoá đơn đầu sau pilot) | chưa có | ≥60% / 40–59% / <40% | [BM] ICONIQ GTM 2026 ~50% (KT 09/10/2026) + [MH] 3/5 | CAC payback |
| Hội thoại xử lý xong / shop / tháng (trung vị) | chưa có | ≥1.050 / 400–1.049 / <400 | [MH] ARPU 4.095.000₫ | ARPU → CAC payback |
| Giờ công triển khai / shop | chưa có | ≤3,1h / 3,2–6,2h / >6,2h | [MH] 88h ÷ 14,1 shop/tháng | Năng lực 30 shop, CAC |

### Đèn kết quả (Lagging — nhìn hằng quý)

| Đèn | Hiện | 🟢 / 🟡 / 🔴 | Nguồn |
|---|---|---|---|
| GM ròng (sau rev-share Haravan) | chưa có (mô hình 68,4%) | ≥60% / 53–59,9% / <53% | [MH] sàn Day22 + [BM] ICONIQ State of AI 2026: 53% 2026P (KT 09/10/2026) |
| CAC payback (tháng) | chưa có | ≤3,0 / 3,1–5,9 / >5,9 | [MH] runway 8,9 − 3 tháng; trần [BM] SMB <12 (Bessemer, KT 09/10/2026) |

### 5 luật quyết định (⏹ = luật dừng)

1. ⏹ **NẾU** TTFV >14 ngày **TRÊN** 2/3 shop onboard gần nhất **THÌ** dừng nhận shop mới, founder ngồi với 2 shop đó trong 48h, cắt onboarding còn 1 luồng (giá/size/còn hàng) tới khi có shop đạt ≤7 ngày. **KHÔNG THÌ** không gia hạn pilot miễn phí, không tìm thêm shop pilot để bù số.
2. ⏹ **NẾU** containment <55,3% (<69,1% khi có rev-share 20%) **TRONG** 2 tuần liên tiếp **VÀ** ≥300 hội thoại/tuần từ ≥3 shop **THÌ** đóng băng tính năng mới và dừng nhận shop 1 sprint; gán nhãn 100 ca thất bại, sửa 2 nguyên nhân lớn nhất. **KHÔNG THÌ** không nâng lên Sonnet 5.5 (GM 46,6%), không nới định nghĩa "xử lý xong".
3. **NẾU** Cost/Job p95 >1.560₫ (>1.248₫ khi có rev-share 20%) **TRONG** 1 tháng **VÀ** shop đó ≥300 job xong **THÌ** đặt trần 8 lượt/hội thoại, bỏ ảnh khỏi lịch sử sau lượt 2 cho shop đó, chạy eval Haiku 5.5. **KHÔNG THÌ** không tăng giá đại trà, không chuyển Biến thể B (GM ~29%).
4. ⏹ **NẾU** POC→paid <40% **TRÊN** một đợt pilot ≥5 shop **THÌ** dừng pilot miễn phí; phỏng vấn mọi shop không trả trong 7 ngày; đợt sau chỉ pilot có phí 1,56tr₫ (hoàn tiền nếu containment <55,3%). **KHÔNG THÌ** không giảm phí tối thiểu hay giá 3.900₫.
5. **NẾU** giờ triển khai >6,2h/shop **TRÊN** 3 shop liên tiếp **THÌ** viết script sync catalog + checklist onboarding trong 1 sprint, chỉ nhận shop có catalog đủ size/tồn kho. **KHÔNG THÌ** không thuê CTV onboarding để chạy kịp chỉ tiêu.

### Cổng gác 90 ngày

| Ngày | Metric (1) | Ngưỡng qua cổng | Bằng chứng | Nếu trượt |
|---|---|---|---|---|
| 30 · 08/11/2026 (học) | Containment đo thật | Có số trên ≥300 hội thoại gán nhãn từ ≥3 shop **và** ≥55,3% | `eval_v1.csv` + `containment_w4.md` | Thiếu mẫu hoặc 44,2–55,2% có nguyên nhân rõ → **FIX** (1 lần); <44,2% (GM <50%) → **PIVOT** phân khúc |
| 60 · 08/12/2026 | POC → paid | ≥60% (≥3/5 shop pilot trả hoá đơn đầu) | 3 hoá đơn + sao kê ngân hàng | 2/5 → **FIX**: pilot có phí; ≤1/5 → **PIVOT** value metric/phân khúc |
| 90 · 07/01/2027 | Lãi gộp tháng 12/2026 | ≥16,9tr₫ = burn (≈7 shop × 2,8tr₫) | P&L tháng 12: sao kê + hoá đơn Anthropic theo shop | 8,5–16,8tr₫ → **FIX** kênh (Haravan/KiotViet); <8,5tr₫ → **KILL** |

**KILL CRITERIA:** Nếu đến **07/02/2027** (sau đúng một lần FIX) lãi gộp tháng vẫn **<8,5tr₫** (½ burn), hoặc containment thật vẫn **<55,3%**, thì dừng CaTối và giữ **~4,9 tháng runway** còn lại cho hướng khác.

**CHƯA ĐO ĐƯỢC:** ① **Cả 8 đèn chưa có số thật**: cần pilot 5 shop (19/10) + log webhook; số đầu tiên vào 26/10. ② **Cost/Job theo shop**: cần gắn metadata mã shop vào API Claude, xong trước 25/10. ③ **Tỷ lệ báo giá/tồn kho sai**: cần eval 300 hội thoại, có số 08/11. ④ **GM ròng**: rev-share Haravan chưa biết, chờ trả lời 30/10, số đầu 12/2026. ⑤ **CAC payback**: cần cohort trả tiền đầu, 01/2027. ⑥ **NRR, sales cycle, pipeline coverage**: cần ≥12 tháng doanh thu / CRM / listing Haravan; sớm nhất 2027. ⑦ **Runway 8,9 tháng là giả định** (mô hình Day22 không có), phải thay bằng số tiền mặt thật trước 16/10.
