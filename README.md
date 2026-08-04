# Elias Extractor -> BIP39 Seed Generator

![Offline](https://img.shields.io/badge/Status-100%25%20Offline-success?style=for-the-badge)
![Zero Dependencies](https://img.shields.io/badge/Dependencies-Zero-blue?style=for-the-badge)
![Math](https://img.shields.io/badge/Entropy-Combinatorics-critical?style=for-the-badge)

---
## 🇹🇭 ส่วนภาษาไทย (Thai Version)
---

เครื่องมือแปลงเอนโทรปีทางกายภาพ (Physical Entropy) เช่น การทอยลูกเต๋า หรือการโยนเหรียญ ให้กลายเป็นรหัส Seed Phrase (BIP-39) ระดับ Cryptographic-Grade โดยใช้คณิตศาสตร์ **Elias Extractor** ในการสกัดบิตที่บริสุทธิ์ 100% และกำจัดความเอนเอียง (Bias) ออกอย่างสมบูรณ์

### สถาปัตยกรรมและปรัชญาการออกแบบ

โปรเจกต์นี้ยึดหลักการ **"Don't Trust, Verify"** อย่างเคร่งครัด
1. **ความบริสุทธิ์ทางคณิตศาสตร์:** ใช้คณิตศาสตร์เชิงการจัด (Combinatorics / BigInt) สกัดเฉพาะความสุ่มที่บริสุทธิ์ (Uniform 100%) อัลกอริทึมนี้ไม่พึ่งพา Cryptographic Hash ในการสร้าง Entropy ทำให้สามารถพิสูจน์ความสุ่มได้ทางคณิตศาสตร์
2. **ไม่มีการเชื่อมต่อภายนอก:** เป็นไฟล์ HTML ไฟล์เดียว (Single-file HTML) ไม่มีการเชื่อมต่อเครือข่าย ไม่เรียกใช้ CDN, Font หรือไลบรารีภายนอก 
3. **Pure-JS Checksum:** ไม่พึ่งพา API ของเบราว์เซอร์ (`crypto.subtle`) โดยเขียนอัลกอริทึม SHA-256 ขึ้นมาประมวลผลเองในระดับ Bitwise เพื่อรองรับการทำงานบน Tor Browser (High Security) หรือเบราว์เซอร์รุ่นเก่าที่ถูกจำกัดสิทธิ์
4. **ระบบเฝ้าระวัง:** มีระบบ `i.i.d. Health Monitor` คอยตรวจสอบสมมติฐานความอิสระของการทอย (Serial Dependence) ป้องกัน Human Error ในระหว่างการสร้างข้อมูล

### ข้อดี และ ข้อจำกัด

**ข้อดี**
*   ปราศจากอคติ (Bias) อย่างสิ้นเชิง ปลอดภัยจากการคาดเดาหรือวิศวกรรมย้อนกลับ (Reverse Engineer)
*   ออกแบบมาเพื่อรันบนสภาพแวดล้อมปิด (Tails OS) เมื่อปิดเครื่อง ข้อมูลใน RAM จะหายไปทันที ไม่ทิ้งร่องรอย
*   ให้ความน่าเชื่อถือทางคณิตศาสตร์เทียบเท่าหรือสูงกว่าชิป Secure Element ทั่วไป 
*   มีระบบ Self-Test ในตัว เพื่อทำ Known Answer Tests (KAT) ทันทีที่เปิดไฟล์ 

**ข้อจำกัดและข้อควรระวัง**
*   ความปลอดภัยทางคณิตศาสตร์จะไร้ความหมาย หากผู้ใช้งานละเลยความปลอดภัยทางกายภาพ (เช่น ถูกมองผ่านกล้อง, ถ่ายรูปหน้าจอ, หรือรันบนเครื่องที่มีมัลแวร์)
*   ต้องใช้เวลาและความพยายามในการทอยลูกเต๋าแบบ Manual ให้ครบตามจำนวนบิตที่ต้องการ
*   ผู้ใช้ต้องเขย่า/ทอยลูกเต๋าให้เกิดความเป็นอิสระต่อกัน (i.i.d.) อย่างแท้จริง หากจงใจเรียงเลข ระบบจะแจ้งเตือนและผลลัพธ์อาจไม่ปลอดภัย

### คู่มือการใช้งาน

**คำเตือน: ห้ามใช้ไฟล์ที่เปิดบนอินเทอร์เน็ตในการสร้าง Seed Phrase สำหรับเก็บเงินจริงเด็ดขาด**

1.  **ดาวน์โหลด:** โหลดไฟล์ `Elias-Extractor-BIP39.html` จากเมนู Releases ลงใน USB Drive
2.  **เตรียมสภาพแวดล้อม (Air-gapped):** บูตคอมพิวเตอร์ด้วย **Tails OS** (หรือ OS ที่มั่นใจว่าไม่มีมัลแวร์) โดย **ไม่ต้องเชื่อมต่ออินเทอร์เน็ต**
3.  **เปิดโปรแกรม:** ดับเบิลคลิกไฟล์ HTML เพื่อเปิดบน Tor Browser (ตั้งค่า Security Level: Safest)
4.  **ทอยและป้อนข้อมูล:** เลือกประเภทอุปกรณ์ (แนะนำ HEX Dice) ทอยลูกเต๋าและป้อนข้อมูลทีละตัวจนกว่าหลอด Entropy จะครบ 100% (256 bits = 24 words)
5.  **จดบันทึก:** จด Seed Phrase ลงบนกระดาษ หรือตอกลงบนแผ่นเหล็ก
6.  **ทำลายหลักฐาน:** ปิดเบราว์เซอร์ และทำการ Shutdown เครื่อง Tails OS (ข้อมูลใน RAM จะถูกทำลายทั้งหมด)

### การตรวจสอบ
ผู้ใช้สามารถตรวจสอบความถูกต้องของ Wordlist มาตรฐาน (ครบถ้วน 2048 คำ โดยไม่มีคำแปลกปลอมนอกเหนือมาตรฐาน) และกระบวนการแปลงรหัสได้ด้วยตนเองผ่าน UI แผงควบคุมขั้นตอนการคำนวณ BIP-39 ที่แสดงให้เห็นการแมปจาก 11-bit ไปยัง Index อย่างโปร่งใส

**ข้อสงวนสิทธิ์:** ซอฟต์แวร์นี้ให้ใช้งานในลักษณะ "ตามสภาพที่เป็นอยู่" (AS IS) ผู้ใช้งานต้องรับผิดชอบต่อความปลอดภัยของสภาพแวดล้อมคอมพิวเตอร์ (OpSec) และการเก็บรักษา Seed Phrase ด้วยตนเอง

<br>

---
## 🇬🇧 English Section (English Version)
---

A tool that converts physical entropy (such as rolling dice or flipping coins) into a Cryptographic-Grade BIP-39 Seed Phrase. It utilizes the **Elias Extractor** mathematical algorithm to extract 100% pure, unbiased bits, completely eliminating physical bias.

### Architecture & Design Philosophy

This project strictly adheres to the **"Don't Trust, Verify"** ethos.
1. **Mathematical Purity:** Uses combinatorics (BigInt) to extract only pure randomness (100% Uniform). This algorithm does not rely on cryptographic hashing to generate entropy, meaning the randomness is mathematically provable.
2. **Zero External Dependencies:** Designed as a single HTML file. It has no network connectivity and does not fetch external CDNs, fonts, or libraries.
3. **Pure-JS Checksum:** Avoids relying on browser APIs (`crypto.subtle`). The SHA-256 algorithm is implemented entirely in bitwise JavaScript, ensuring compatibility with the Tor Browser (High Security level) or highly restrictive environments.
4. **Diagnostic Safeguards:** Includes an `i.i.d. Health Monitor` to observe the serial dependence of your rolls, safeguarding against human error during manual entropy generation.

### Strengths & Limitations

**Pros**
*   Completely free from bias. Secure against prediction or reverse-engineering attempts.
*   Air-gapped ready. Built to run on live operating systems like Tails OS; when the machine shuts down, all RAM is cleared leaving zero traces.
*   Provides mathematical trust equivalent to or exceeding standard Secure Element chips.
*   Built-in Self-Test module executes Known Answer Tests (KAT) the moment the file is opened.

**Cons & Risks**
*   The math is meaningless if Operational Security (OpSec) is ignored (e.g., shoulder-surfing via cameras, taking screenshots, or running on malware-infected machines).
*   Time-consuming. Generating adequate entropy requires manually rolling physical dice many times.
*   The user must ensure rolls are independent and identically distributed (i.i.d.). Intentional patterns will trigger the health monitor, and the resulting seed may be compromised.

### Usage Instructions

**WARNING: NEVER use the online/hosted version of this file to generate a seed phrase for real funds.**

1.  **Download:** Download the `Elias-Extractor-BIP39.html` file from the Releases page onto a USB drive.
2.  **Air-gapped Environment:** Boot a computer using **Tails OS** (or any clean, trusted OS). Ensure the machine is **completely disconnected from the internet**.
3.  **Run:** Open the HTML file in the Tor Browser (set Security Level to: Safest).
4.  **Generate:** Select your entropy source (HEX Dice recommended). Roll your dice and input the results until the Entropy bar reaches 100% (256 bits = 24 words).
5.  **Record:** Write your Seed Phrase on paper or stamp it into metal.
6.  **Destroy Traces:** Close the browser and shutdown Tails OS immediately (this completely wipes the system RAM).

### Verification
Users can manually verify the integrity of the strict 2048 standard wordlist and the conversion process. The BIP-39 control panel transparently displays the step-by-step mapping from the 11-bit binary chunks directly to the wordlist index.

**Disclaimer:** This software is provided "AS IS". The user is entirely responsible for maintaining the physical security of their computer environment (OpSec) and the safe storage of their Seed Phrase.
