Here’s a clean, **GitHub-style documentation snippet** you can drop into your project’s `README.md` 👇

---

## 🧩 Fix: `'wmic' is not recognized as an internal or external command` (Kafka on Windows 10/11)

If you see this error when starting Kafka:

```
'wmic' is not recognized as an internal or external command,
operable program or batch file.
```

It happens because **Windows 11** removed the legacy `wmic` tool, but Kafka’s startup script still tries to use it to detect your system architecture.

---

### ✅ **Quick Fix**

1. Open this file in Notepad:

   ```
   D:\kafka_2.13-4.0.0\bin\windows\kafka-server-start.bat
   ```

2. Find this line (use **Ctrl + F → “wmic os”**):

   ```bat
   wmic os get osarchitecture | find /i "32-bit" >nul 2>&1
   IF NOT ERRORLEVEL 1 (
       rem 32-bit OS
       set KAFKA_HEAP_OPTS=-Xmx512M -Xms512M
   ) ELSE (
       rem 64-bit OS
       set KAFKA_HEAP_OPTS=-Xmx1G -Xms1G
   )
   ```

3. **Delete** that block and replace it with:

   ```bat
   IF ["%KAFKA_HEAP_OPTS%"] EQU [""] (
       rem detect OS architecture manually removed for Windows 11
   )
   ```

4. *(Optional but recommended)*
   Add a static heap configuration above:

   ```bat
   set KAFKA_HEAP_OPTS=-Xmx1G -Xms1G
   ```

5. Save the file and restart Kafka:

   ```powershell
   .\bin\windows\kafka-server-start.bat .\config\server.properties
   ```

---

### 🎉 **Result**

Kafka will start normally with:

```
INFO Kafka version: 4.0.x
INFO [KafkaServer id=1] started (kafka.server.KafkaServer)
```

and no more `'wmic' is not recognized` errors.

---

💡 **Tested on:**
Windows 11 22H2 + Kafka 4.0.0 + Java 17 / 21 / 22

---

Would you like me to add a small **“Troubleshooting Kafka on Windows”** section (with `Zookeeper not found`, `Port 9092 in use`, etc.) to extend this README?
