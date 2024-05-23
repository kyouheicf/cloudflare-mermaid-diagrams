```mermaid
---
title: Deliver email to the junk email folder and user managed quarantine
---
graph TB
Incoming[Incoming Email] --> MALICIOUS & SPAM & SUSPICIOUS & BENIGN
subgraph Cloudflare Area 1 Email Security
subgraph Dispositions
MALICIOUS
SUSPICIOUS["SUSPICIOUS<br>---------------<br>BULK"]
SPAM["SPAM<br>---------------<br>SPOOF"]
BENIGN
end
Admin[Admin Quarantine]
MALICIOUS --> |Quarantine Policy|Admin
end
subgraph Office 365
MailFlow1["Mail flow rule #1<br>(Transport rule)"]
SUSPICIOUS --> |X-Area1Security-Disposition<br>message header|MailFlow1
MailFlow2["Mail flow rule #2<br>(Transport rule)"]
SPAM --> |X-Area1Security-Disposition<br>message header|MailFlow2
subgraph Anti-spam policy
Spam["Spam<br>(SCL = 5, 6)"]
Hcspam["High confidence spam<br>(SCL = 7, 8, 9)"]
Phishing["Phishing<br>------------------------------<br>High confidence phishing"]
end
MailFlow1 --> |"Set SCL to 5<br>(Spam Confidence Level)"|Spam
MailFlow2 --> |"Set SCL to 9<br>(Spam Confidence Level)"|Hcspam
Defender[Administrative Quarantine]
User[User-managed Quarantine]
Junk[Junk Email]
Inbox
end
Spam ---> |Move messages to Junk Email folder|Junk
Hcspam --> |"Quarantine Policy #1"|User
User -.-> |User Notification<br>+<br>User Release|Inbox
Phishing --> |"Quarantine Policy #2"|Defender
Defender -.-> |User Notification<br>+<br>Admin Release|Inbox
BENIGN ----> Spam & Hcspam & Phishing & Inbox
```
