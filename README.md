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

> เครื่องมือที่มี: ออสซิลโลสโคป Siglent SDS1104X-E (4CH, 100MHz, ไม่มี isolation ในตัว), LCR Meter, เครื่องมือช่างอิเล็กทรอนิกส์ทั่วไป (มัลติมิเตอร์, หัวแร้ง, เพาเวอร์ซัพพลาย ฯลฯ)
> ดูหมายเหตุข้อจำกัดของเครื่องมือท้ายตารางสารบัญ ก่อนดำเนินการข้อ 13–14

12. **การทดสอบการทำงานของระบบควบคุมที่แรงดันต่ำ (Low-Voltage Safe Testing & Gate Drive Verification)**
    ใช้ออสซิลโลสโคปตรวจเช็คสัญญาณ PWM, Soft-start และพฤติกรรมของไอซีควบคุม เพื่อความปลอดภัยก่อนจ่ายไฟจริง — ทำได้เต็มรูปแบบด้วยเครื่องมือที่มี เพราะจ่ายไฟส่วนควบคุมด้วย DC bench supply แยกต่างหาก ไม่ได้ต่อกับไฟเมนจึงไม่มีปัญหาเรื่อง ground

13. **ผลการทดสอบการทำงานที่สภาวะโหลดสูงสุด (Full-Load Testing & Waveform Analysis)**
    ใช้สโคปวัดคลื่นกระแส/แรงดันอินพุตเพื่อดู Phase Shift (ประเมิน PF) และใช้โหมด FFT ดูการลดลงของสัญญาณรบกวนความถี่สูง (แทน Spectrum Analyzer) — **ทำได้บางส่วน โปรดดูข้อจำกัดด้านล่าง**

14. **การวิเคราะห์ประสิทธิภาพและสรุปผลการทดลอง (Efficiency Evaluation & Conclusion)**
    ใช้มัลติมิเตอร์วัดกำลังไฟฟ้าฝั่งเอาต์พุต (V_DC × I_DC) ร่วมกับสโคปฝั่งอินพุต เพื่อสรุปประสิทธิภาพเปรียบเทียบกับสเปคเป้าหมาย — **ทำได้บางส่วน โปรดดูข้อจำกัดด้านล่าง**

### ข้อจำกัดของเครื่องมือที่มี (Equipment Limitations — โปรดอ่านก่อนทดสอบข้อ 13–14)

จากเครื่องมือที่มี (SDS1104X-E, LCR Meter, เครื่องมือช่างทั่วไป) มีจุดที่ **ทำไม่ได้ตรงตามที่เขียนในสารบัญ** และต้องปรับวิธีวัดหรือเพิ่มอุปกรณ์ ดังนี้

| ข้อ | ปัญหา | สาเหตุ | แนวทางแก้ |
|---|---|---|---|
| 13 | **ห้ามต่อ probe ของสโคปตรงเข้าวงจร PFC ขณะจ่ายไฟเมนจริง** | SDS1104X-E เป็นสโคปแบบ ground-referenced (ground ของทุกช่องรวมถึงสายดินเครื่อง ต่อร่วมกันและต่อลง earth ผ่านปลั๊กไฟ) วงจร Boost PFC ไม่มี isolation จากไฟเมน ถ้าหนีบ ground clip ผิดจุด (เช่น ฝั่ง hot หรือกลางบัส DC ที่ไม่ใช่ 0V อ้างอิงเดียวกับ earth) จะเกิดไฟช็อตผ่านสาย ground ของ probe ทันที เสี่ยงทั้งไฟดูด อุปกรณ์เสียหาย และเบรกเกอร์ตัด | ต้องใช้ **หม้อแปลงแยกกราวด์ (Isolation Transformer)** คั่นระหว่างไฟเมนกับวงจร PFC ก่อน จึงจะใช้สโคปธรรมดาวัดได้อย่างปลอดภัย หรือใช้ **Differential Probe** แทน probe ธรรมดาที่จุดวัดซึ่งไม่ได้อ้างอิงกับ earth |
| 13 | **ไม่มีอุปกรณ์วัดกระแสอินพุตเป็นคลื่น (waveform)** | การดู Phase Shift ระหว่าง Vin/Iin เพื่อประเมิน PF ต้องมีสัญญาณกระแสเป็นแรงดันเทียบเวลาบนสโคป แต่ในเครื่องมือที่ระบุมาไม่มี Current Probe/Current Clamp ที่ใช้กับสโคปได้ (มัลติมิเตอร์ทั่วไปวัดได้แค่ค่า RMS ตัวเลข ไม่ใช่คลื่น) | ต้องหา **Current Probe (Clamp-on CT) ที่ใช้กับสโคปได้** หรือใช้ **ตัวต้านทานชันท์ (shunt) ค่าน้อยมากต่ออนุกรมกับสาย + Differential Probe** วัดแรงดันคร่อมชันท์แทน (ต้องระวังเรื่อง isolation เช่นกัน) |
| 13 | **โหมด FFT บนสโคปใช้แทน Spectrum Analyzer ได้แค่เชิงคุณภาพ** | FFT ของสโคป bench-grade มี dynamic range/resolution ต่ำกว่า EMI Receiver มาก และไม่ได้ผ่านวงจร LISN ตามมาตรฐาน จึงนำไปเทียบกับ limit line ของ EN55022/มอก. โดยตรงไม่ได้ | ใช้รายงานผลเป็น "แนวโน้มการลดลงของสัญญาณรบกวนเชิงเปรียบเทียบ" เท่านั้น ไม่ใช่ผลทดสอบมาตรฐาน — สอดคล้องกับขอบเขตงานที่ระบุไว้แล้วว่าไม่ครอบคลุมการวิเคราะห์ EMI เชิงลึก |
| 13 | **แรงดันเกินพิกัด probe ที่มากับสโคป** | probe 10x มาตรฐานที่แถมกับ SDS1104X-E ส่วนใหญ่ CAT I/II และพิกัดแรงดัน 300V ขึ้นไป แต่ต้องเช็คว่าเกิน 400Vdc bus + ripple ที่จะวัดหรือไม่ | ตรวจสอบพิกัดแรงดัน probe ก่อนใช้งานทุกครั้ง ถ้าต่ำกว่า 600V ควรเปลี่ยนเป็น probe พิกัดสูงกว่า หรือใช้ differential probe |
| 14 | **มัลติมิเตอร์วัด V×I ฝั่งอินพุต AC จะได้ Apparent Power (S) ไม่ใช่ Real Power (P)** | P = V×I×PF ถ้าคูณค่า RMS ของ V และ I ตรงๆ จากมัลติมิเตอร์ 2 ตัว จะได้ S ไม่ใช่ P จริง ทำให้ค่าประสิทธิภาพที่คำนวณได้คลาดเคลื่อน (ยกเว้น PF = 1 พอดี) | ฝั่งเอาต์พุต (DC) ใช้ V_DC × I_DC ได้ตรงๆ ไม่มีปัญหา — ฝั่งอินพุต (AC) แนะนำใช้สโคป (เมื่อมี current probe + differential probe ตามข้อ 13 แล้ว) คูณสัญญาณ V และ I ด้วย Math channel (Power/Watt) ของสโคปเพื่อหาค่ากำลังจริงเฉลี่ยแทน หรือใช้เครื่อง Power Meter/Wattmeter ถ้าหาได้ |

**สรุปสั้น:** ข้อ 10–12 ทำได้ตามที่เขียนไว้ด้วยเครื่องมือที่มีครบ ข้อ 13–14 **ทำได้แค่บางส่วน** — ต้องเพิ่ม Isolation Transformer และ Current Probe อย่างน้อย ก่อนจึงจะวัดที่สภาวะโหลดสูงสุดขณะต่อไฟเมนจริงได้อย่างปลอดภัยและได้ค่าที่แม่นยำ

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

- [Circuit_PFC_V1_Final.pdf](Circuit_PFC_V1_Final.pdf) — ไดอะแกรมวงจรฉบับล่าสุด
- [Datasheets/ucc28180.pdf](Datasheets/ucc28180.pdf) — Datasheet ไอซีควบคุม UCC28180
- [Datasheets/shemeticV1.pdf](Datasheets/shemeticV1.pdf) — วงจร Schematic
- [Datasheets/214_Design.pdf](Datasheets/214_Design.pdf), [Datasheets/anp015_en.pdf](Datasheets/anp015_en.pdf) — Application Note ประกอบการออกแบบ
- [Datasheets/Ch 17.Input Filter Problem.pdf](Datasheets/Ch%2017.Input%20Filter%20Problem.pdf) — อ้างอิงการออกแบบ EMI Filter
- [Images/](Images/) — รูปภาพวงจร, บอร์ดต้นแบบ, และ BOM
