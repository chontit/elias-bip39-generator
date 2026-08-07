# Elias Extractor -> BIP39 Seed Generator

![Offline](https://img.shields.io/badge/Status-100%25%20Offline-success?style=for-the-badge)
![Zero Dependencies](https://img.shields.io/badge/Dependencies-Zero-blue?style=for-the-badge)
![Combinatorics](https://img.shields.io/badge/Entropy-Combinatorics-critical?style=for-the-badge)
![License](https://img.shields.io/badge/License-Open%20Source-green?style=for-the-badge)

**🌐 Live Demo / ลองใช้งานออนไลน์:** [Elias-Extractor-BIP39.html](https://chontit.github.io/elias-bip39-generator/Elias-Extractor-BIP39.html)
*(คำเตือน: เวอร์ชันออนไลน์มีไว้เพื่อการทดสอบ UI และการทำงานเท่านั้น ห้ามใช้สร้าง Seed สำหรับเก็บเงินจริงเด็ดขาด)*

**🔒 SHA256 Hash of `Elias-Extractor-BIP39.html`:**
```text
3ACF616301423B0B37EE766E27CB498E2E0CE41EE716842A73E1A4FBEDEECA8C
```

---
## 🇹🇭 ส่วนภาษาไทย (Thai Version)
---

เครื่องมือแปลงเอนโทรปีทางกายภาพ (Physical Entropy) เช่น การทอยลูกเต๋า หรือการโยนเหรียญ ให้กลายเป็นรหัส **Seed Phrase (BIP-39)** ระดับ Cryptographic-Grade โปรเจกต์นี้ยึดหลักการ **"Don't Trust, Verify"** อย่างเคร่งครัด โดยใช้คณิตศาสตร์เชิงการจัด (Combinatorics) สกัดเฉพาะความสุ่มที่บริสุทธิ์ (100% Uniform) และกำจัดความเอนเอียง (Bias) ออกอย่างสมบูรณ์แบบโดยไม่ต้องพึ่งพาฟังก์ชัน Hash ในกระบวนการสร้างเอนโทรปี

### 🧠 อัลกอริทึมที่ใช้งานจริงทั้งหมด (Algorithms Under the Hood)

1.  **Elias Extractor (Combinatorial Number System):**
    *   เปลี่ยนลำดับการทอยเต๋าที่มีอคติให้เป็น "อันดับการเรียงลำดับ" (Rank) 
    *   ภายใต้สมมติฐานที่ว่าการทอยแต่ละครั้งเป็นอิสระต่อกัน (i.i.d.) อัลกอริทึมจะใช้คณิตศาสตร์ BigInt คำนวณสัมประสิทธิ์พหุนาม (Multinomial Coefficient) เพื่อสกัดบิตที่ปราศจากอคติ
2.  **System Entropy Salt (Defense-in-Depth):**
    *   **ฟังก์ชันทางเลือก (Opt-in):** รองรับการผสมความสุ่มจากเครื่อง (XOR) เข้ากับความสุ่มจากลูกเต๋า ป้องกันกรณีที่ผู้โจมตีแอบจดบันทึกลำดับการทอยเต๋าของคุณ
    *   ใช้ `crypto.getRandomValues()` (OS CSPRNG) เป็นแหล่งหลัก ทำการ XOR ร่วมกับ Entropy Pool ที่รวบรวมจาก Hardware Interrupts (การขยับเมาส์, การแตะหน้าจอ, จังหวะการกดคีย์บอร์ด, และระยะเวลา `performance.now()`)
3.  **SHA-256 (Pure JavaScript Implementation):**
    *   ใช้สำหรับคำนวณ **BIP-39 Checksum เท่านั้น** ไม่ได้ใช้สร้างเอนโทรปี
    *   เขียนขึ้นมาประมวลผลเองในระดับ Bitwise โดยไม่พึ่งพา Web Crypto API (`crypto.subtle`) เพื่อให้โค้ดทำงานได้แม้ในเบราว์เซอร์รุ่นเก่าหรือ Tor Browser ที่ตั้งค่า Security Level ขั้นสูงสุด
4.  **Strict BIP-39 Standard:**
    *   ฝัง Wordlist มาตรฐาน 2048 คำ พร้อมระบบตรวจสอบความสมบูรณ์แบบรัดกุมหลายชั้น (ตรวจการเรียง A-Z, ห้ามมีคำซ้ำ, และตรวจสอบ SHA-256 Hash ของชุดคำทั้งก้อน)
5.  **ไม่มีการเชื่อมต่อภายนอก:** เป็นไฟล์ HTML ไฟล์เดียวจบ ไม่มีการดึงไฟล์ CSS, JS, Font หรือไลบรารีจากภายนอก

### 🛡️ จุดเด่นและระดับความปลอดภัยขั้นสูงสุด (Paranoia-Level Security)

*   **100% Air-Gapped & Single File:** ปลอดภัยสูงสุดเมื่อใช้งานในสภาพแวดล้อมที่ไม่มีอินเทอร์เน็ต
*   **Zero Storage Footprint:** ไม่ใช้ `localStorage`, `sessionStorage`, หรือ `cookie` ข้อมูลทั้งหมดเก็บอยู่ใน RAM และเมื่อปิดเครื่องหรือแท็บ ข้อมูลจะหายไปทันที
*   **Privacy Mode (กันแอบมอง):** ฟีเจอร์เบลอข้อมูลลับ (Seed, Hex, Binary) โดยอัตโนมัติ ข้อมูลจะแสดงเมื่อนำเมาส์ไปชี้หรือแตะค้างไว้เท่านั้น ป้องกันการถูกบันทึกภาพจากกล้องวงจรปิดหรือคนมองข้ามไหล่
*   **i.i.d. Health Monitor:** ระบบเฝ้าระวังพฤติกรรมการทอยเต๋าแบบเรียลไทม์ ตรวจจับการเกิดรูปแบบซ้ำ (Serial Dependence) ด้วยสถิติ Lag-1 Autocorrelation, Max Repeat Run, Adaptive Proportion Test และ Wald-Wolfowitz Runs Test
*   **Built-in Self-Test (KAT):** เมื่อเปิดไฟล์ ระบบจะรันทดสอบตัวเองอัตโนมัติเพื่อยืนยันความถูกต้องของอัลกอริทึม Elias, SHA-256, และ BIP-39 Test Vectors หากมีข้อผิดพลาดระบบจะล็อกการทำงานทันที

### 📖 คู่มือการใช้งาน (Operational Security)

**คำเตือน: ห้ามใช้ไฟล์ที่เปิดบนอินเทอร์เน็ตในการสร้าง Seed Phrase สำหรับเก็บเงินจริงเด็ดขาด**

1.  **ดาวน์โหลด:** โหลดไฟล์ `Elias-Extractor-BIP39.html` ลงใน USB Drive 
2.  **เตรียมสภาพแวดล้อม (Air-gapped):** บูตคอมพิวเตอร์ด้วย **Tails OS** แบบออฟไลน์ (ปิดสวิตช์ Wi-Fi/ถอดสาย LAN ออกทั้งหมด)
3.  **เปิดโปรแกรม:** เปิดไฟล์บน Tor Browser ของ Tails (แนะนำตั้ง Security Level: Safest)
4.  **ทอยและป้อนข้อมูล:** เลือกประเภทอุปกรณ์ (แนะนำ HEX Dice) ทอยลูกเต๋าแล้วป้อนข้อมูลทีละตัวจนกว่าความคืบหน้าจะครบ 100% (256 bits = 24 words)
5.  **จดบันทึก:** จด Seed Phrase ทั้ง 12 หรือ 24 คำลงบนกระดาษ หรือตอกลงบนแผ่นเหล็ก (หลีกเลี่ยงการใช้ปุ่มคัดลอกลง Clipboard หากไม่จำเป็น)
6.  **ทำลายหลักฐาน:** ปิดเบราว์เซอร์ และทำการ Shutdown เครื่อง Tails OS (ระบบจะล้างข้อมูลใน RAM ทิ้งทั้งหมด ไม่เหลือร่องรอย)

### 🔍 การตรวจสอบและข้อสงวนสิทธิ์

ผู้ใช้สามารถตรวจสอบความถูกต้องของ Wordlist มาตรฐาน (ครบ 2048 คำ) และกระบวนการแปลงรหัสได้ด้วยตนเองผ่าน UI ที่แสดงให้เห็นการแมปจาก 11-bit ไปยัง Index อย่างโปร่งใส

**ข้อสงวนสิทธิ์:** ซอฟต์แวร์นี้ให้ใช้งานในลักษณะ "ตามสภาพที่เป็นอยู่" (AS IS) ผู้ใช้งานต้องรับผิดชอบต่อความปลอดภัยของสภาพแวดล้อมคอมพิวเตอร์ (OpSec) และการเก็บรักษา Seed Phrase ด้วยตนเอง

<br>

---
## 🇬🇧 English Section (English Version)
---

A physical entropy extraction tool that converts biased dice rolls or coin flips into a Cryptographic-Grade **BIP-39 Seed Phrase**. Strictly adhering to the **"Don't Trust, Verify"** philosophy, this project uses pure combinatorics to extract 100% uniform randomness, eliminating physical bias completely without relying on any cryptographic hashes in the entropy generation path.

### 🧠 Algorithms Under the Hood

1.  **Elias Extractor (Combinatorial Number System):**
    *   Translates biased physical rolls into a "Rank" within their specific sequence configuration.
    *   Assuming the rolls are independent and identically distributed (i.i.d.), it uses BigInt combinatorics to calculate multinomial coefficients, perfectly peeling away unbiased bits from the sequence rank.
2.  **System Entropy Salt (Defense-in-Depth):**
    *   **Opt-in Feature:** Allows XOR-ing hardware-derived randomness with your dice rolls. This prevents your seed from being perfectly reproducible if an attacker records your physical dice sequence.
    *   Combines the OS CSPRNG (`crypto.getRandomValues()`) with an entropy pool constantly gathered from hardware interrupts (mouse movements, touch events, keyboard strokes, and `performance.now()` micro-timings).
3.  **SHA-256 (Pure JavaScript Implementation):**
    *   Used **exclusively for the BIP-39 Checksum**. It is never used to generate or stretch entropy.
    *   Implemented entirely in bitwise JS to ensure the file runs perfectly on restrictive browsers or Tor Browser's "Safest" security level without needing the `crypto.subtle` Web API.
4.  **Strict BIP-39 Standard:**
    *   Embeds the official 2048 wordlist with multi-layered integrity checks, including strict A-Z sorting, duplication blocks, and a full-blob SHA-256 validation to prevent malicious word substitutions.
5.  **Zero External Dependencies:** Designed as a single HTML file. It has no network connectivity and does not fetch external CDNs, fonts, or libraries.

### 🛡️ Paranoia-Level Security Features

*   **100% Air-Gapped & Single File:** Everything is bundled into one HTML file. Completely network-isolated.
*   **Zero Storage Footprint:** Completely avoids `localStorage`, `sessionStorage`, and `cookies`. State exists only in RAM, featuring an in-place overwrite wipe mechanism.
*   **Privacy Mode (Anti-Shoulder Surfing):** Sensitive data (Seed, Hex, Binary) is heavily blurred by default and only revealed on hover or press-and-hold, protecting against CCTV and nearby observers.
*   **i.i.d. Health Monitor:** A real-time diagnostic engine that scrutinizes your physical rolls for dangerous patterns or serial dependence, utilizing Lag-1 Autocorrelation, Max Repeat Run, Adaptive Proportion Test, and Wald-Wolfowitz stats.
*   **Built-in Self-Test (KAT):** On boot, the system autonomously executes Known Answer Tests (KAT) for the Elias algorithm, SHA-256 vectors, and official BIP-39 vectors. Any failure instantly locks the application.

### 📖 Usage Instructions (OpSec)

**WARNING: NEVER use the online/hosted version to generate a seed phrase for real funds.**

1.  **Download:** Save `Elias-Extractor-BIP39.html` to a clean USB Drive from the Releases page.
2.  **Air-gapped Environment:** Boot a computer using **Tails OS** (or a clean, trusted live Linux distribution). **Disconnect entirely from the internet** (unplug LAN, disable Wi-Fi).
3.  **Run:** Open the HTML file using the Tor Browser (set Security Level to Safest).
4.  **Generate:** Select your entropy source (HEX Dice recommended). Roll physically and input the results until the progress bar reaches 100% (256 bits = 24 words).
5.  **Record:** Write your 12 or 24-word Seed Phrase onto paper or punch it into a metal plate (avoid using the clipboard copy button if possible).
6.  **Destroy Traces:** Close the browser and completely shut down the Tails OS machine (this triggers an amnesic wipe, clearing all system RAM).

### 🔍 Verification & Disclaimer

Users can manually verify the integrity of the strict 2048 standard wordlist and the conversion process. The BIP-39 control panel transparently displays the step-by-step mapping from the 11-bit binary chunks directly to the wordlist index.

**Disclaimer:** This software is provided "AS IS". The user is entirely responsible for maintaining the physical security of their computer environment (OpSec) and the safe storage of their Seed Phrase.

---
*“Don't Trust, Verify.”* — **Chollatis Bitcoiner.**
