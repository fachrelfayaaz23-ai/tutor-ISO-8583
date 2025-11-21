# tutor-ISO-8583
Oke, gue jelasin **ulang dari nol**, pakai bahasa yang *paling gampang*, terus nanti gue lanjut jelasin **semua bagian** ISO-8583 untuk transaksi tarik tunai ATM.

---

# 🔥 **1. APA ITU ISO-8583? (Versi paling gampang)**

ISO-8583 itu **format standar** yang dipakai bank & ATM untuk ngobrol satu sama lain.

Anggap aja ini seperti **format chat antar mesin**.
Kalo ATM mau narik uang, dia ngirim pesan ke bank dalam **format 8583**.

Pesan itu isinya:

* “Nomor kartunya ini ya…”
* “Mau tarik berapa…”
* “Ini PIN-nya tapi sudah dienkripsi…”
* “Ini waktu transaksinya…”
* dll.

---

# 🔥 **2. Struktur pesan ISO-8583 itu cuma 3 bagian utama**

```
[MTI] [BITMAP] [DATA ELEMENTS]
```

## 1) **MTI** → jenis pesan apa

## 2) **Bitmap** → field apa saja yang dipakai

## 3) **Data Elements (DE)** → isi pesannya

---

# 🔥 **3. Contoh *nyata* pesan tarik tunai ATM (ISO-8583:1987)**

Kita pakai pesan request **0200** (Financial Transaction Request):

```
MTI: 0200

Bitmap: 723A840128A08012

DE2 : 5264221001234567
DE3 : 010000
DE4 : 000000010000
DE7 : 1120193045
DE11: 123456
DE12: 193045
DE13: 1120
DE14: 2612
DE18: 6011
DE22: 021
DE25: 00
DE32: 000123
DE35: 5264221001234567=261210112345678000
DE37: ATM2031123045
DE41: ATM12345
DE42: BANKATM000001
DE49: 360
DE52: A1B2C3D4E5F67890   ← PIN Block
DE102: 1234567890
```


# 🔥 FORMAT MTI

MTI terdiri dari 4 digit:

| Digit | Arti                                                            |
| ----- | --------------------------------------------------------------- |
| 1     | Version (1 = 1987)                                              |
| 2     | Class (1 = authorization, 2 = financial, 3 = file action, dll.) |
| 3     | Function (0 = request, 1 = response, 2 = advice)                |
| 4     | Origin (0 = acquirer, 1 = issuer, dll.)                         |

Contoh:
**0200 = financial request**
**0210 = financial response**

---

# 📌FORMAT BITMAP

* Bitmap 1 = DE 1–64
* Bitmap 2 = DE 65–128

---

# 🚀 SEKARANG MASUK KE DE ISO 8583:1987

**Ini daftar lengkapnya dulu (full table).**

## 📌 PRIMARY BITMAP (DE1–64)

| DE | Nama                                | Format                 |
| -- | ----------------------------------- | ---------------------- |
| 1  | Secondary Bitmap                    | b16                    |
| 2  | PAN                                 | LLVAR (max 19) numeric |
| 3  | Processing Code                     | n6                     |
| 4  | Amount, Transaction                 | n12                    |
| 5  | Amount, Settlement                  | n12                    |
| 6  | Amount, Cardholder Billing          | n12                    |
| 7  | Transmission Date & Time            | n10                    |
| 8  | Amount, Cardholder Fee              | n8                     |
| 9  | Conversion Rate, Settlement         | n8                     |
| 10 | Conversion Rate, Billing            | n8                     |
| 11 | STAN                                | n6                     |
| 12 | Local Transaction Time              | n6 (HHMMSS)            |
| 13 | Local Transaction Date              | n4 (MMDD)              |
| 14 | Expiry Date                         | n4 (YYMM)              |
| 15 | Settlement Date                     | n4                     |
| 16 | Conversion Date                     | n4                     |
| 17 | Capture Date                        | n4                     |
| 18 | Merchant Type                       | n4                     |
| 19 | Acquiring Institution Country       | n3                     |
| 20 | PAN Country Code                    | n3                     |
| 21 | Forwarding Inst. Country Code       | n3                     |
| 22 | POS Entry Mode                      | n3                     |
| 23 | PAN Sequence Number                 | n3                     |
| 24 | Function Code                       | n3                     |
| 25 | POS Condition Code                  | n2                     |
| 26 | POS PIN Capture Code                | n2                     |
| 27 | Authorizing Identification Resp Len | n1                     |
| 28 | Amount, Transaction Fee             | x+n9                   |
| 29 | Amount, Settlement Fee              | x+n9                   |
| 30 | Amount, Transaction Processing Fee  | x+n9                   |
| 31 | Amount, Settlement Processing Fee   | x+n9                   |
| 32 | Acquiring Institution ID Code       | LLVAR (11) numeric     |
| 33 | Forwarding Institution ID Code      | LLVAR (11) numeric     |
| 34 | PAN Extended                        | LLVAR (28)             |
| 35 | Track 2 Data                        | LLVAR (37)             |
| 36 | Track 3 Data                        | LLLVAR (104)           |
| 37 | Retrieval Reference Number          | an12                   |
| 38 | Authorization ID                    | an6                    |
| 39 | Response Code                       | an2                    |
| 40 | Service Restriction Code            | an3                    |
| 41 | Terminal ID                         | ans8                   |
| 42 | Merchant ID                         | ans15                  |
| 43 | Card Acceptor Location              | ans40                  |
| 44 | Additional Response Data            | LLVAR (25)             |
| 45 | Track 1 Data                        | LLVAR (76)             |
| 46 | Additional Data (ISO)               | LLLVAR (999)           |
| 47 | Additional Data (National)          | LLLVAR (999)           |
| 48 | Additional Data (Private)           | LLLVAR (999)           |
| 49 | Currency Code, Transaction          | n3                     |
| 50 | Currency Code, Settlement           | n3                     |
| 51 | Currency Code, Billing              | n3                     |
| 52 | PIN Data                            | b8                     |
| 53 | Security Control                    | n16                    |
| 54 | Additional Amounts                  | LLLVAR                 |
| 55 | ICC Data (EMV)                      | LLLVAR                 |
| 56 | Reserved (ISO)                      | LLLVAR                 |
| 57 | Reserved (national)                 | LLLVAR                 |
| 58 | Reserved (national)                 | LLLVAR                 |
| 59 | Reserved (national)                 | LLLVAR                 |
| 60 | Reserved (national)                 | LLLVAR                 |
| 61 | Reserved (private)                  | LLLVAR                 |
| 62 | Reserved (private)                  | LLLVAR                 |
| 63 | Reserved (private)                  | LLLVAR                 |
| 64 | MAC                                 | b8                     |

---

## 📌 SECONDARY BITMAP (DE65–128)

Jika bit 1 = 1 maka field ini boleh muncul.

Aku akan jelaskan kalau kamu butuh sampai 128.

---

Siap. Kita mulai dari **dasarnya dulu**, supaya ketika nanti masuk ke **DE 1–128 versi ISO 8583:1987**, kamu benar-benar paham total.

---

# 🧩 **1. FORMAT DATA ISO 8583 (N, AN, ANS, BINARY, LLVAR, LLLVAR, dst.)**

ISO 8583 memakai **kode** untuk menjelaskan:

1. **Jenis karakter**
2. **Berapa panjangnya**
3. **Apakah panjangnya fixed atau variabel**

Berikut penjelasan lengkap.

---

## 🔥 **A. Jenis Karakter (Data Type)**

### **1) n (numeric)**

Hanya angka 0–9.

Contoh format: **n6**
→ Artinya **6 digit angka**.

Contoh isi valid:

```
123456
000789
```

---

### **2) a (alphabetic)**

Hanya huruf A–Z.

---

### **3) an (alphanumeric)**

Huruf + angka.

Contoh: **an12**

```
AB12CD34EF56
```

---

### **4) ans (alphanumeric special)**

Huruf + angka + karakter seperti spasi, titik, dash, dll.

Digunakan untuk Terminal ID, Merchant Name, Address.

Contoh:

```
ATM12345
TOKO INDO KOPI
```

---

### **5) b (binary)**

Hexadecimal (0–9, A–F).
Digunakan untuk:

* Bitmap (16 bytes)
* MAC (Message Authentication Code)
* PIN Block (8 bytes = 16 hex)

Contoh bitmap:

```
723A840128A08012
```

Contoh PIN block:

```
A1B2C3D4E5F67890
```

---

# 🔥 **B. Panjang Data (Fixed / Variable)**

### **1) Fixed length**

Panjang tetap.

Contoh: **n6**
Selalu 6 digit.

---

### **2) LLVAR**

**L = Length**, jadi:

* **LL = 2 digit length**
* **VAR = variable length**

Contoh DE2 (PAN) = LLVAR (max 19)

Jika data = `5264221001234567` (16 digit)
maka encode:

```
16 5264221001234567
```

“16” = panjang data.

---

### **3) LLLVAR**

3 digit length + data.

Contoh Track 3 (max 104 digit).
Jika panjang data = 55:

```
055 <data>
```

---

# 🧩 **2. ENTITAS DALAM TRANSAKSI (ACQUIRER, ISSUER, SWITCH, HOST)**

Sebelum belajar DE, kamu wajib tahu "siapa melakukan apa".

---

## 🔥 **1) Issuer**

Bank yang **mengeluarkan kartu**.

Contoh:
Kartu BRI → Issuer = BRI.

Issuer berfungsi:

* Validasi PIN
* Cek saldo
* Memberi auth code
* Terbitkan 0210 response

---

## 🔥 **2) Acquirer**

Bank yang **memproses transaksi dari terminal**.

* Bank pemilik ATM (kalau transaksi di ATM)
* Bank pemilik EDC/POS (kalau transaksi di toko)

Fungsi:

* Menerima swipe/insertion
* Membuat ISO 8583 0200
* Mengirim ke switch/issuer

---

## 🔥 **3) Switch / Network**

Jembatan antar bank.

Contoh Indonesia:

* ATM Bersama
* PRIMA
* LINK
* GPN

Contoh global:

* VisaNet (Visa)
* Mastercard Network
* UnionPay

Switch:

* Cek routing (bank mana issuer)
* Forward 0200 → issuer
* Forward 0210 → acquirer

---

## 🔥 **4) Cardholder**

Pemilik kartu.

---

## 🔥 **5) Merchant**

Toko yang menerima pembayaran.

---

## 🔥 Urutan Transaksi ATM (aliran ISO 8583)

1. Cardholder → ATM
2. ATM → Acquirer
3. Acquirer → Switch
4. Switch → Issuer
5. Issuer → Switch
6. Switch → Acquirer
7. Acquirer → ATM
8. ATM → Cardholder

---

# 🧩 **3. STRUKTUR MESSAGE ISO 8583**

Format umum:

```
MTI (4 digit)
Primary Bitmap (16 hex)
Secondary Bitmap (16 hex, optional)
DE1
DE2
DE3
... sampai DE128
```

---

# 🧩 **4. TIPE PESAN (MTI)**

Contoh yang sering:

| MTI  | Arti                      |
| ---- | ------------------------- |
| 0100 | Authorization request     |
| 0110 | Authorization response    |
| 0200 | Financial request         |
| 0210 | Financial response        |
| 0220 | Financial advice          |
| 0230 | Financial advice response |
| 0400 | Reversal request          |
| 0410 | Reversal response         |

---

SIAP.
**Gue bakal jelasin lagi SEMUA DE yang dipakai buat tarik tunai ATM**, tapi:

* **lebih rinci**
* **lebih detail**
* **lebih enak dibaca (rapi, tegas, profesional tapi tetap gampang dipahami)**
* **masih pakai format yang SAMA**
* **ditambah: pilihan isiannya apa saja, kenapa, kapan dipakai, logicnya gimana**

Gaya penjelasan bakal kayak *senior switch engineer ngejelasin ke engineer baru*.

---

# ⚡ **DE YANG DIPAKAI UNTUK TARIK TUNAI ATM — SUPER DETAIL VERSION**

Format tetap:

### 🔹 **DE xx — Nama Field**

**Format:**
**Fungsi:**
**Cara Isi:**
**Pilihan Isi:**
**Contoh:**
**Catatan:**

---

# 🔥 **DE 2 — PRIMARY ACCOUNT NUMBER (PAN)**

**Format:**

* **LLVAR (maks 19 digit)**

**Fungsi:**

* Nomor kartu yang dipakai untuk transaksi.
* Issuer pakai ini untuk *identifikasi customer* dan *routing* ke sistem core banking.
* Switch pakai PAN untuk menentukan BIN (Bank Identification Number) → routing ke acquirer/issuer.

**Cara Isi:**

* ATM membaca PAN dari **Track 2** kartu.
* Tidak boleh diedit oleh terminal maupun acquirer.
* Dikirim apa adanya (kecuali masking di log).

**Pilihan Isi:**

* 13–19 digit.
* Format selalu numeric.
* Panjang tergantung issuer (Visa, Mastercard, GPN, private card).

**Contoh:**

```
16 5264221001234567
```

**Catatan:**

* Sangat sensitif → PCI-DSS.
* Tidak dienkripsi, tapi tidak boleh tampil di log open text.

---

# 🔥 **DE 3 — PROCESSING CODE**

**Format:**

* **n6**

**Fungsi:**

* Menjelaskan jenis transaksi, from-account, dan to-account.
* Ini field *PALING PENTING* untuk menentukan transaksi apa yang dilakukan.

**Cara Isi:**
Format:

```
FF TT TT
F = From account  
T = To account  
TT = Transaction Type
```

**Pilihan Isi:**

### **Digit 1–2 (FROM ACCOUNT):**

* 00 = Default
* 01 = Savings
* 02 = Checking
* 03 = Credit
* 10 = Credit Card Payment (jarang untuk ATM)

### **Digit 3–4 (TO ACCOUNT):**

Karena tarik tunai **tidak memindahkan uang ke rekening tujuan**, biasanya:

* 00 = Default

### **Digit 5–6 (TRANSACTION TYPE):**

* 00 = Purchase (POS)
* **01 = Cash withdrawal (ATM)**
* 30 = Inquiry
* 31 = Balance inquiry
* 40 = Transfer
* 50 = Payment

**Untuk tarik tunai:**

```
010000  → From savings, to nothing, cash withdrawal
```

**Contoh:**

```
010000
```

**Catatan:**

* Issuer baca DE3 untuk menentukan rule: apakah boleh, apakah kartu support, apakah limit cukup.

---

# 🔥 **DE 4 — TRANSACTION AMOUNT**

**Format:**

* **n12**

**Fungsi:**

* Nominal uang yang mau ditarik.

**Cara Isi:**

* Angka saja, 12 digit, leading zero.
* Tidak ada titik/koma.
* ATM menentukan nilai berdasarkan input user.

**Pilihan Isi:**

* 000000000100 → Rp 100
* 000000010000 → Rp 10.000
* 000001000000 → Rp 1.000.000

**Contoh:**

```
000000010000
```

**Catatan:**

* Bisa ditambah surcharge jika menggunakan foreign ATM (host menambahkan DE28).

---

# 🔥 **DE 7 — TRANSMISSION DATE AND TIME**

**Format:**

* **n10 (MMDDhhmmss)**

**Fungsi:**

* Waktu transaksi dikirim oleh acquirer, bukan waktu ATM.

**Cara Isi:**

* Acquirer switch isi field ini dengan timestamp server.
* Dipakai untuk settlement.

**Pilihan Isi:**

* Harus format timestamp yang valid.

**Contoh:**

```
1120193045
```

**Catatan:**

* Kalau server telat → bisa reject karena timeout window.

---

# 🔥 **DE 11 — STAN**

**Format:**

* **n6**

**Fungsi:**

* Nomor unik transaksi untuk 1 hari.
* Dipakai untuk:

  * Request–response matching
  * Reversal
  * Settlement
  * Dispute tracking

**Cara Isi:**

* ATM generate counter 000001 → 999999.
* Reset harian.

**Pilihan Isi:**

* Harus 000001–999999
* Tidak boleh sama dalam 24 jam pada terminal yang sama.

**Contoh:**

```
123456
```

**Catatan:**

* STAN bukan RRN.

---

# 🔥 **DE 12 — LOCAL TRANSACTION TIME**

**Format:**

* **n6 (hhmmss)**

**Fungsi:**

* Waktu **di ATM**.

**Cara Isi:**

* ATM ambil jam internal.
* Tidak peduli timezone acquirer.

**Pilihan Isi:**

* 000000 → 235959

**Contoh:**

```
193045
```

**Catatan:**

* Kalau jam ATM error (misal reset), issuer bisa tolak transaksi karena mismatch risk rules.

---

# 🔥 **DE 13 — LOCAL TRANSACTION DATE**

**Format:**

* **n4 (MMDD)**

**Fungsi:**

* Tanggal lokal ATM.

**Cara Isi:**

* ATM mengisi berdasarkan internal clock.

**Contoh:**

```
1120
```

**Catatan:**

* Digabung dengan DE12 untuk fraud detection.

---

# 🔥 **DE 14 — EXPIRATION DATE**

**Format:**

* n4 (YYMM)

**Fungsi:**

* Validasi apakah kartu masih berlaku.

**Cara Isi:**

* ATM baca dari Track 2.

**Pilihan Isi:**
Contoh format:

* 2612 → Desember 2026
* 2508 → Agustus 2025

**Contoh:**

```
2612
```

**Catatan:**

* Issuer wajib reject kalau expired.

---

# 🔥 **DE 18 — MERCHANT CATEGORY CODE (MCC)**

**Format:**

* n4

**Fungsi:**

* Klasifikasi jenis terminal.

**Cara Isi:**
Untuk ATM **selalu**:

* **6011 = Automated Teller Machine**

**Contoh:**

```
6011
```

**Catatan:**

* Avoid confusion dengan POS.

---

# 🔥 **DE 22 — POS ENTRY MODE**

**Format:**

* n3

**Fungsi:**

* Menjelaskan bagaimana data kartu dibaca.

**Cara Isi:**
Digit 1–2 = mode kartu
Digit 3 = mode PIN

**Pilihan Isi PENTING:**

### **Mode kartu (digit 1–2):**

* 02 = Magnetic stripe read
* 05 = Chip read
* 07 = NFC (contactless)

### **Mode PIN (digit 3):**

* 1 = PIN entry capability
* 2 = Contactless + PIN
* 0 = ATM (PIN ALWAYS used)

**ATM paling umum:**

* 021 = Magstripe + PIN
* 051 = Chip + PIN

**Contoh:**

```
021
```

**Catatan:**

* Sangat penting untuk fraud risk scoring.

---

# 🔥 **DE 25 — POS CONDITION CODE**

**Format:**

* n2

**Fungsi:**

* Menjelaskan kondisi transaksi.

**Pilihan Isi:**

* **00 = Normal (ATM ALWAYS use this)**
* 08 = Mail/phone order (POS only)
* 99 = Unknown condition

**Contoh:**

```
00
```

**Catatan:**

* ATM selalu 00, tidak berubah.

---

# 🔥 **DE 32 — ACQUIRER ID**

**Format:**

* LLVAR

**Fungsi:**

* Mengidentifikasi bank acquirer.

**Cara Isi:**

* Switch atau ATM provider isi sesuai kode bank.

**Contoh:**

```
06 000123
```

**Pilihan Isi:**

* 5–11 digit

**Catatan:**

* Penting untuk settlement antar bank.

---

# 🔥 **DE 35 — TRACK 2 DATA**

**Format:**

* LLVAR

**Fungsi:**

* Data kartu untuk issuer validation.

**Cara Isi:**
Format:

```
PAN=YYMMSSSxxxxxxxxxxxx
```

**Pilihan Isi:**

* Service code contoh:

  * 101 = Magnetic, international
  * 201 = Chip, international
  * 221 = Chip, domestic only

**Contoh:**

```
37 5264221001234567=261210112345678000
```

**Catatan:**

* Tidak encrypted
* Berisi expiry dan service code

---

# 🔥 **DE 37 — RRN (Retrieval Reference Number)**

**Format:**

* an12

**Fungsi:**

* ID unik secara global.

**Cara Isi:**
Format umum:

* YYDDD + HHMMSS
* Julian date + STAN
* Terserah acquirer, asalkan unik.

**Contoh:**

```
203112304512
```

**Catatan:**

* Digunakan untuk reversal & dispute.

---

# 🔥 **DE 41 — TERMINAL ID (TID)**

**Format:**

* ans8

**Fungsi:**

* Identifikasi unik ATM.

**Cara Isi:**

* Fixed sebelum ATM beroperasi.
* Tidak berubah.

**Contoh:**

```
ATM12345
```

**Pilihan Isi:**

* Huruf + angka
* 8 karakter tepat

**Catatan:**

* Unik dalam network acquirer.

---

# 🔥 **DE 42 — CARD ACCEPTOR ID (MID)**

**Format:**

* ans15

**Fungsi:**

* Identitas pemilik ATM (bank/acquirer).

**Contoh:**

```
BANKATM000001
```

**Catatan:**

* Biasanya 15 char fixed padded.

---

# 🔥 **DE 49 — TRANSACTION CURRENCY CODE**

**Format:**

* n3

**Fungsi:**

* Menentukan mata uang transaksi.

**Cara Isi:**

* Menggunakan ISO 4217.

**Pilihan Isi ATM:**

* 360 = Rupiah
* 840 = USD (jika ATM forex)
* 978 = EUR (international zone)

**Contoh:**

```
360
```

---

# 🔥 **DE 52 — PIN BLOCK**

**Format:**

* b16 (8 byte) hex

**Fungsi:**

* Mengirim PIN terenkripsi 3DES.

**Cara Isi:**
Format ISO-0 PIN block:

```
0 | PIN length | PIN digits | F padding  
XOR  
PAN rightmost 12 digits (excluding last)
```

**Contoh:**

```
A1B2C3D4E5F67890
```

**Pilihan Isi:**

* Selalu 16 hex digit
* Bukan ASCII

**Catatan:**

* Satu-satunya field terenkripsi.

---

# 🔥 **DE 102 — FROM ACCOUNT**

**Format:**

* LLVAR

**Fungsi:**

* Menentukan rekening sumber uang.

**Cara Isi:**

* Nomor rekening customer
* Bentuk numeric string

**Contoh:**

```
10 1234567890
```

---

