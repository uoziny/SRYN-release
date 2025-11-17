# SRYN 암호화 라이브러리 - 빠른 시작 가이드

## 🚀 **역할**
SRYN은 양자저항성 암호화 라이브러리로, 문자열과 파일의 안전한 암호화/복호화를 제공합니다. Java 7 이상 호환.

## 📝 **기본 사용법**

### 1. JAR 파일 추가
```bash
# Maven
<dependency>
    <groupId>com.kumadastudio</groupId>
    <artifactId>sryn-crypto</artifactId>
    <version>2.0.0</version>
</dependency>

# Gradle
implementation 'com.kumadastudio:sryn-crypto:2.0.0'

# 직접 사용
java -cp sryn-crypto-2.0.0.jar YourApp
```

### 2. 문자열 암호화
```java
import com.sryn.crypto.SRYNUtil;

public class Example {
    public static void main(String[] args) throws Exception {
        String encrypted = SRYNUtil.encryptString("비밀 메시지", "MyPassword123!");
        String decrypted = SRYNUtil.decryptString(encrypted, "MyPassword123!");
        System.out.println(decrypted); // 비밀 메시지
    }
}
```

### 3. 파일 암호화
```java
SRYNUtil.encryptFile("document.pdf", "document.pdf.enc", "MyPassword123!");
SRYNUtil.decryptFile("document.pdf.enc", "document.pdf", "MyPassword123!");
```

## 🔐 **보안 권장사항**
- 강력한 비밀번호 사용 (12자 이상).
- 비밀번호 하드코딩 피하기.
- 파일별 다른 비밀번호 권장.

## ⚠️ **에러 처리**
```java
try {
    String decrypted = SRYNUtil.decryptString(encrypted, "WrongPassword");
} catch (SecurityException e) {
    System.err.println("비밀번호 오류 또는 데이터 변조");
}
```

## ❓ **FAQ**
**Q1: 비밀번호 잊으면?** A1: 복구 불가능.  
**Q2: 파일 크기 제한?** A2: 없음.  
**Q3: Java 버전?** A3: 7~21 지원.  
**Q4: 상업 사용?** A4: 유료 라이선스 필요.

## 📚 **추가 정보**
- 라이선스: `LICENSE` (개인/학술 무료, 상업 유료)  
- 문의: uoziny@gmail.com

**SRYN Cryptographic Library v2.0.0**  
**Copyright © 2024-present kumadastudio**  
**SPDX-License-Identifier: LicenseRef-SRYN-Dual**

## 🎯 **실전 시나리오**

### 시나리오 1: 웹 애플리케이션 (사용자 데이터 암호화)

```java
import com.sryn.crypto.SRYNUtil;

public class UserDataService {
    public void saveUserData(String userId, String sensitiveData) throws Exception {
        // 사용자별 비밀번호 (환경 변수 또는 키 관리 시스템)
        String password = System.getenv("USER_KEY_" + userId);
        
        // DB에 저장할 암호문
        String encrypted = SRYNUtil.encryptString(sensitiveData, password);
        
        // DB 저장 (예: INSERT INTO users SET data = ?)
        saveToDatabase(userId, encrypted);
    }
    
    public String loadUserData(String userId) throws Exception {
        String encrypted = loadFromDatabase(userId);
        String password = System.getenv("USER_KEY_" + userId);
        
        return SRYNUtil.decryptString(encrypted, password);
    }
}
```

### 시나리오 2: 파일 전송 (네트워크 암호화)

```java
import com.sryn.crypto.SRYNUtil;
import java.net.*;

public class SecureFileTransfer {
    public void sendEncryptedFile(String filePath, Socket socket) throws Exception {
        // 1. 파일 암호화
        String encFile = filePath + ".enc";
        SRYNUtil.encryptFile(filePath, encFile, "NetworkKey2025!");
        
        // 2. 암호화된 파일 전송
        sendFile(encFile, socket);
        
        // 3. 정리
        new java.io.File(encFile).delete();
    }
    
    public void receiveEncryptedFile(Socket socket, String outputPath) throws Exception {
        // 1. 암호화된 파일 수신
        String encFile = outputPath + ".enc";
        receiveFile(socket, encFile);
        
        // 2. 파일 복호화
        SRYNUtil.decryptFile(encFile, outputPath, "NetworkKey2025!");
        
        // 3. 정리
        new java.io.File(encFile).delete();
    }
}
```

### 시나리오 3: 로그 파일 암호화 (자동 백업)

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
                
                // 암호화 후 원본 삭제
                SRYNUtil.encryptFile(log.getAbsolutePath(), encrypted, archivePassword);
                log.delete();
                
                System.out.println("보관 완료: " + encrypted);
            }
        }
    }
}
```

---

## 📚 **추가 리소스**

- **기술 지원**: uoziny@gmail.com
- **라이선스**: `LICENSE` (개인/학술 무료, 상업 유료)

---

## ❓ **FAQ**

**Q1: 비밀번호를 잊어버리면 복구할 수 있나요?**  
A1: 아니요. 양자 저항 암호화로 보호되어 있어 복구 불가능합니다.

**Q2: 파일 크기 제한이 있나요?**  
A2: 없습니다. 스트리밍 방식으로 처리하여 100GB 이상도 가능합니다.

**Q3: Java 버전 호환성은?**  
A3: Java 7 ~ 21 모두 지원합니다.

**Q4: 상업적 사용이 가능한가요?**  
A4: 유료 라이선스가 필요합니다. uoziny@gmail.com

**Q5: 개인/학습 목적은 무료인가요?**  
A5: 네, 개인 프로젝트, 학습, 연구는 무료입니다. (비상업적)

---

**SRYN Cryptographic Library v2.0.0**  
**Copyright © 2024-present kumadastudio (bighill)**  
**SPDX-License-Identifier: LicenseRef-SRYN-Dual**  
**개인/학술 무료 | 상업/기업 유료**  
**문의: uoziny@gmail.com**
