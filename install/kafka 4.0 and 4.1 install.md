Here’s your documentation rewritten with a **professional GitHub-README look**, clean markdown formatting, syntax highlighting, and emoji icons — perfect for a repository README.md 👇

---

# ⚙️ Kafka Initial Setup (Windows 11 / KRaft Mode)

> 🧠 **Note:** This guide sets up Apache Kafka in **KRaft mode** (no Zookeeper needed).
> Tested on: **Windows 11 + Kafka 4.0.x + Java 17 / 21 / 22**

---

## 🚀 1. Configure `server.properties`

Open:

```bash
D:\ApacheKafka\kafka\config\server.properties
```

Change the log directory path:

```properties
# Old:
# log.dirs=/tmp/kraft-combined-logs

# New:
log.dirs=D:/ApacheKafka/kafka/logs
```

---

## 📁 2. Create Logs Folder

If it doesn’t exist, create:

```
D:\ApacheKafka\kafka\logs
```

---

## 🗝️ 3. Set Controller Voter

Add this line (below `node.id=1`):

```properties
controller.quorum.voters=1@localhost:9093
```

> ℹ️ **Explanation:**
>
> * `1` = node ID
> * `localhost:9093` = controller listener port

---

## 🔑 4. Generate Cluster UUID

Run this in PowerShell:

```bash
.\bin\windows\kafka-storage.bat random-uuid
```

Copy the generated UUID.

---

## 🧹 5. Format Kafka Storage

Replace `<UUID>` with the one you copied:

```bash
.\bin\windows\kafka-storage.bat format -t <UUID> -c .\config\server.properties --ignore-formatted
```

---

## 🧩 6. Start Kafka Server (KRaft Mode)

```bash
.\bin\windows\kafka-server-start.bat .\config\server.properties
```

🎉 You should see:

```
INFO Kafka version: 4.0.x
INFO [KafkaServer id=1] started (kafka.server.KafkaServer)
```

---

# 💬 Create Topic, Producer & Consumer

## 🧱 7. Create a Kafka Topic

```bash
.\bin\windows\kafka-topics.bat --create --topic my-topic --bootstrap-server localhost:9092
```

---

## ✉️ 8. Start a Producer

*(Open a new CMD window)*

```bash
.\bin\windows\kafka-console-producer.bat --topic my-topic --bootstrap-server localhost:9092
```

---

## 👂 9. Start a Consumer

*(Open another CMD window)*

```bash
.\bin\windows\kafka-console-consumer.bat --topic my-topic --from-beginning --bootstrap-server localhost:9092
```

---

# ⚡ Advanced Topic Management (KRaft Mode)

## 🧠 10. Create Topic with Advanced Config

```bash
.\bin\windows\kafka-topics.bat --create --topic my-topic --partitions 1 --replication-factor 1 --bootstrap-server localhost:9092
```

**Explanation:**

| Flag                   | Description                                        |
| ---------------------- | -------------------------------------------------- |
| `--partitions`         | Number of partitions (logical shards), usually 1–3 |
| `--replication-factor` | Keep `1` for single-node KRaft setup               |

---

## 🔍 11. Describe a Topic

```bash
.\bin\windows\kafka-topics.bat --describe --topic my-topic --bootstrap-server localhost:9092
```

---

## 📜 12. List All Topics

```bash
.\bin\windows\kafka-topics.bat --list --bootstrap-server localhost:9092
```

---

## 🗑️ 13. Delete a Topic

```bash
.\bin\windows\kafka-topics.bat --delete --topic my-topic --bootstrap-server localhost:9092
```

> 🧩 Make sure deletion is enabled in `server.properties`:
>
> ```properties
> delete.topic.enable=true
> ```

---

## ➕ 14. Increase Topic Partitions

(You can **only increase**, not decrease.)

```bash
.\bin\windows\kafka-topics.bat --alter --topic my-topic --partitions 3 --bootstrap-server localhost:9092
```

---

# 🧰 Bonus Tip: Fix `'wmic' is not recognized'` on Windows 11

If you see this error during Kafka startup:

```
'wmic' is not recognized as an internal or external command
```

👉 Open:

```
D:\ApacheKafka\kafka\bin\windows\kafka-server-start.bat
```

Find and remove:

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

Replace with:

```bat
IF ["%KAFKA_HEAP_OPTS%"] EQU [""] (
    rem detect OS architecture manually removed for Windows 11
)
```

✅ Restart Kafka — the error is gone!

---

### 🧡 Author

**Md. Saddam Hossen**
Full-Stack Java Developer • Spring Boot + Kafka + WebFlux
🌐 [github.com/saddamhdev](https://github.com/saddamhdev)

---

Would you like me to add **code-block icons** (🚀, 🧠, ⚡) and a **“Quick Commands Summary” table** at the top for a more polished GitHub README style?
