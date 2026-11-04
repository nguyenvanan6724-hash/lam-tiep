# ESP32-C6 audio recorder – project KiCad 10 (rev B)

Mở `esp32c6_recorder.kicad_pro` bằng KiCad 10 → mở Schematic → *Tools → Update PCB from Schematic* (F8) để bắt đầu vẽ layout.

| File | Nội dung |
|---|---|
| `esp32c6_recorder.kicad_sch` | Schematic rev B (khổ A2), đã gán footprint cho mọi linh kiện |
| `esp32c6_recorder.kicad_pro` | Project, có net class sẵn: POWER_1A 0.5 mm, SW_NODE 0.8 mm, USB 0.3 mm, Default 0.25 mm |
| `lib/esp32c6_rec.kicad_sym` | Symbol riêng: ESP32-C6-WROOM-1, TPS63802, CH340C (V3 = power input), header module mic/OLED/pin |
| `lib/esp32c6_rec.pretty/` | Footprint riêng: ESP32-C6-WROOM-1, TPS63802 (DLA0010A) |
| `sym-lib-table`, `fp-lib-table` | Khai báo 2 thư viện riêng ở trên (đường dẫn `${KIPRJMOD}`) |
| `bom.csv` | Danh sách linh kiện gộp (giá trị, footprint, thông số, mã gợi ý, ghi chú) |
| `erc.rpt` | Kết quả ERC: 0 lỗi, 0 cảnh báo |

Đọc giải thích nguyên lý, cách tính và các chỗ cần tự kiểm tra trong `../../giai_thich_schematic_rev_B.pdf`.

Lưu ý:
- Footprint TPS63802 do Claude tự vẽ theo land pattern DLA0010A trong datasheet TI (SLVSEU9D). In tỉ lệ 1:1 và so lại trước khi đặt mạch.
- Symbol và footprint ESP32-C6-WROOM-1 lấy từ thư viện KiCad chính thức của Espressif (github.com/espressif/kicad-libraries), giấy phép CC-BY-SA 4.0 kèm ngoại lệ cho thiết kế, giống thư viện chuẩn của KiCad.
- Khoảng cách 2 hàng chân của module INMP441 (J8/J9, J10/J11) phải đo trên module thật.
