# SRYN Cryptographic Library - Quick Start Guide

## 🚀 **Role**
SRYN is a quantum-resistant encryption library that provides secure encryption/decryption of strings and files. Compatible with Java 7 and above.

## 📝 **Basic Usage**

### 1. Add JAR File
```bash
# Maven
<dependency>
    <groupId>com.kumadastudio</groupId>
    <artifactId>sryn-crypto</artifactId>
    <version>2.0.0</version>
</dependency>

# Gradle
implementation 'com.kumadastudio:sryn-crypto:2.0.0'

# Direct Use
java -cp sryn-crypto-2.0.0.jar YourApp
```

### 2. String Encryption
```java
import com.sryn.crypto.SRYNUtil;

public class Example {
    public static void main(String[] args) throws Exception {
        String encrypted = SRYNUtil.encryptString("Secret message", "MyPassword123!");
        String decrypted = SRYNUtil.decryptString(encrypted, "MyPassword123!");
        System.out.println(decrypted); // Secret message
    }
}
```

### 3. File Encryption
```java
SRYNUtil.encryptFile("document.pdf", "document.pdf.enc", "MyPassword123!");
SRYNUtil.decryptFile("document.pdf.enc", "document.pdf", "MyPassword123!");
```

## 🔐 **Security Recommendations**
- Use strong passwords (12+ characters).
- Avoid hardcoding passwords.
- Use different passwords for each file.

## ⚠️ **Error Handling**
```java
try {
    String decrypted = SRYNUtil.decryptString(encrypted, "WrongPassword");
} catch (SecurityException e) {
    System.err.println("Password error or data tampering");
}
```

## ❓ **FAQ**
**Q1: What if I forget the password?** A1: Irrecoverable.  
**Q2: File size limit?** A2: None.  
**Q3: Java version?** A3: 7-21 supported.  
**Q4: Commercial use?** A4: Paid license required.

## 📚 **Additional Information**
- License: `LICENSE` (Free for personal/academic, paid for commercial)  
- Contact: uoziny@gmail.com

**SRYN Cryptographic Library v2.0.0**  
**Copyright © 2024-present kumadastudio**  
**SPDX-License-Identifier: LicenseRef-SRYN-Dual**

## 🎯 **Real-World Scenarios**

### Scenario 1: Web Application (User Data Encryption)

```java
import com.sryn.crypto.SRYNUtil;

public class UserDataService {
    public void saveUserData(String userId, String sensitiveData) throws Exception {
        // User-specific password (environment variable or key management system)
        String password = System.getenv("USER_KEY_" + userId);
        
        // Encrypted data to save in DB
        String encrypted = SRYNUtil.encryptString(sensitiveData, password);
        
        // Save to DB (e.g., INSERT INTO users SET data = ?)
        saveToDatabase(userId, encrypted);
    }
    
    public String loadUserData(String userId) throws Exception {
        String encrypted = loadFromDatabase(userId);
        String password = System.getenv("USER_KEY_" + userId);
        
        return SRYNUtil.decryptString(encrypted, password);
    }
}
```

### Scenario 2: File Transfer (Network Encryption)

```java
import com.sryn.crypto.SRYNUtil;
import java.net.*;

public class SecureFileTransfer {
    public void sendEncryptedFile(String filePath, Socket socket) throws Exception {
        // 1. Encrypt file
        String encFile = filePath + ".enc";
        SRYNUtil.encryptFile(filePath, encFile, "NetworkKey2025!");
        
        // 2. Send encrypted file
        sendFile(encFile, socket);
        
        // 3. Cleanup
        new java.io.File(encFile).delete();
    }
    
    public void receiveEncryptedFile(Socket socket, String outputPath) throws Exception {
        // 1. Receive encrypted file
        String encFile = outputPath + ".enc";
        receiveFile(socket, encFile);
        
        // 2. Decrypt file
        SRYNUtil.decryptFile(encFile, outputPath, "NetworkKey2025!");
        
        // 3. Cleanup
        new java.io.File(encFile).delete();
    }
}
```

### Scenario 3: Log File Encryption (Automatic Backup)

```java
import com.sryn.crypto.SRYNUtil;
import java.io.File;
import java.text.SimpleDateFormat;
import java.util.Date;

public class SecureLogManager {
    public void archiveAndEncryptLogs() throws Exception {
        File logDir = new File("logs/");
        File[] logFiles = logDir.listFiles();
        
        String date = new SimpleDateFormat("yyyyMMdd").format(new Date());
        String archivePassword = "LogArchive2025_" + date;
        
        for (File log : logFiles) {
            if (log.getName().endsWith(".log")) {
                String encrypted = "archive/" + log.getName() + ".enc";
                
                // Encrypt and delete original
                SRYNUtil.encryptFile(log.getAbsolutePath(), encrypted, archivePassword);
                log.delete();
                
                System.out.println("Archived: " + encrypted);
            }
        }
    }
}
```

---

## 📚 **Additional Resources**

- **Technical Support**: uoziny@gmail.com
- **License**: `LICENSE` (Free for personal/academic, paid for commercial)

---

## ❓ **FAQ**

**Q1: What if I forget the password?**  
A1: No, it is protected by quantum-resistant encryption and cannot be recovered.

**Q2: Is there a file size limit?**  
A2: No, it handles files up to 100GB+ using streaming.

**Q3: Java version compatibility?**  
A3: Supports Java 7 to 21.

**Q4: Commercial use allowed?**  
A4: Requires a paid license. Contact uoziny@gmail.com

**Q5: Free for personal/learning purposes?**  
A5: Yes, free for personal projects, learning, and research (non-commercial).

---

**SRYN Cryptographic Library v2.0.0**  
**Copyright © 2024-present kumadastudio (bighill)**  
**SPDX-License-Identifier: LicenseRef-SRYN-Dual**  
**Free for Personal/Academic | Paid for Commercial/Enterprise**  
**Contact: uoziny@gmail.com**