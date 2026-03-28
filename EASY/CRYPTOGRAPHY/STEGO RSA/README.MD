
---

# 🏁 Challenge Writeup

## 📌 Challenge Name

**StegoRSA**

## 🎯 Level

**Easy**

## 🧩 Category

**Cryptography**

---

# 🧠 Concept Used

* Steganography (hidden data in image metadata)
* RSA decryption using leaked private key
* Hex → PEM key reconstruction

---

# 🪜 Step-by-Step Solution

## 🔍 Step 1: Analyze the image metadata

```bash
exiftool image.jpg
```

👉 Found in `Comment`:

* A long **hex string**

---

## 🔄 Step 2: Convert HEX → Private Key

Hint:

> “Hex can be turned back into a key file”

### Method:

```bash
echo "<HEX_DATA>" | xxd -r -p > solve.pem
```

👉 This reconstructs:

```
-----BEGIN PRIVATE KEY-----
...
-----END PRIVATE KEY-----
```

---

## 🔑 Step 3: Verify the key

```bash
cat solve.pem
```

👉 Confirms valid RSA private key

---

## 📥 Step 4: Download encrypted flag

```bash
wget <flag.enc URL>
```

---

## 🔓 Step 5: Decrypt the flag

```bash
openssl pkeyutl -decrypt -inkey solve.pem -in flag.enc -out decrypted_flag.txt
```

---

## 🏆 Step 6: Get the flag

```bash
cat decrypted_flag.txt
```

✅ Output:

```
picoCTF{rs4_k3y_1n_1mg_26586619}
```

---

# ⚙️ Command Breakdown (VERY IMPORTANT)

### Command:

```bash
openssl pkeyutl -decrypt -inkey solve.pem -in flag.enc -out decrypted_flag.txt
```

Let’s break **every part**:

---

## 🔧 `openssl`

* A powerful cryptographic toolkit
* Used for encryption, decryption, key handling

---

## 🛠️ `pkeyutl`

* Stands for **Public Key Utility**
* Used for operations with public/private keys (RSA, etc.)

---

## 🔓 `-decrypt`

* Tells OpenSSL:
  👉 “Perform decryption”
* Uses **private key** to decrypt data

---

## 🔑 `-inkey solve.pem`

* Specifies the **input key file**
* Here:

  * `solve.pem` = your recovered private key

---

## 📥 `-in flag.enc`

* Input file (encrypted data)
* Contains ciphertext

---

## 📤 `-out decrypted_flag.txt`

* Output file (decrypted result)
* Stores plaintext (flag)

---

# 🧠 What Actually Happened (Logic)

1. RSA encryption was used 🔐

2. Public key missing ❌

3. BUT private key was:

   * Hidden in image metadata
   * Encoded in HEX

4. You:

   * Extracted metadata
   * Converted HEX → PEM
   * Used OpenSSL to decrypt

👉 **Game over for RSA security **

---

# 💡 Key Learning Points

* Metadata can leak sensitive info ⚠️
* Encoding (Hex/Base64) ≠ Encryption
* If private key is exposed → RSA is useless
* Always check:

  * exiftool
  * strings
  * binwalk  

---


