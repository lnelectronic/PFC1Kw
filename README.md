# โปรเจคจบ: การออกแบบและสร้างวงจร Active Power Factor Correction (PFC) แบบ Boost Converter 1 kW

> เอกสารนี้เป็นโครงร่างสารบัญ (Table of Contents) สำหรับใช้เขียนรายงานโปรเจคจบฉบับเต็มส่งอาจารย์ที่ปรึกษา

## ข้อกำหนดการออกแบบ (Design Specifications)

| พารามิเตอร์ | ค่า |
|---|---|
| แรงดันไฟฟ้าขาเข้า (Vin) | 220 Vac, 50 Hz (198–242 Vac, ±10%) |
| แรงดันไฟฟ้าขาออก (Vout) | 400 Vdc |
| กำลังไฟฟ้าขาออกสูงสุด (Pout) | 1000 W |
| ไอซีควบคุม (IC Control) | UCC28180 (Average Current Mode, CCM) |
| ความถี่สวิตชิ่ง (fsw) | 70 kHz |
| ตัวประกอบกำลัง (Power Factor) | ≥ 0.96 (ที่โหลด 50–100%) |

**ขอบเขตงานวิจัย:** ศึกษา ออกแบบ คำนวณ จำลอง และสร้างต้นแบบวงจร PFC ชนิด Boost แบบ Active ทดสอบด้วยโหลดตัวต้านทานเชิงเส้น (Resistive Load) เท่านั้น ไม่ครอบคลุมการออกแบบ/วิเคราะห์เชิงลึกด้าน EMI, การทดสอบความทนทานระยะยาว และการออกแบบเพื่อผลิตเชิงพาณิชย์

---

## สารบัญ (Table of Contents)

### ภาคที่ 1: การคำนวณและการออกแบบเชิงทฤษฎี (Design & Theory)

1. **การกำหนดข้อกำหนดและเป้าหมายการออกแบบ (Design Specifications)**
   สเปคขาเข้า/ขาออก, กำลังไฟฟ้าเป้าหมาย, ประสิทธิภาพเป้าหมาย (η ≈ 93–95%), PF/THD target, hold-up time, temperature derating

2. **การคำนวณหาค่ากระแสพื้นฐานทางไฟฟ้า (Basic Current Calculations)**
   Pin = Pout/η, Iout(max) = Pout/Vout ≈ 2.5 A, Iin_rms(max) และ Iin_pk(max) ที่ Vin_min = 198 Vac (worst case)

3. **การออกแบบและคำนวณหาพารามิเตอร์ของตัวเหนี่ยวนำบูสต์ (Boost Inductor Design)**
   เลือก %ripple current, คำนวณ L จากสมการ worst-case duty cycle, เลือกแกน/จำนวนรอบ, ตรวจสอบ saturation current และ core loss ที่ 70 kHz

4. **การเลือกพิกัดไดโอดบูสต์และการวิเคราะห์ความสูญเสีย (Boost Diode Selection)**
   VRRM ≥ 400 V + margin, IF(avg) จาก Iout, เลือกชนิด Ultrafast/SiC Diode เพื่อลด reverse recovery loss ที่ความถี่สูง

5. **การเลือกพิกัด MOSFET และการประเมินกำลังสูญเสียจากการสวิตชิ่ง (MOSFET Selection)**
   VDS ≥ 500–600 V, คำนวณ Irms ผ่านสวิตช์, เลือก RDS(on), ประเมิน conduction loss และ switching loss ที่ 70 kHz

6. **การออกแบบและเลือกขนาดตัวเก็บประจุเอาต์พุต (Output Bulk Capacitor Design)**
   คำนวณจาก hold-up time requirement และ voltage ripple ที่ความถี่ 100 Hz (double-line frequency), ตรวจสอบ ripple current rating

7. **การออกแบบวงจรร่วมสนับสนุนไอซี UCC28180 และระบบป้องกัน (IC Peripheral Circuits)**
   RT resistor กำหนด fsw = 70 kHz, VCC supply/UVLO, Soft-start, Enable, วงจร Current Sense (RSENSE), OVP, Peak Current Limit (PKLMT)

8. **การออกแบบลูปควบคุมแรงดันและกระแส (Control Loop Compensation Design)**
   ตัวแบ่งแรงดัน VSENSE และการชดเชยที่ VCOMP pin (bandwidth ต่ำ < 20 Hz เพื่อลด distortion กระแสอินพุต), Voltage Feedforward (VFF), Multiplier (IMO) และการชดเชยที่ CAOUT

9. **การออกแบบวงจรกรองสัญญาณรบกวนอินพุต (Input EMI Filter Design)**
   วงจรกรอง Differential-mode และ Common-mode แบบ two-stage ตามมาตรฐานอ้างอิง (เช่น EN55022 หรือ มอก.)

### ภาคที่ 2: การจำลองสถานการณ์และการสร้างตัวต้นแบบ (Simulation & Prototyping)

10. **ผลการจำลองสถานการณ์การทำงานของวงจร (Circuit Simulation Results via LTspice/TINA-TI)**
    เน้นผลลัพธ์ THD, ประสิทธิภาพ และสัญญาณควบคุมจากโปรแกรมจำลอง เพื่อใช้เป็นฐานข้อมูลเปรียบเทียบกับผลการทดลองจริง

11. **การสร้างตัวต้นแบบและการวัดค่าพารามิเตอร์ชิ้นส่วนจริง (Prototype Fabrication & Component Characterization)**
    ใช้ LCR Meter วัดและบันทึกค่าจริงของ Inductor (L/DCR), Capacitor (C/ESR) และ EMI Filter ก่อนประกอบลงบอร์ด เพื่อเทียบกับค่าทางทฤษฎี

### ภาคที่ 3: การทดสอบและการประเมินผลด้วยเครื่องมือวัด (Testing & Verification)



12. **การทดสอบการทำงานของระบบควบคุมที่แรงดันต่ำ (Low-Voltage Safe Testing & Gate Drive Verification)**
    ใช้ออสซิลโลสโคปตรวจเช็คสัญญาณ PWM, Soft-start และพฤติกรรมของไอซีควบคุม เพื่อความปลอดภัยก่อนจ่ายไฟจริง — ทำได้เต็มรูปแบบด้วยเครื่องมือที่มี เพราะจ่ายไฟส่วนควบคุมด้วย DC bench supply แยกต่างหาก ไม่ได้ต่อกับไฟเมนจึงไม่มีปัญหาเรื่อง ground

13. **ผลการทดสอบการทำงานที่สภาวะโหลดสูงสุด (Full-Load Testing & Waveform Analysis)**
    ใช้สโคปวัดคลื่นกระแส/แรงดันอินพุตเพื่อดู Phase Shift (ประเมิน PF) และใช้โหมด FFT ดูการลดลงของสัญญาณรบกวนความถี่สูง (แทน Spectrum Analyzer) — 

14. **การวิเคราะห์ประสิทธิภาพและสรุปผลการทดลอง (Efficiency Evaluation & Conclusion)**
    ใช้มัลติมิเตอร์วัดกำลังไฟฟ้าฝั่งเอาต์พุต (V_DC × I_DC) ร่วมกับสโคปฝั่งอินพุต เพื่อสรุปประสิทธิภาพเปรียบเทียบกับสเปคเป้าหมาย — 

---

## สรุปผลการคำนวณเบื้องต้น (Preliminary Calculation Results)

> อ้างอิงสมการจาก TI Datasheet UCC28180 (SLUSBQ5D หัวข้อ 9.2.2) คำนวณที่ Vin_min = 198 Vac (worst case)
> ค่าสมมติ: η = 0.94, Power Factor = 0.99, ΔI_ripple = 30% ของ Iin_pk (ควรปรับตามอุปกรณ์จริงที่เลือกใช้)

| พารามิเตอร์ | ค่าที่คำนวณได้ |
|---|---|
| Iout(max) | 2.5 A |
| Iin_rms(max) | 5.43 A |
| Iin_pk(max) / Iin_avg(max) | 7.68 A / 4.89 A |
| RFREQ (สำหรับ fsw = 70 kHz) | ≈ 30.3 kΩ |
| ΔI_ripple / I_L_peak(max) | 2.30 A / 8.83 A |
| Boost Inductor (L_min, worst case D = 0.5) | ≈ 620 µH |
| Duty cycle สูงสุด (ที่จุดพีคของ Vin_min) | 0.30 (30%) |
| ตัวเก็บประจุอินพุต EMI (X-cap) | ≈ 0.22 µF |
| Boost Diode (SiC Schottky, Qrr ≈ 0) | P_diode ≈ 2.5 W, VR > 425 V |
| กระแส RMS ผ่าน MOSFET / พีค | ≈ 3.22 A(rms) / ≈ 8.8 A |
| Sense Resistor (RSENSE) | ≈ 0.027 Ω, P_RSENSE ≈ 0.79 W |
| Output Capacitor (ripple-based, 5% ของ Vout) | ≥ 200 µF → เลือกมาตรฐาน 220–270 µF |
| ตัวแบ่งแรงดันป้อนกลับ VSENSE | RFB1 = 1 MΩ, RFB2 ≈ 12.7 kΩ |

หมายเหตุ: ค่า η, PF และ ΔI_ripple ข้างต้นเป็นค่าสมมติเบื้องต้น ต้องปรับตาม MOSFET/Diode ที่เลือกใช้จริง เพื่อความแม่นยำในการคำนวณ conduction/switching loss และเพื่อให้บรรลุเป้าหมาย PF ≥ 0.96 ตามสเปค

---

## เอกสารและไฟล์อ้างอิงในโปรเจค

- [tools.md](tools.md) — เครื่องมือวัดที่ใช้และข้อจำกัด (Test Equipment & Limitations)
- [Circuit_PFC_V1_Final.pdf](Circuit_PFC_V1_Final.pdf) — ไดอะแกรมวงจรฉบับล่าสุด
- [Datasheets/ucc28180.pdf](Datasheets/ucc28180.pdf) — Datasheet ไอซีควบคุม UCC28180
- [Datasheets/shemeticV1.pdf](Datasheets/shemeticV1.pdf) — วงจร Schematic
- [Datasheets/214_Design.pdf](Datasheets/214_Design.pdf), [Datasheets/anp015_en.pdf](Datasheets/anp015_en.pdf) — Application Note ประกอบการออกแบบ
- [Datasheets/Ch 17.Input Filter Problem.pdf](Datasheets/Ch%2017.Input%20Filter%20Problem.pdf) — อ้างอิงการออกแบบ EMI Filter
- [Images/](Images/) — รูปภาพวงจร, บอร์ดต้นแบบ, และ BOM
