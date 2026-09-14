# Elias Extractor -> BIP39 Seed Generator

![Version](https://img.shields.io/badge/Version-3.1.1-blueviolet?style=for-the-badge)
![Offline](https://img.shields.io/badge/Status-100%25%20Offline-success?style=for-the-badge)
![Zero Dependencies](https://img.shields.io/badge/Dependencies-Zero-blue?style=for-the-badge)
![Combinatorics](https://img.shields.io/badge/Entropy-Combinatorics-critical?style=for-the-badge)
![License](https://img.shields.io/badge/License-Open%20Source-green?style=for-the-badge)

**🌐 Live Demo / ลองใช้งานออนไลน์:** [Elias-Extractor-BIP39.html](https://chontit.github.io/elias-bip39-generator/Elias-Extractor-BIP39.html)
*(คำเตือน: เวอร์ชันออนไลน์มีไว้เพื่อการทดสอบ UI และการทำงานเท่านั้น ห้ามใช้สร้าง Seed สำหรับเก็บเงินจริงเด็ดขาด)*

**🔒 SHA256 Hash of `Elias-Extractor-BIP39.html` (v3.1.1):**
```text
C472245B4EE9606299C3B1188C131489FFE3AE00E488CD5773C6A65011509018
```
**🔑 OpenPGP Signing Key / กุญแจสำหรับตรวจลายเซ็น Release:**
```text
Key:         ed25519 — Chollatis Maneewong - Bitcoiner <chon_tit@hotmail.com>
Fingerprint: EEFC F3F0 928D 0199 BA7E 56EC 2DB5 4085 AB23 3A47
Valid until: 2028-09-13
Key file:    chollatis-bitcoiner-pubkey.asc (in this repo & attached to every Release)
```
ทุก Release มีไฟล์ลายเซ็น `.asc` แนบคู่เสมอ — วิธีตรวจ / How to verify:
```bash
gpg --import chollatis-bitcoiner-pubkey.asc
gpg --verify Elias-Extractor-BIP39.html.asc Elias-Extractor-BIP39.html
gpg --fingerprint "Chollatis"   # ต้องตรงกับ fingerprint ด้านบนทุกตัวอักษร
```
*อย่าเชื่อ fingerprint จากแหล่งเดียว (รวมถึงหน้านี้) — เทียบจากหลายช่องทางอิสระ: repo นี้, learning.chontit.win, และประกาศของชุมชน / Never trust a single channel for the fingerprint — cross-check it across this repo, learning.chontit.win, and community announcements.*

---
## 🆕 มีอะไรใหม่ใน v3.1.1 (Calibration & Claim-Accuracy Patch)

ต่อยอดจาก v3.1.0 หลัง **external audit รอบที่ 3** — ทุกข้อวัดผลเชิงปริมาณก่อนแก้:

| # | การเปลี่ยนแปลง | ผลที่วัดได้ |
|---|---|---|
| A | **Autocorrelation threshold ปรับตามขนาดตัวอย่าง** — เดิมใช้ค่าคงที่ \|ρ\|>0.2 ซึ่งที่ N=96 เท่ากับเพียง ~2σ ใหม่: RED \|ρ\|>max(0.20, 3.5/√N) · YELLOW >max(0.12, 2.5/√N) | **RED false-positive 4.80% → 0.09%** · YELLOW 38.5% → 2.4% · detection power คงเดิม (sticky-dice 50% → RED 100%, sequential pattern → RED 100%) |
| B | **แก้ claim ให้ตรงความจริง** — เลิกอ้าง "restores exact uniformity 100%" เปลี่ยนเป็น "fixed batch กำจัด stopping-time selection ออกจากโปรโตคอล · exact uniformity เป็นจริงภายใต้สมมติฐาน i.i.d. ที่ประกาศไว้" พร้อมเปิดเผยผลของ gate conditioning ตรงๆ ใน UI | ขอบเขตบนที่**พิสูจน์ได้** log₂(1/P(accept)) = **0.0013 บิต จาก 256** (v3.1.0 = 0.071) เทียบกับความเสียหายระดับหลายสิบบิตจาก source ที่ correlated จริง |
| C | **System Entropy: HARD FAIL เมื่อไม่มี CSPRNG** — ไม่ fallback ไป mouse/timing pool อีกต่อไป และเลิกใช้คำว่า "salt" เป็นชื่อหลัก (ชนกับ BIP39 passphrase salt ใน PBKDF2) | ปิดช่องที่ผู้ใช้อาจได้ entropy ที่ประเมินค่าไม่ได้โดยไม่รู้ตัว |
| D | **นิยาม "bias" ของตาราง BATCH ให้ชัด** — p_max ≤ 1.1/n และระบุว่าเป็น simulation bound ไม่ใช่ proof | ผู้ตรวจสอบทราบขอบเขตของ claim ตรงไปตรงมา |

**หมายเหตุเชิงทฤษฎีที่สำคัญ (ค้นพบระหว่าง audit รอบนี้):** APT และ chi-square เป็นฟังก์ชันของ **type (count vector) ล้วน** — ทฤษฎีบทของ Elias คือ "conditioned on type, rank uniform" ดังนั้นการ gate ด้วยสองตัวนี้**ไม่ทำลาย uniformity เลยแม้แต่นิดเดียว** (เหตุผลเดียวกับที่การทิ้ง batch เพราะบิตไม่ถึงเป้าปลอดภัย) มีเพียง RCT/autocorr/runs ที่ขึ้นกับ**ลำดับ** จึงก่อ conditioning ซึ่งถูก bound ไว้ที่ 0.0013 บิตข้างต้น

**เหตุผลที่ยังคง hard gate ไว้ (ไม่เปลี่ยนเป็น advisory):** การเปลี่ยนเป็นคำเตือนแล้วให้ผู้ใช้ตัดสินใจทิ้ง batch เอง **ไม่ลด conditioning เลยแม้แต่นิดเดียว** — ถ้าผู้ใช้ทำตามคำแนะนำ การกระจายถูก condition เหมือนกันเป๊ะ ต่างแค่ผู้ตัดสินใจ การกำจัด conditioning จริงต้องยอมใช้ batch แรกเสมอแม้ตรวจพบ correlation ซึ่งแลก 0.0013 บิต กับความเสี่ยงหลายสิบบิต — เป็นการแลกที่ขาดทุนชัดเจน ทางที่ถูกคือ**คง gate ไว้ ลด false positive ให้ต่ำที่สุด และประกาศ bound อย่างโปร่งใส** ซึ่งคือสิ่งที่ v3.1.1 ทำ

---

## v3.1.0 (Security-Hardening Release) — สรุปย่อ
---

เวอร์ชันนี้เกิดจาก **การ audit ภายนอกอิสระ 2 รอบ** ทุกประเด็นถูกพิสูจน์/วัดขนาดก่อนแก้ (ไม่แก้ตามความรู้สึก) — รายละเอียดเต็มใน Release Notes

| # | การเปลี่ยนแปลง | เหตุผล |
|---|---|---|
| 1 | **Fixed-Batch Protocol** — จำนวนทอยถูกล็อกล่วงหน้า (N) ต่อชนิดลูกเต๋า/เป้าหมาย ห้ามหยุดก่อน–ทอยเกินไม่ได้ | ปิดช่องโหว่เชิงทฤษฎี *stopping-time bias*: การ "ทอยจนบิตถึงเป้าแล้วหยุด" ทำให้เงื่อนไขการหยุดพัวพันกับข้อมูลที่จะเป็น seed (พิสูจน์ด้วย exact enumeration: เหรียญแฟร์ target 1 บิต ให้ 0.596/0.404) การล็อก N ก่อนทอยลูกแรกคืนเงื่อนไขการพิสูจน์ uniformity แบบเป๊ะ 100% |
| 2 | **Hard i.i.d. Gate** — ไฟแดง = บล็อกการสร้าง seed จริง (ต้องทิ้ง batch), ไฟเหลือง = ต้องติ๊กยืนยัน | เดิม monitor เป็นเพียงคำเตือน ผู้ใช้สร้าง seed ต่อได้แม้ไฟแดง (control flaw) |
| 3 | **ตัดปุ่มคัดลอก Clipboard ออกทั้งหมด** — บังคับจดด้วยมือ | ลดจำนวนสำเนา secret ใน memory/clipboard/DOM |
| 4 | **ซ่อนค่าบิตจนกว่า batch จะครบ** | ตัดช่องทาง selection bias (เลือกหยุด/เริ่มใหม่ตามค่าที่เห็น) |
| 5 | **External Audit Vector ฝังใน self-test** | ค่าคาดหวังตายตัว ตรวจข้ามกับ implementation อิสระคนละภาษาได้ (Don't Trust, Verify ของจริง) |
| 6 | แก้ test distribution ให้รวม = 1.00 พอดี · ปรับ wording สถิติเป็น "No anomaly detected" | ความสะอาดของ test suite + ไม่กล่าวอ้างเกินหลักฐาน (finite test พิสูจน์ i.i.d. ไม่ได้) |

**ตารางแผนการทอยที่ล็อกไว้ (Pre-committed N):**

| แหล่ง | 256 บิต (24 คำ) | 128 บิต (12 คำ) |
|---|---|---|
| HEX Dice (d16) | **96 ครั้ง** (= 32 โยน × 3 ลูก) | **51 ครั้ง** (= 17 โยน × 3 ลูก) |
| D6 | **132 ครั้ง** | **68 ครั้ง** |
| Coin | **336 ครั้ง** | **172 ครั้ง** |

*ค่า N ทุกตัวผ่าน simulation หลายพันรอบ: โอกาสที่บิตไม่ถึงเป้า ≈ 0 แม้ลูกเต๋าเบี้ยว ~10% — ถ้า batch ใดสกัดบิตไม่ถึงเป้า (ลูกเต๋าเบี้ยวหนักมาก) กติกาคือ**ทิ้งทั้งชุดแล้วเริ่มใหม่** ห้ามต่อเติม*

---
## 🇹🇭 ส่วนภาษาไทย (Thai Version)
---

เครื่องมือแปลงเอนโทรปีทางกายภาพ (Physical Entropy) เช่น การทอยลูกเต๋า หรือการโยนเหรียญ ให้กลายเป็นรหัส **Seed Phrase (BIP-39)** ระดับ Cryptographic-Grade โปรเจกต์นี้ยึดหลักการ **"Don't Trust, Verify"** อย่างเคร่งครัด โดยใช้คณิตศาสตร์เชิงการจัด (Combinatorics) สกัดเฉพาะความสุ่มที่บริสุทธิ์ และกำจัดความเอนเอียง (Bias) ของลูกเต๋าออกอย่างสมบูรณ์โดยไม่พึ่งพาฟังก์ชัน Hash ในเส้นทางสร้างเอนโทรปี — ตั้งแต่ v3.1.0 การรับประกัน uniformity แบบเป๊ะมีผลภายใต้เงื่อนไขครบถ้วน: **การทอยเป็นอิสระต่อกัน (i.i.d.) + ทอยครบตามแผน N ที่ล็อกล่วงหน้า**

### 🧠 อัลกอริทึมที่ใช้งานจริงทั้งหมด (Algorithms Under the Hood)

1.  **Elias Extractor (Combinatorial Number System):**
    *   เปลี่ยนลำดับการทอยเต๋าที่มีอคติให้เป็น "อันดับการเรียงลำดับ" (Rank) ภายใน type class
    *   ภายใต้ i.i.d. อัลกอริทึมใช้ BigInt คำนวณสัมประสิทธิ์พหุนาม (Multinomial Coefficient) เพื่อสกัดบิตที่ปราศจากอคติ — bias ของลูกเต๋าถูกจ่ายเป็น "จำนวนทอยที่มากขึ้น" ไม่ใช่คุณภาพบิตที่ลดลง
2.  **Fixed-Batch Protocol (ใหม่ใน v3.1.0):**
    *   จำนวนทอย N ถูกกำหนด**ก่อน**ลูกเต๋าลูกแรกตกพื้น — การตัดสินใจหยุดจึงเป็นค่าคงที่ ไม่พัวพันกับข้อมูลสุ่ม ทำให้ทฤษฎีบทของ Elias คุ้มครองแบบไม่มีดอกจัน
3.  **System Entropy Salt (Defense-in-Depth, Opt-in):**
    *   ผสมความสุ่มจากเครื่อง (XOR) เข้ากับลูกเต๋า — กัน**การทำซ้ำ seed จาก log การทอย/keylogger** เท่านั้น (ไม่กันกล้อง/ผู้เห็นหน้าจอ ซึ่ง seed แสดงบนจออยู่แล้ว)
    *   เชิงคณิตศาสตร์: XOR ต้องการเพียง "แหล่งใดแหล่งหนึ่ง" ที่ uniform และอิสระ — ผลลัพธ์แข็งอย่างน้อยเท่าแหล่งที่ดีที่สุดเสมอ
4.  **SHA-256 (Pure JavaScript):** ใช้เฉพาะ **BIP-39 Checksum** ไม่แตะเส้นทางสร้างเอนโทรปี — ทำงานได้แม้บน Tor Browser ระดับ Safest (ไม่พึ่ง `crypto.subtle`)
5.  **Strict BIP-39 Standard:** Wordlist 2048 คำ + ตรวจ 5 ชั้น (จำนวน/ไม่ซ้ำ/เรียง A→Z/รูปแบบอักษร/SHA-256 ทั้งก้อน)
6.  **ไม่มีการเชื่อมต่อภายนอก:** ไฟล์เดียวจบ ไม่มี fetch/CDN/font/library ภายนอก และไม่ใช้ storage ใดๆ

### 🛡️ จุดเด่นด้านความปลอดภัย (Paranoia-Level Security)

*   **100% Air-Gapped & Single File** — ออกแบบสำหรับ Tails OS แบบตัดเน็ตถาวร
*   **Hard i.i.d. Gate (v3.1.0)** — RCT · Adaptive Proportion · Lag-1 Autocorrelation · Runs Test: ไฟแดง = **ระบบปฏิเสธการสร้าง seed** จนกว่าจะทิ้ง batch และทอยชุดใหม่
*   **Zero Clipboard (v3.1.0)** — ไม่มีปุ่มคัดลอก: อ่านทีละคำ จดลงกระดาษ/แผ่นโลหะ อ่านทวนกลับ
*   **Bit Concealment (v3.1.0)** — ค่าบิตถูกซ่อนจนกว่าจะทอยครบแผน ตัด selection bias
*   **Zero Storage Footprint** — ทุกอย่างอยู่ใน RAM · ปุ่ม WIPE เขียนทับ buffer + รีสตาร์ต Tails เพื่อล้าง RAM จริง
*   **Privacy Mode** — เบลอข้อมูลลับ กันมองข้ามไหล่/กล้อง (เป็น shoulder-surfing reduction ไม่ใช่ memory protection)
*   **Built-in Self-Test (KAT)** — Elias round-trip + uniformity, SHA-256 KAT, BIP-39 official vectors, i.i.d. monitor, และ **External Audit Vector**

### 🔍 External Audit Vector — ตรวจข้าม Implementation ด้วยตัวเอง

ป้อนลำดับนี้ให้ implementation อิสระใดๆ (Python/Rust/C — ห้ามแชร์โค้ดกับไฟล์นี้) แล้วผลต้องตรงทุกบิต:

```text
Input (d16, k=96):  "0123456789ABCDEF" ซ้ำ 6 รอบ
Extracted length:    346 bits
First 256 bits hex:  0006542aa4520bad7ce59b3e19ea1f9ca9c7aa8b7c55ba9d4b19861ef035913d
BIP-39 (24 words):   abandon crawl apple emerge camera stove vicious recall dignity soon
                     margin deer organ stem combine melt ritual tumble shoe around
                     upper bracket eager put
```
*(หมายเหตุ: ลำดับนี้เป็น pattern สมบูรณ์แบบโดยเจตนาเพื่อให้คำนวณซ้ำง่าย — หากป้อนผ่าน UI จริง Hard Gate จะบล็อกทันทีเพราะตรวจพบ autocorrelation ซึ่งเป็นการยืนยันว่า gate ทำงาน · ใช้ตรวจผ่าน self-test ในตัว หรือเรียกฟังก์ชันตรงในคอนโซล)*

### 📖 คู่มือการใช้งาน (Operational Security)

**คำเตือน: ห้ามใช้ไฟล์ที่เปิดบนอินเทอร์เน็ตสร้าง Seed สำหรับเงินจริงเด็ดขาด**

1.  **ดาวน์โหลด + ตรวจ:** โหลดจากหน้า Releases → ตรวจ SHA-256 ให้ตรงด้านบน → ตรวจลายเซ็น OpenPGP (`gpg --verify`)
2.  **เตรียมสภาพแวดล้อม:** บูต **Tails OS** แบบ air-gapped (ตรวจลายเซ็น Tails image ก่อน flash เสมอ)
3.  **เปิดโปรแกรม:** เปิดไฟล์บน Tor Browser (Security Level: Safest ได้)
4.  **ล็อกแผนก่อนทอย:** เลือกชนิดลูกเต๋า + ขนาด seed → ระบบแสดง **N ที่ล็อกไว้** → ทอยจริงให้ครบ N พอดี (ลูกเต๋าหลายลูกให้กำหนดกติกาอ่านลำดับตายตัว เช่น สีแดง→ขาว→ดำ ทุกโยน)
5.  **ผ่าน Gate:** ไฟแดง = ทิ้ง batch ทอยใหม่ · บิตไม่ถึงเป้าใน N = ทิ้ง batch (ห้ามต่อเติม)
6.  **จดด้วยมือ:** อ่านทีละคำ เขียนลงกระดาษ/โลหะ อ่านทวนกลับทุกคำ — แล้ว**ตรวจซ้ำบนอุปกรณ์อิสระ** (นำ entropy hex ไปเทียบ 24 คำบนเครื่องมือคนละตัว แบบ offline) ก่อนใช้จริง
7.  **ทำลายร่องรอย:** ปิดเบราว์เซอร์ + Shutdown Tails (ล้าง RAM)

### 🔍 การตรวจสอบและข้อสงวนสิทธิ์

ผู้ใช้ตรวจสอบ Wordlist, การแมป 11-bit → index → word, และ External Audit Vector ได้ด้วยตนเองทั้งหมดผ่าน UI ที่โปร่งใส

**ข้อสงวนสิทธิ์:** ซอฟต์แวร์นี้ให้ใช้ "ตามสภาพที่เป็นอยู่" (AS IS) ผู้ใช้รับผิดชอบ OpSec ของสภาพแวดล้อมและการเก็บรักษา Seed ด้วยตนเอง

<br>

---
## 🇬🇧 English Section (English Version)
---

A physical-entropy extraction tool that converts biased dice rolls or coin flips into a cryptographic-grade **BIP-39 seed phrase**. Strictly "Don't Trust, Verify": pure combinatorics extracts uniform randomness and eliminates physical bias with **no cryptographic hash in the entropy path**. Since v3.1.0 the exact-uniformity guarantee holds under the complete condition set: **i.i.d. rolls + a pre-committed fixed batch of exactly N rolls**.

### 🧠 Algorithms Under the Hood

1.  **Elias Extractor (Combinatorial Number System):** maps the roll sequence to its rank within its type class; under i.i.d., BigInt multinomial arithmetic peels off provably unbiased bits. Dice bias costs *more rolls*, never bit quality.
2.  **Fixed-Batch Protocol (new in v3.1.0):** the roll count N is committed *before the first die lands*, so the stopping decision is a constant — independent of the randomness that becomes the seed. This closes the *stopping-time bias* found in external audit (proven by exact enumeration: a fair coin with target 1 bit under first-crossing stopping yields 0.596/0.404) and restores the exact-uniformity proof with no asterisks. If a batch extracts fewer bits than the target (grossly unfair dice), **discard the whole batch** — never top-up.
3.  **System Entropy Salt (opt-in, defense-in-depth):** XORs OS CSPRNG randomness into the dice output. Scope: defeats seed reconstruction from a roll log/keylogger only — not cameras/screen observers. Mathematically XOR needs only *one* of the two sources to be uniform and independent; the result is at least as strong as the best source.
4.  **SHA-256 (pure JS):** BIP-39 checksum only — never touches the entropy path. Runs on Tor Browser "Safest" without `crypto.subtle`.
5.  **Strict BIP-39:** embedded 2048-word list with 5-layer integrity checks (count / uniqueness / strict A→Z order / charset / full-blob SHA-256).
6.  **Zero external anything:** one file, no network, no storage.

### 🛡️ Paranoia-Level Security Features

*   **100% Air-Gapped & Single File**
*   **Hard i.i.d. Gate (v3.1.0):** RCT, Adaptive Proportion, Lag-1 Autocorrelation, Runs Test — **RED blocks seed generation** (discard the batch); YELLOW requires explicit confirmation; GREEN means *"no anomaly detected"*, never *"i.i.d. proven"*.
*   **Zero Clipboard (v3.1.0):** the copy button is gone. Hand-copy word by word, then read back.
*   **Bit Concealment (v3.1.0):** extracted bit values stay hidden until the batch completes, removing any selection channel.
*   **Zero Storage Footprint** with an in-place overwrite WIPE; restart Tails to truly clear RAM.
*   **Privacy Mode:** blur-until-hover (shoulder-surfing reduction — not memory protection).
*   **Built-in Self-Test (KAT):** Elias round-trip + uniformity on heavily-biased input, SHA-256 KATs, official BIP-39 vectors, i.i.d. monitor check, and an **External Audit Vector**.

### 🔍 External Audit Vector — verify across implementations

Feed this to any independent implementation (different language, no shared code); every bit must match:

```text
Input (d16, k=96):  "0123456789ABCDEF" repeated 6 times
Extracted length:    346 bits
First 256 bits hex:  0006542aa4520bad7ce59b3e19ea1f9ca9c7aa8b7c55ba9d4b19861ef035913d
BIP-39 (24 words):   abandon crawl apple emerge camera stove vicious recall dignity soon
                     margin deer organ stem combine melt ritual tumble shoe around
                     upper bracket eager put
```
*(Deliberately a perfect pattern for easy recomputation — entering it through the live UI trips the RED hard gate on autocorrelation, which itself confirms the gate works. Verify via the built-in self-test or by calling the functions directly.)*

### 📖 Usage (OpSec)

1.  **Download + verify:** from Releases → check SHA-256 above → verify the OpenPGP signature (`gpg --verify`).
2.  **Air-gapped Tails** (verify the Tails image signature before flashing).
3.  **Open** the file in Tor Browser.
4.  **Commit the plan before rolling:** pick dice type + seed size → the tool locks **N** → roll exactly N (with multiple dice, fix a reading order — e.g. red→white→black every throw).
5.  **Pass the gate:** RED → discard & re-roll a fresh batch. Bits short of target at N → discard (never top-up).
6.  **Hand-copy** the words, read them back, then **cross-verify on an independent offline device** (entropy hex → 24 words) before funding.
7.  **Shut down Tails** to wipe RAM.

**Disclaimer:** provided "AS IS". You are responsible for your environment's OpSec and for safeguarding your seed.

---
*"Don't Trust, Verify."* — **Chollatis Bitcoiner.**
