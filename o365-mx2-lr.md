```mermaid
---
title: Deliver email to the junk email folder and user managed quarantine
---
graph LR
Incoming[Incoming<br>Email] --> MALICIOUS & SPAM & SUSPICIOUS & BENIGN
subgraph Cloudflare Area 1 Email Security
subgraph Dispositions
MALICIOUS
SUSPICIOUS["SUSPICIOUS<br>---------------<br>BULK"]
SPAM["SPAM<br>---------------<br>SPOOF"]
BENIGN
end
Admin[Admin<br>Quarantine]
MALICIOUS --> |Quarantine<br>Policy|Admin
end
subgraph Office 365
MailFlow1["Mail flow rule #1<br>(Transport rule)"]
SUSPICIOUS --> |X-Area1Security-Disposition<br>message header|MailFlow1
MailFlow2["Mail flow rule #2<br>(Transport rule)"]
SPAM --> |X-Area1Security-Disposition<br>message header|MailFlow2
subgraph "Anti-spam<br>policy"
Spam["Spam<br>(SCL = 5, 6)"]
Hcspam["High<br>confidence<br>spam<br>(SCL = 7, 8, 9)"]
Phishing["Phishing<br>---------------<br>High<br>confidence<br>phishing"]
end
MailFlow1 --> |"Set SCL to 5<br>(Spam Confidence Level)"|Spam
MailFlow2 --> |"Set SCL to 9<br>(Spam Confidence Level)"|Hcspam
Defender[Administrative<br>Quarantine]
User[User managed<br>Quarantine]
Junk[Junk<br>Email<br>Folder]
Inbox
end
Spam ---> |Move messages to Junk Email folder|Junk
Hcspam --> |"Quarantine<br>Policy #1"|User
User -.-> |User Notification<br>+<br>User Release|Inbox
Phishing --> |"Quarantine<br>Policy #2"|Defender
Defender -.-> |User Notification<br>+<br>Admin Release|Inbox
BENIGN ----> Spam & Hcspam & Phishing & Inbox
```
