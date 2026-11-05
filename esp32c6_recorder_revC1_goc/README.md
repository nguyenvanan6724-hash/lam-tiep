# ESP32-C6 audio recorder – project KiCad 10 (rev C)

Mở `esp32c6_recorder.kicad_pro` bằng KiCad 10 → mở Schematic → **chạy ERC trước** → *Tools → Update PCB from Schematic* (F8).

## Rev C thay đổi gì so với rev B

- **Bỏ CH340C (U11) + mạch tự reset (Q4, Q5, R53–R56, C36, C37).** Nạp code và xem log bằng khối USB Serial/JTAG có sẵn trong ESP32-C6.
- USB-C D+/D− → R70/R71 (22 Ω) → GPIO13/GPIO12. Có chừa chỗ C44/C45 xuống GND (KHÔNG hàn), theo khuyến nghị của Espressif.
- Thẻ SD dời khỏi GPIO12/13: **SD_SCK → GPIO18** (qua R62), **SD_MISO → GPIO19**. SD_CS (IO10) và SD_MOSI (IO11) giữ nguyên.
- UART0 (GPIO16/17) để trống (có cờ no-connect).
- Mọi linh kiện có thêm trường `MPN` và `LCSC` (mã LCSC đã kiểm còn hàng ngày 04/11/2026).
- Net class `USB` có thêm `USB_DP_MCU`, `USB_DM_MCU`.

## Firmware cần chỉnh

- ESP-IDF: `idf.py menuconfig` → Component config → ESP System Settings → Channel for console output = **USB Serial/JTAG**.
- Arduino: Tools → **USB CDC On Boot: Enabled**.
- Firmware treo, ngủ sâu, hoặc lấy IO12/IO13 làm GPIO thì cổng USB sẽ mất. Cách nạp lại: **giữ BOOT, bấm RESET, thả BOOT** để chip vào ROM download, cổng USB sẽ hiện lại.

## File trong project

| File | Nội dung |
|---|---|
| `esp32c6_recorder.kicad_sch` | Schematic rev C (A2) |
| `bom.csv` | BOM gộp: footprint, MPN, **mã LCSC, giá "từ", tồn kho, link** |
| `lib/esp32c6_rec.kicad_sym` | Symbol riêng (vẫn còn symbol CH340C trong thư viện, schematic không dùng nữa) |
| `lib/esp32c6_rec.pretty/` | Footprint riêng: ESP32-C6-WROOM-1, TPS63802 (DLA0010A) |
| `schematic_rev_B_cu.pdf`, `erc_rev_B_cu.rpt` | Bản in và kết quả ERC của **rev B cũ**, giữ lại để so sánh |

## Phải tự kiểm tra

1. **ERC trong KiCad 10.** Rev C được sửa trực tiếp trong file schematic, chưa mở bằng KiCad. Tao đã tự kiểm netlist bằng script (chỉ đổi đúng các net dự kiến, không có dây treo hay nhãn lạc), nhưng vẫn phải chạy ERC thật.
2. In PDF rev C mới: File → Plot.
3. L2: mã mua là Murata **DFE201612E**-R47M, còn footprint đang gán là **DFE201610P**. Hai loại cùng thân 2.0×1.6 mm, nhưng phải so land pattern trong datasheet Murata trước khi đặt PCB.
4. Footprint TPS63802 do Claude tự vẽ theo DLA0010A (TI SLVSEU9D). In tỉ lệ 1:1 và so lại.
5. Khoảng cách 2 hàng chân module INMP441 (J8/J9, J10/J11) phải đo trên module thật.
6. LED 0603: dấu cực tính trên LED LCSC mỗi hãng một kiểu, xem datasheet trước khi hàn.
