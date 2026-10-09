# Worksheet — CaTối

Họ tên: Nguyễn Văn Biển · MSSV: 2A202602416 · Ngày làm: 09/10/2026

> **CaTối** là AI trực inbox Fanpage cho shop thời trang dùng Haravan. AI trả lời xong các hội thoại hỏi giá, size, còn hàng, phí ship và lên đơn nháp vào Haravan, chủ yếu trong ca tối 21h–23h30.
> **Trạng thái hôm nay (09/10/2026):** chưa có khách. Pilot miễn phí với 5 shop chạy từ 19/10 đến 15/11/2026. Đề xuất partner gửi Haravan trước 16/10, cần câu trả lời có/không trước 30/10/2026.

## Số liệu đầu vào (từ bài Mô hình tài chính & Value Metric — Day22 `NguyenVanBien_Day22_model.xlsx`)

| Số | Giá trị | Nguồn (ô Excel Day22) |
|---|---|---|
| **ARPU** | **4.095.000₫/tháng** (≈ $156,4) = 1.050 hội thoại xử lý xong × 3.900₫ | `4_Channel_Fit!B5` |
| **Gross margin** | **68,4%** (chưa trừ rev-share partner) · mục tiêu sàn 60% | `2_Pricing!B21`, `B32` |
| **CAC** | Ngân sách CAC tối đa **33,6tr₫/shop** ($1.284) · mục tiêu kế hoạch 90 ngày **≤ 3tr₫/shop** · ước tính telesales VN 8,25tr₫ ($315) | `4_Channel_Fit!B9`, `5_90Day_Plan!C17`, `7_Stress_Test!B39` |
| **CAC payback mục tiêu** | **12 tháng** (SMB, Bessemer — kiểm tra 09/10/2026) | `4_Channel_Fit!B8` |
| **Runway** | **8,9 tháng** = 150tr₫ tiền mặt ÷ burn 16,9tr₫/tháng → hết tiền khoảng đầu 07/2027. Burn gồm: hạ tầng + API ở quy mô pilot **$150/tháng = 3,9tr₫** (`7_Stress_Test!B16`) · QA part-time tuyển từ tháng 2 (`5_90Day_Plan!C18`) ≈ ¼ × 12,5tr₫ fully-loaded (`1_Cost_Job!E50`) = **3,1tr₫**, làm tròn 3tr · founder chi phí sống tối thiểu **10tr₫** (giả định) | ⚠️ **Giả định** — mô hình Day22 không có ô runway. Hai khoản chi đầu lấy từ Day22; tiền mặt 150tr₫ và 10tr₫ sinh hoạt founder là giả định của tôi, sẽ xác nhận ngân sách thật trước 16/10/2026. Runway đổi thì phải tính lại MH-3, cổng ngày 90 và kill criteria |
| **Cost/Job** | **1.232₫** ($0,0470) / hội thoại xử lý xong · giá bán 3.900₫ · breakeven containment 55,3% | `1_Cost_Job!B69`, `2_Pricing!B33` |
| Value Metric | Hybrid: phí tối thiểu 1.560.000₫/tháng (gồm 400 hội thoại xử lý xong) + 3.900₫/hội thoại từ thứ 401 | `3_Value_Metric!B32` |
| Containment giả định | 70% (chưa có eval) | `1_Cost_Job!B10` |

Tỷ giá 26.190₫/USD (Vietcombank 06/10/2026). Đây là sản phẩm của bài học, số không phải số liệu mật nên không scale.

---

## Trạm 1 — Loại mô hình

**Ba câu hỏi, trả lời theo hôm nay:**

1. **Ai trả tiền?** Chủ shop thời trang (hộ kinh doanh / doanh nghiệp nhỏ). Không có cá nhân nào trả tiền → không phải B2C.
2. **Ai dùng sản phẩm?** Chính chủ shop: họ cài app, đồng bộ catalog, bật AI cho ca tối, nhận ca chuyển người, xác nhận đơn nháp. Khách mua hàng chat với Fanpage **dưới thương hiệu của shop** và không biết có CaTối. Giá trị (đỡ lương trực page, không mất đơn đêm) rơi vào túi shop.
3. **Có bên trung gian không?** Có, Haravan, nhưng Haravan chỉ là **kênh phân phối** (kho ứng dụng Haravan Apps). Haravan không trả tiền, không dùng sản phẩm, và khách của Haravan chính là shop, tức người trả tiền cho tôi. Theo HANDBOOK §3.3, kênh giới thiệu/phân phối **không phải B2B2C**.

**Câu chốt loại:** Chúng tôi là **B2B** vì tiền đến từ chủ shop thời trang (1,56–4,1tr₫/tháng), người dùng sản phẩm cũng là chủ shop (cài app, bật ca tối, xác nhận đơn nháp trong Haravan), còn khách mua hàng chỉ thấy thương hiệu Fanpage của shop. Haravan chỉ là kênh bán app, không phải người trả tiền, nên không phải B2B2C.

> Vì sao không phải B2C: không có cá nhân nào trả tiền. Vì sao không phải B2B2C: tôi không có thương hiệu trước mặt người mua; Haravan không trả tiền và không có "người dùng cuối" riêng mà tôi phục vụ hộ họ.
> Lưu ý: đây là **B2B SMB, bán tự phục vụ/qua kênh**, không phải enterprise. Vì vậy tôi giữ bảng đèn B2B nhưng đổi thang thời gian (TTFV tính bằng ngày, không phải tháng) và bỏ các đèn chỉ hợp với enterprise.

**Bảng đèn B2B (HANDBOOK §3.2) — chép đủ mọi đèn, đánh dấu theo hôm nay:**

| Đèn | ✅ / 🔧 / ❌ | Số nằm ở đâu / cần gì để đo |
|---|---|---|
| Time-to-first-value (TTFV) ⭐ | 🔧 | Cần log 2 mốc: thời điểm OAuth Haravan + kết nối Fanpage thành công, và ngày shop đạt first value (lũy kế 50 hội thoại xử lý xong + 1 đơn nháp được xác nhận). Có từ shop pilot đầu tiên (19/10) |
| Pipeline coverage | ❌ | Chưa có CRM, chưa có target doanh thu quý; nguồn lead chính (lượt cài từ Haravan Apps) chưa tồn tại vì chưa được listing. Cần Google Sheet CRM + câu trả lời của Haravan (30/10) |
| % deal chết ở security/procurement | ❌ | Shop SMB không có khâu procurement. Thứ tương đương là shop từ chối cấp quyền OAuth/Fanpage và Haravan duyệt listing. Chưa biết đo thế nào cho có nghĩa → không đưa vào dashboard |
| POC → paid | 🔧 | Đợt pilot 5 shop kết thúc 15/11; số có khi shop trả hoá đơn đầu tiên (trước 08/12/2026). Cần sổ theo dõi pilot + sao kê |
| Sales cycle (tuần) | 🔧 | Cần ghi ngày tiếp xúc đầu tiên và ngày trả tiền cho từng shop trong CRM. Mẫu 5 shop quá nhỏ; số có nghĩa sớm nhất 01/2027 |
| Usage depth trong tài khoản | 🔧 | Đo bằng số hội thoại xử lý xong/shop/tháng từ log Messenger webhook. Có từ tuần đầu pilot |
| Chi phí triển khai ÷ ACV | 🔧 | Cần timesheet: số giờ founder bỏ ra cho mỗi shop (sync catalog, cấu hình, hướng dẫn). Bắt đầu ghi từ shop pilot đầu tiên |
| Tập trung doanh thu | 🔧 | Chưa có doanh thu. Tính được ngay khi có hoá đơn đầu tiên (12/2026); trước khi có ≥5 shop trả tiền thì con số này luôn "đỏ" và vô nghĩa |
| NRR | ❌ | Cần ≥12 tháng dữ liệu doanh thu theo shop; số đầu tiên sớm nhất 12/2027 |
| Gross Margin | 🔧 | Cần gắn `metadata` mã shop vào mọi lời gọi API Claude + xuất usage theo shop; GM ròng còn phụ thuộc tỷ lệ rev-share Haravan (chưa biết, chờ 30/10) |
| CAC payback | ❌ | Chưa có khách trả tiền, chưa có CAC thật. Số đầu tiên khi có cohort trả tiền đầu tiên (01/2027) |

**Đèn thêm ngoài bảng §3.2 (cần cho sản phẩm AI của tôi):**

| Đèn | ✅ / 🔧 / ❌ | Số nằm ở đâu / cần gì để đo |
|---|---|---|
| Containment rate (AI tự xử lý xong) | 🔧 | Log Messenger webhook (sender bot vs người) + mốc 24h. Có từ pilot; số tin được sau khi gán nhãn 300 hội thoại (trước 08/11) |
| Cost/Job thực, p95 theo shop | 🔧 | Cần gắn mã shop vào request API + chia hoá đơn Anthropic theo shop. Làm trong tuần 19–25/10 |
| Tỷ lệ báo giá/tồn kho sai | ❌ | Chưa có bộ eval; cần QA đối chiếu với Haravan. Có số sau eval 300 hội thoại (08/11) |

---

## Trạm 2 — Thẻ đèn

**North Star:** **TTFV trung vị** của các shop onboard trong 4 tuần gần nhất — hiện tại **chưa có số** (shop đầu tiên go-live 19/10/2026) — mục tiêu **≤ 7 ngày**.
Lý do: với B2B, deal chết ở khoảng giữa lúc ký và lúc khách thấy giá trị. Pilot của tôi miễn phí 28 ngày, nên shop nào không thấy giá trị sớm thì sẽ không trả tiền.

| # | Tầng | Đèn | Định nghĩa (đếm gì · **không** đếm gì) | Công thức | Nhịp · ai lấy số | Báo trước cho |
|---|---|---|---|---|---|---|
| 1 | **L** | **TTFV** ⭐ | Số ngày từ lúc shop **kết nối xong** (OAuth Haravan + Fanpage webhook nhận tin thật đầu tiên) đến ngày shop **lần đầu đạt cả hai**: lũy kế 50 hội thoại xử lý xong (định nghĩa job Day22) **và** ≥1 đơn nháp CaTối tạo được shop xác nhận giao. **Không đếm** hội thoại do shop/CaTối tự nhắn thử, hội thoại chuyển người, đơn nháp bị huỷ | Ngày đạt mốc − ngày kết nối xong (mỗi shop); dashboard lấy trung vị 4 tuần | Mỗi shop, tổng hợp thứ Hai hằng tuần · founder (script đọc log + Haravan Orders API) | POC→paid (#4) → CAC payback (#8) |
| 2 | **L** | **Containment rate** | % hội thoại hỏi mua được AI xử lý xong: khách nhận đủ thông tin hoặc có đơn nháp, **không có tin do người gõ**, khách không nhắn lại trong 24h. **Mẫu số** = mọi hội thoại mới vào Fanpage trong **khung cố định 21h–23h30** cộng mọi giờ khác AI được bật. Shop tắt AI trong khung 21h–23h30 thì hội thoại khung đó vẫn nằm trong mẫu số (tính là không xong), để không ai làm đẹp số bằng cách chỉ bật AI ở giờ vắng. **Không đếm** (cả tử và mẫu): spam/quảng cáo, tài khoản test, tin chỉ có sticker. Hội thoại QA phát hiện **báo sai giá/tồn kho** bị đếm là **không xong** | Hội thoại xử lý xong ÷ hội thoại thử (gộp mọi shop) | Hằng ngày (cảnh báo), chốt theo tuần · founder, từ log webhook | Cost/Job thực (#3) → GM ròng (#7); và POC→paid (#4) |
| 3 | **L** | **Cost/Job thực — p95 theo shop** (đèn chi phí AI) | Chi phí để **xử lý xong 1 hội thoại**, tính **riêng từng shop**, rồi lấy shop ở phân vị 95 (với <20 shop thì p95 = **shop tệ nhất**). Chi phí gồm: hoá đơn Claude API gắn mã shop + retry + hạ tầng phân bổ theo số hội thoại + QA nội bộ. **Không lấy trung bình** toàn hệ thống; **không** tính chi phí chạy eval offline | (API + retry + infra + QA của shop) ÷ số hội thoại xử lý xong của shop; lấy p95 | Hằng tuần · founder (Anthropic usage export theo metadata + log) | GM ròng (#7) — báo trước khoảng 1 tháng trước khi hiện ra ở báo cáo GM |
| 4 | **O** | **POC → paid** | % shop đã **kết thúc pilot miễn phí 28 ngày** mà trả **hoá đơn đầu tiên** trong 14 ngày sau khi pilot kết thúc. **Không đếm** shop hứa "sẽ trả", shop được gia hạn miễn phí, shop chưa hết 28 ngày | Shop trả hoá đơn đầu ÷ shop đã kết thúc pilot | Mỗi đợt pilot, chốt hằng tháng · founder (sao kê + sổ pilot) | CAC payback (#8) và doanh thu |
| 5 | **O** | **Hội thoại xử lý xong / shop / tháng** (usage depth) | Số job xử lý xong (định nghĩa #2) trong **tháng dương lịch** của mỗi shop **đang trả tiền**, lấy **trung vị**. **Không đếm** shop đang pilot miễn phí, tháng đầu chưa đủ 30 ngày | Trung vị (job xử lý xong/tháng) qua các shop trả tiền | Hằng tháng, xem nhanh hằng tuần · founder | ARPU thật → CAC payback (#8); shop dùng ít là tín hiệu churn |
| 6 | **O** | **Giờ công triển khai / shop** | Tổng giờ founder/QA bỏ ra cho **một shop** từ lúc nhận lời tới khi TTFV đạt: gọi điện, sync & sửa catalog, cấu hình chính sách ship/đổi trả, hướng dẫn. **Không đếm** giờ hỗ trợ sau TTFV và giờ phát triển tính năng chung | Σ giờ ghi timesheet cho shop đó | Mỗi shop · founder (timesheet ghi theo shop) | Năng lực đạt 30 shop/90 ngày, CAC thật → CAC payback (#8) |
| 7 | **G** | **GM ròng** | (Doanh thu đã thu − rev-share Haravan − COGS) ÷ doanh thu đã thu, trong tháng. COGS = API + infra + retry + QA (như Cost/Job Day22). **Không** tính chi phí founder, sales, eval offline | (Doanh thu − rev-share − COGS) ÷ doanh thu | Hằng tháng, báo cáo quý · founder | (kết quả) |
| 8 | **G** | **CAC payback** | Số tháng lãi gộp của một shop trả lại CAC của shop đó. CAC = chi phí quảng cáo/lead + giờ founder bán hàng & triển khai quy ra tiền + ưu đãi pilot. **Không** tính chi phí phát triển sản phẩm | CAC ÷ (ARPU × GM ròng) | Hằng tháng theo cohort, báo cáo quý · founder | (kết quả) |

**Kiểm tra:** 3 Leading · 3 Operating · 2 Lagging (2/8 = 25% < 50%). Đèn chi phí AI là **đèn số 3** (đèn số 2 cũng chạm chi phí, vì containment thấp làm Cost/Job trên mỗi job xong tăng).

**Đèn đã cắt và lý do:** pipeline coverage, % deal chết ở procurement (không đo được có nghĩa với SMB hôm nay), sales cycle (mẫu 5 shop quá nhỏ), tập trung doanh thu (luôn đỏ khi <5 shop), NRR (cần 12 tháng). Các đèn này nằm ở mục "Chưa đo được" của dashboard.

---

## Trạm 3 — Ngưỡng

| # | Đèn | 🟢 | 🟡 | 🔴 | Nguồn | Lý do (1 câu) · ngày kiểm tra nếu [BM] |
|---|---|---|---|---|---|---|
| 1 | TTFV ⭐ | ≤ 7 ngày | 8–14 ngày | > 14 ngày | [MH] | Pilot miễn phí 28 ngày và shop cần ≥ 14 ngày (2 chu kỳ livestream tuần) dùng thật để quyết định trả tiền, nên TTFV > 28 − 14 = 14 ngày là shop không kịp thấy giá trị (phụ lục MH-4); mốc 🟢 7 ngày là mục tiêu activation đã cam kết ở Day22. Ngưỡng tạm, thay bằng baseline [TB] sau 2 đợt onboard (≈ 01/2027) |
| 2 | Containment | ≥ 70,0% | 55,3–69,9% | < 55,3% | [MH] | 55,3% là breakeven để GM còn 60% ở giá 3.900₫ (phụ lục MH-1); 70% là giả định mô hình. **Khi ký rev-share 20% với Haravan, 🔴 tăng lên < 69,1%** |
| 3 | Cost/Job thực p95 | ≤ 1.355₫ | 1.356–1.560₫ | > 1.560₫ | [MH] | GM 60% ⇒ Cost/Job tối đa = 40% × 3.900₫ = 1.560₫; 🟢 là Cost/Job mô hình 1.232₫ + 10% biên nhiễu (phụ lục MH-2). **Khi có rev-share 20%: 🔴 > 1.248₫** |
| 4 | POC → paid | ≥ 60% (≥ 3/5 shop) | 40–59% (2/5) | < 40% (≤ 1/5) | [BM] + [MH] | ICONIQ *State of GTM 2026*: POC/trial → paid ~50% (2026), ~36% (2025), **tôi mở nguồn kiểm tra 09/10/2026**; 🟢 60% là mục tiêu 3/5 shop của kế hoạch Day22; 🔴 < 40% nghĩa là tệ hơn cả mặt bằng 2025 |
| 5 | Hội thoại xử lý xong/shop/tháng | ≥ 1.050 | 400–1.049 | < 400 | [MH] | ARPU 4.095.000₫ giả định 1.050 job/tháng; dưới 400 job shop chỉ trả phí tối thiểu 1.560.000₫ (ARPU giảm 62%) và mục tiêu MRR 30 shop rơi từ 123tr xuống 47tr₫ (phụ lục MH-5) |
| 6 | Giờ công triển khai/shop | ≤ 3,1 giờ | 3,2–6,2 giờ | > 6,2 giờ | [MH] | Để đạt 30 shop ở ngày 90, founder phải onboard ~14,1 shop/tháng bằng ≤ 50% thời gian (88 giờ) ⇒ tối đa 6,2 giờ/shop; 🟢 = ≤ 25% thời gian (phụ lục MH-6). Tiền không phải ràng buộc: 6,2 giờ chỉ ≈ 0,7% ACV |
| 7 | GM ròng | ≥ 60,0% | 53,0–59,9% | < 53,0% | [BM] + [MH] | 60% là GM sàn của mô hình Day22 (dưới đó breakeven containment vượt giả định 70%); 53% là GM trung bình sản phẩm AI 2026P theo ICONIQ *State of AI 2026* (45% 2025 → 53% 2026 → 59% 2027), **tôi mở nguồn kiểm tra 09/10/2026**; thấp hơn mặt bằng ngành là tín hiệu đỏ |
| 8 | CAC payback | ≤ 3,0 tháng | 3,1–5,9 tháng | > 5,9 tháng | [MH] (trần [BM] 12 tháng) | Runway 8,9 tháng: shop có được ở ngày 90 phải hoàn vốn trước khi hết tiền ⇒ ≤ 5,9 tháng, chặt hơn chuẩn SMB < 12 tháng của Bessemer *Scaling to $100 Million* (bvp.com/atlas/scaling-to-100-million, **tôi mở nguồn kiểm tra 09/10/2026**); 🟢 3,0 tháng ⇔ CAC ≤ 8,4tr₫ ≈ CAC telesales VN ước tính 8,25tr₫ (phụ lục MH-3) |

**Benchmark tôi cố ý KHÔNG dùng:** TTFV < 30 ngày và sales cycle 19 tuần của bảng B2B. Đó là số của B2B có sales (ICONIQ khảo sát công ty phần mềm Mỹ, deal lớn). Shop SMB tự cài app phải thấy giá trị trong vài ngày, nên dùng ngưỡng 30 ngày ở đây là đặt ngưỡng lỏng tới mức đèn không bao giờ đỏ.

### Phụ lục [MH] — phép tính

**[MH] 1 — Containment rate (breakeven)**

```
Đầu vào (Day22, 2_Pricing):
  v = chi phí biến đổi / hội thoại thử (API + infra + retry) = $0,029539
  q = QA nội bộ / hội thoại thử                             = $0,003390
  e = chi phí 1 ca escalate (Biến thể A → shop tự xử lý)     = $0
  P = giá / hội thoại xử lý xong = 3.900₫ = $0,148912 · GM mục tiêu = 60%
Phép tính: R_min = (v + q + e) / (P × (1 − GM) + e)
          = 0,032929 / (0,148912 × 0,40) = 0,032929 / 0,059565 = 55,3%
  Nếu Haravan lấy rev-share r = 20%: P_ròng = 0,148912 × 0,8 = 0,119130
          R_min = 0,032929 / (0,119130 × 0,40) = 0,032929 / 0,047652 = 69,1%
Kết quả → 🟢 ≥ 70,0% (giả định mô hình, GM 68,4%) · 🟡 55,3–69,9% · 🔴 < 55,3%
          (sau khi ký rev-share 20%: 🔴 < 69,1%, vùng vàng gần như biến mất)
```

**[MH] 2 — Cost/Job thực p95 (đèn chi phí AI)**

```
Đầu vào: giá 3.900₫/hội thoại xử lý xong · GM sàn 60% · Cost/Job mô hình 1.232₫
Phép tính: Cost/Job tối đa = 3.900 × (1 − 60%) = 1.560₫
           Mức xanh = 1.232 × 1,10 = 1.355₫ → GM = 1 − 1.355/3.900 = 65,3%
           Có rev-share 20%: 3.900 × 0,8 × 40% = 1.248₫ (chỉ cao hơn mô hình 1,3%)
Kết quả → 🟢 ≤ 1.355₫ · 🟡 1.356–1.560₫ · 🔴 > 1.560₫
Ghi chú: giá 3.900₫ là giá thấp nhất mỗi job; shop chỉ trả phí tối thiểu có giá
hiệu dụng cao hơn, nên ngưỡng này bảo thủ. Day22 tab 7: lên Sonnet 5.5 → 2.081₫ (đỏ).
```

**[MH] 3 — CAC payback (từ runway)**

```
Đầu vào: ARPU 4.095.000₫ · GM 68,4% · runway 8,9 tháng (150tr ÷ 16,9tr/tháng, giả định)
Lãi gộp / shop / tháng = 4.095.000 × 68,4% = 2.801.000₫
Ngân sách CAC theo chuẩn 12 tháng (Day22) = 2.801.000 × 12 = 33,6tr₫ (trần lỏng)
Ràng buộc runway: shop có được ở ngày 90 (tháng 3,0) phải hoàn vốn trước tháng 8,9
           → payback tối đa = 8,9 − 3,0 = 5,9 tháng ⇔ CAC tối đa = 2.801.000 × 5,9 = 16,5tr₫
Mức xanh: 3,0 tháng ⇔ CAC ≤ 2.801.000 × 3 = 8,4tr₫ (≈ CAC telesales VN ước tính 8,25tr₫)
Kết quả → 🟢 ≤ 3,0 tháng · 🟡 3,1–5,9 tháng · 🔴 > 5,9 tháng
```

**[MH] 4 — TTFV (từ thiết kế pilot)**

```
Đầu vào: pilot miễn phí 28 ngày · shop cần ≥ 2 chu kỳ livestream/đăng bài hằng tuần
         (= 14 ngày) có AI chạy thật mới đủ bằng chứng để trả tiền
Phép tính: TTFV tối đa = 28 − 14 = 14 ngày
           Mục tiêu activation Day22 = 7 ngày → còn 21 ngày (3 chu kỳ) để thấy giá trị
Kết quả → 🟢 ≤ 7 ngày · 🟡 8–14 ngày · 🔴 > 14 ngày
```

**[MH] 5 — Hội thoại xử lý xong / shop / tháng**

```
Đầu vào: phí tối thiểu 1.560.000₫ gồm 400 job · 3.900₫/job từ job 401 · ARPU mô hình dựa trên 1.050 job
Phép tính: ARPU(1.050) = 1.560.000 + (1.050 − 400) × 3.900 = 1.560.000 + 2.535.000 = 4.095.000₫
           ARPU(< 400) = 1.560.000₫ = 38% ARPU mô hình (giảm 62%)
           MRR 30 shop: 30 × 4.095.000 = 122,9tr₫ → 30 × 1.560.000 = 46,8tr₫
Kết quả → 🟢 ≥ 1.050 · 🟡 400–1.049 · 🔴 < 400
```

**[MH] 6 — Giờ công triển khai / shop**

```
Đầu vào: mục tiêu 30 shop trả tiền ở ngày 90 (Day22) · 5 shop từ pilot → 25 shop mới
         trong 15/11/2026 → 07/01/2027 = 53 ngày = 1,77 tháng
         founder 176 giờ/tháng, tối đa 50% cho onboarding = 88 giờ
Phép tính: shop mới / tháng = 25 ÷ 1,77 = 14,1
           giờ tối đa / shop = 88 ÷ 14,1 = 6,2 giờ · mức xanh (25% thời gian) = 44 ÷ 14,1 = 3,1 giờ
           quy ra tiền: 6,2 × (10.000.000 ÷ 176) = 352.000₫ = 0,7% ACV (49,1tr₫)
Kết quả → 🟢 ≤ 3,1 giờ · 🟡 3,2–6,2 giờ · 🔴 > 6,2 giờ
```

---

## Trạm 4 — 5 luật quyết định

Đánh dấu ⏹ cho luật dừng (có 3).

1. ⏹ **TTFV** — **NẾU** TTFV > 14 ngày **TRÊN** 2 trong 3 shop onboard gần nhất **THÌ** dừng nhận shop mới (kể cả pilot) từ thứ Hai tuần sau; founder ngồi với 2 shop đó trong 48 giờ để tìm điểm tắc, và cắt phạm vi onboarding xuống một luồng (chỉ trả lời giá/size/còn hàng, tắt lên đơn nháp) cho tới khi một shop mới đạt TTFV ≤ 7 ngày. **KHÔNG THÌ** không được gia hạn pilot miễn phí "thêm 2 tuần cho shop thấy giá trị", và không đi tìm thêm shop pilot để bù số.
2. ⏹ **Containment** — **NẾU** containment < 55,3% (hoặc < 69,1% sau khi ký rev-share 20%) **TRONG** 2 tuần liên tiếp **VÀ** mỗi tuần ≥ 300 hội thoại thử từ ≥ 3 shop **THÌ** đóng băng mọi tính năng mới và dừng nhận shop mới trong 1 sprint; gán nhãn 100 hội thoại thất bại gần nhất và sửa đúng 2 nguyên nhân lớn nhất (catalog thiếu size/tồn kho, chính sách ship, prompt). **KHÔNG THÌ** không được nâng model lên Sonnet 5.5 cho "AI thông minh hơn" (Cost/Job 2.081₫, GM rơi xuống 46,6%), và không được nới định nghĩa "xử lý xong" (bỏ mốc 24h, đếm cả ca có người gõ) để số đẹp lên.
3. **Cost/Job p95** — **NẾU** Cost/Job thực p95 > 1.560₫ (hoặc > 1.248₫ khi có rev-share 20%) **TRONG** 1 tháng **VÀ** shop đó có ≥ 300 hội thoại xử lý xong trong tháng **THÌ** trong sprint tới đặt trần 8 lượt/hội thoại và bỏ ảnh khỏi lịch sử sau lượt thứ 2 cho shop vượt ngưỡng, rồi chạy bộ eval 300 hội thoại trên Haiku 5.5 để quyết định có hạ model không. **KHÔNG THÌ** không tăng giá 3.900₫ cho mọi shop (shop dùng nhẹ phải gánh cho shop nặng), và không chuyển sang Biến thể B (CaTối tự trả lời ca khó) để "giữ chất lượng" (GM rơi còn ~29%).
4. ⏹ **POC → paid** — **NẾU** POC → paid < 40% **TRÊN** một đợt pilot ≥ 5 shop (≤ 1/5 trả tiền) **THÌ** dừng mở đợt pilot miễn phí mới; trong 7 ngày phỏng vấn mọi shop không trả tiền theo 3 câu cố định (đã thấy giá trị chưa · thấy ở đâu · vì sao không trả); đợt sau chỉ chạy pilot **có phí** 1.560.000₫, hoàn tiền nếu containment của shop < 55,3%. **KHÔNG THÌ** không giảm phí tối thiểu hay giá 3.900₫ để cứu deal: 1,56tr₫ chỉ bằng 22% lương một nhân viên trực page, nên shop không trả không phải vì giá mà vì chưa thấy giá trị.
5. **Giờ công triển khai** — **NẾU** giờ công triển khai > 6,2 giờ/shop **TRÊN** 3 shop liên tiếp **THÌ** trong 1 sprint viết script sync catalog tự động + checklist onboarding 1 trang, và từ tuần sau chỉ nhận shop có catalog Haravan đã đủ size và tồn kho. **KHÔNG THÌ** không thuê freelancer/CTV onboarding để chạy kịp chỉ tiêu 30 shop: làm vậy là biến CaTối thành công ty dịch vụ mặc áo sản phẩm, và chi phí đó không giảm theo từng shop.

**Kiểm tra:** 5 luật · mỗi luật đủ NẾU / TRONG-TRÊN / THÌ / KHÔNG THÌ · có vế VÀ ở các luật dễ nhiễu do mẫu nhỏ (#2, #3) · 3 luật dừng ⏹ (#1, #2, #4) · không luật nào kết thúc bằng "xem xét lại".

**Phản xạ sai tôi đã tự hỏi trước khi viết vế KHÔNG THÌ:**

| Đèn đỏ | Phản xạ đầu tiên của tôi | Vì sao sai |
|---|---|---|
| TTFV | Cho shop thêm thời gian miễn phí | Kéo dài pilot chỉ làm chậm câu trả lời; shop không thấy giá trị ở ngày 14 thì ngày 28 cũng khó thấy |
| Containment | Đổi sang model mạnh hơn | Lỗi thường nằm ở dữ liệu catalog chứ không ở model; Sonnet 5.5 kéo GM xuống 46,6% |
| Cost/Job | Tăng giá đại trà | Phạt 90% shop nhẹ vì vài shop nặng |
| POC → paid | Giảm giá | B2B chết vì không thấy giá trị, không phải vì giá |
| Giờ triển khai | Thuê thêm người | Biến chi phí không scale thành chi phí cố định |

---

## Nhật ký phản biện bằng AI (HANDBOOK §5)

AI chỉ dùng để phản biện; quyết định nhận/bác là của tôi. Chạy ngày 09/10/2026.

| Prompt | AI chỉ ra | Quyết định | Đã làm gì / vì sao |
|---|---|---|---|
| 5.1 Tier Audit | Cost/Job p95 có thật là Leading? | ACCEPT (giữ nguyên) | Số đổi theo tuần từ log token, báo trước GM ròng khoảng 1 tháng → giữ ở tầng L |
| 5.1 Tier Audit | Mẫu số containment "trong giờ AI bật" có hai cách đọc và lách được bằng cách chỉ bật AI giờ vắng | ACCEPT | Cố định mẫu số theo khung 21h–23h30; shop tắt AI trong khung vẫn bị tính là không xong (thẻ đèn #2) |
| 5.1 Tier Audit | Thiếu đèn chất lượng nhìn từ người mua (báo sai giá) | PARTIAL | Không thêm đèn thứ 9; hội thoại QA phát hiện báo sai bị tính là "không xong" trong containment, tỷ lệ báo sai đưa vào mục Chưa đo được (eval 08/11) |
| 5.2 Threshold | GM 60% là số tròn | REJECT | 60% là sàn của mô hình Day22, cũng là tham số trong MH-1/MH-2; mức đỏ 53% là benchmark ICONIQ có ngày kiểm tra |
| 5.2 Threshold | Ngưỡng dễ sai nhất: TTFV 14 ngày (giả định "2 chu kỳ livestream") | ACCEPT | Ghi rõ là ngưỡng tạm, đổi sang [TB] sau 2 đợt onboard (≈ 01/2027) |
| 5.2 Threshold | Dùng TTFV < 30 ngày của bảng B2B | REJECT | Đó là số của B2B có sales; shop SMB tự cài app thì ngưỡng 30 ngày lỏng tới mức đèn không bao giờ đỏ |
| 5.3 Red-Team | Luật 3 bắn nhầm: với < 20 shop, p95 = shop tệ nhất, một shop gửi nhiều ảnh là kích hoạt | ACCEPT | Thêm vế VÀ ≥ 300 job xong/tháng; hành động THÌ chỉ áp cho shop vượt ngưỡng, không áp cho mọi shop |
| 5.3 Red-Team | Luật 2 lách được bằng cách nới định nghĩa "xử lý xong" | ACCEPT | Đưa việc này vào vế KHÔNG THÌ của luật 2 |
| 5.3 Red-Team | Luật còn thiếu: Haravan đòi rev-share > 30% | REJECT thêm luật thứ 6 | Ngưỡng đỏ containment và Cost/Job đã có phiên bản theo rev-share (69,1% / 1.248₫); GM ròng < 53% và kill criteria bắt được trường hợp này |
| 5.4 Wrong-Reflex | Phản xạ sai khi POC→paid đỏ: giảm giá | ACCEPT | Cấm ở vế KHÔNG THÌ luật 4; lý do: 1,56tr₫ chỉ bằng 22% lương một nhân viên trực page |
| 5.6 Gate Designer | Cổng ngày 90 có cần số shop 30 như kế hoạch Day22? | REJECT | Cổng là mức tối thiểu để sống (lãi gộp ≥ burn 16,9tr₫ ≈ 7 shop); 30 shop là mục tiêu, không phải điều kiện qua cổng |
