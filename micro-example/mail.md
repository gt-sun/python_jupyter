

### 简单发送文本mail

```python

# 腾讯企业邮箱

from email.mime.text import MIMEText
import smtplib
sender = "notify@qq.com"
receiver = ["wqsun@qq.com"]
host = 'smtp.exmail.qq.com'
passwd = 'zO6******u6'
port = 465
content = "test"
msg = MIMEText(content)
msg['From'] = "notify@qq.com"
msg['To'] =';'.join(receiver)
msg['Subject'] = "vps警报"
try:
    smtp = smtplib.SMTP_SSL(host, port)
    smtp.login(sender, passwd)
    smtp.sendmail(sender, receiver, msg.as_string())
except Exception:
    print("error")
```