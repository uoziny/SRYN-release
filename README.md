# SRYN-release
sryn-1.0.0.jar

# SRYN — Secure Random Yielding Network

![License: SRYN Dual](https://img.shields.io/badge/license-Dual--SRYN-blue)
![Java](https://img.shields.io/badge/java-11%2B-green)


**Java Post-Quantum Cryptography Library**  
_Copyright (c) 2024-present kumadastudio (uoziny)_  
_Contact: uoziny@gmail.com_

---

## 🛡️ What is SRYN?

SRYN is a pure Java library for quantum-resistant cryptography.  
It supports multi-mode encryption, one-way hashing, digital signature, and secure key management.

---

## 🔑 Symmetric Encryption

- **Class:** `SRYNCipherEngine`
- **Purpose:** Fast, secure two-way encryption and decryption using PQC SIME algorithm (dynamic S-Box, Barrett reduction)
- **Typical Use:**
    ```java
    int[] key = RandomUtil.generateRandomIntArray(seed, 768/32);<br>
    SRYNCipherEngine cipher = new SRYNCipherEngine(key);<br>
    byte[] msg = "Secret".getBytes(StandardCharsets.UTF_8);<br>
    int[] enc = cipher.encrypt(ByteUtil.encodeToIntArray(msg));<br>
    int[] dec = cipher.decrypt(enc);<br>
    String recovered = new String(ByteUtil.decodeFromIntArray(dec), StandardCharsets.UTF_8);<br>
    ```

---

## 🚫 Irreversible (One-way) Encryption

- **Class:** `SRYNEncryptor`
- **Purpose:** Irreversible encryption for tokens, verification, or irreversible storage (Blake3-based)
- **Typical Use:**
    ```java
    byte[] hash = SRYNEncryptor.encrypt("token-data", seed);
    ```

---

## ✍️ Digital Signature

- **Classes:** `SRYNSignature`, `SRYNSignatureVerifier`
- **Purpose:** Digital signing and verification (non-repudiation, PQC key agreement)
- **Typical Use:**
    ```java
    byte[] sig = SRYNSignature.sign("message", privateSeed, publicSeed);<br>
    boolean valid = SRYNSignatureVerifier.verifySignature("message", sig, privateSeed, publicSeed);<br>
    ```

---

## 🔗 Key Agreement / KEM

- **Classes:** `SRYNKeyAgreement`, `SRYNKemGenerator`<br>
- **Purpose:** PQC deterministic key generation and shared key agreement<br>
- **Note:** For use in multi-party encryption and signatures<br>

---

## #️⃣ Hashing

- **Class:** `SRYNBlake3`
- **Purpose:** Quantum-resistant hashing, 512bit+ output (custom Blake3 reimplementation)
- **Typical Use:**<br>
    ```java
    byte[] digest = SRYNBlake3.digest(data);
    ```

---

## 🧰 Utilities

- **Classes:** `ByteUtil`, `RandomUtil`, `KeyUtil`
- **Purpose:** Safe byte/int conversion, secure PRNG, key conversion
- **Typical Use:**
    ```java
    int[] arr = ByteUtil.encodeToIntArray(bytes);
    byte[] bytes = ByteUtil.decodeFromIntArray(arr);
    int[] random = RandomUtil.generateRandomIntArray(seed, 24);
    ```

---

## 🧪 Benchmark & Test

- **Class:** `SRYNBenchmarkSuite` and friends
- **Purpose:** Run performance tests and validate all cryptographic operations
- **Sample Output:**
    ```
    [SRYN SYMMETRIC ENCRYPT/DECRYPT TEST]<br>
    Original: John Doe/010-1234-1234/660101-1320013<br>
    Recovered: John Doe/010-1234-1234/660101-1320013<br>
    Match: true<br>
    [SRYN ONE-WAY ENCRYPTOR TEST]<br>
    Encrypted (hex): 46DA...<br>
    [SRYN DIGITAL SIGNATURE TEST]<br>
    Signature valid: true<br>
    ```
---

## 🚀 Usage

### 1. Bidirectional (Symmetric) Encryption/Decryption

```java
// 768bit key (int[24]), seed must be at least 32 bytes
String seed = "your_secure_seed_32bytes_minimum";
int[] key = RandomUtil.generateRandomIntArray(seed, 24);
SRYNCipherEngine cipher = new SRYNCipherEngine(key);

// Encrypt
String plain = "Sensitive information";
byte[] plainBytes = plain.getBytes(StandardCharsets.UTF_8);
int[] enc = cipher.encrypt(ByteUtil.encodeToIntArray(plainBytes));

// Decrypt
int[] dec = cipher.decrypt(enc);
String recovered = new String(ByteUtil.decodeFromIntArray(dec), StandardCharsets.UTF_8);
```
---





## 🛠️ SpringBoot Integration

You can use SRYN as a Spring Bean:


```ini
sryn.crypto.seed.local=YOUR_PRIVATE_SEED
sryn.crypto.seed.remote=YOUR_REMOTE_SEED


```java
@Component
public class SRYNCryptoService {
    private final SRYNCryptoSignatureBox box;

    @Autowired
    public SRYNCryptoService(@Value("${sryn.crypto.seed.local}") String localSeed,
                             @Value("${sryn.crypto.seed.remote}") String remoteSeed) {
        int[] key = RandomUtil.generateRandomIntArray(localSeed, 768 / 32);
        this.box = new SRYNCryptoSignatureBox(key, localSeed, remoteSeed);
    }
    // ...API methods...
}

