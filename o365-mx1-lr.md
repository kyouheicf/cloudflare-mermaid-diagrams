```mermaid
---
title: Deliver email to the O365 junk email folder
---
graph LR
Incoming[Incoming<br>Email] --> MALICIOUS & SUSPICIOUS & BENIGN
subgraph Cloudflare Area 1 Email Security
subgraph Dispositions
MALICIOUS["MALICIOUS<br>---------------<br>SPAM<br>---------------<br>SPOOF"]
SUSPICIOUS["SUSPICIOUS<br>---------------<br>BULK"]
BENIGN
end
Admin[Admin<br>Quarantine]
MALICIOUS --> |Quarantine<br>Policy|Admin
end
subgraph Office 365
MailFlow["Mail flow rule<br>(Transport rule)"]
Junk[Junk<br>Email<br>Folder]
subgraph "Anti-spam<br>policy"
Spam["Spam<br>(SCL = 5, 6)"]
Hcspam["High<br>confidence<br>spam<br>(SCL = 7, 8, 9)<br>---------------<br>Phishing<br>---------------<br>High<br>confidence<br>phishing"]
end
Defender[Administrative<br>Quarantine]
Inbox
end
SUSPICIOUS ---> |X-Area1Security-Disposition<br>message header|MailFlow
BENIGN ---> Spam & Hcspam
BENIGN ---> Inbox
MailFlow --> |"Set SCL to 5<br>(Spam Confidence Level)"|Spam
Spam ---> |Move messages to Junk Email folder|Junk
Hcspam --> |Quarantine<br>Policy|Defender
Defender -.-> |User Notification<br>+<br>Admin Release|Inbox
```
