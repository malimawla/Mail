import smtplib
from email.mime.multipart import MIMEMultipart
from email.mime.text import MIMEText

SMTP_SERVER = "smtp.gmail.com"
SMTP_PORT = 587
EMAIL_ADDRESS = "sociale.fam@gmail.com"
EMAIL_PASSWORD = "wcph mpju kyyv vena"

TO_NAME = "Recipient Name"
TO_EMAIL = "recipient@example.com"

LOGO_URL = "https://i.imgur.com/XgpHNMY.jpeg"
IMG_URL = "https://i.imgur.com/D9OXFON.jpeg"

subject = "Thank you for attending sociale's first event!"

body_template = f"""
<html>
  <body style="margin:0; padding:0; background: linear-gradient(135deg,#1a1124 0%,#2b183d 100%);
    font-family: 'Inter', Arial, sans-serif; color:#fff;">
    <table align="center" cellpadding="0" cellspacing="0" width="100%"
      style="max-width:600px; margin:auto; background:#20122b; border-radius:18px;
      border:1px solid #472065;">
      <!-- Logo Centered -->
      <tr>
        <td style="padding:20px 40px 0 40px; text-align:center;">
          <img src="{LOGO_URL}" alt="Logo" width="120" style="display:block; max-width:120px; height:auto; margin:auto;" />
        </td>
      </tr>
      <!-- Main Event Image full width center -->
      <tr>
        <td style="padding:20px 40px 20px 40px; text-align:center;">
          <img src="{IMG_URL}" alt="Sociale Event" width="100%" style="max-width:560px; height:auto; border-radius:16px; display:block; margin:auto;" />
        </td>
      </tr>
      <!-- Greeting -->
      <tr>
        <td style="padding:10px 40px 10px 40px; text-align:left;">
          <p style="font-size:16px; line-height:1.5; margin:14px 0 0 0; color:#fff;">
            Dear {{name}},
          </p>
        </td>
      </tr>
      <!-- Body text and button -->
      <tr>
        <td style="padding:0 40px 40px 40px; text-align:center;">
          <p style="font-size:16px; line-height:1.5; margin:14px 0; color:#fff;">
            Thank you for making our very first <strong>sociale</strong> event unforgettable.
          </p>
          <p style="font-size:16px; line-height:1.5; margin:14px 0; color:#fff;">
            As a thank-you, you’ll get priority access when tickets drop for our next event.
          </p>
          <p style="font-size:16px; line-height:1.5; margin:14px 0 30px 0; color:#fff;">
            Photos and videos are coming soon on IG, so make sure to follow us to relive the night and stay in the loop.
          </p>
          <!-- Follow Instagram Button -->
          <p style="margin:32px 0;">
            <a href="https://instagram.com/sociale.ch" target="_blank"
              style="background: linear-gradient(90deg, #ea83ff 0%, #d366fd 100%);
                color: #fff; font-weight: 600; font-size: 16px; padding: 14px 40px;
                border-radius: 38px; text-decoration: none; display: inline-block;
                box-shadow: 0 6px 14px #d366fd; cursor: pointer; user-select: none;">
              👉 Follow us on Instagram
            </a>
          </p>
          <p style="font-size:16px; line-height:1.5; margin:14px 0; color:#fff;">
            We also know the queue was long — we’ll make it smoother next time.
          </p>
          <!-- Signature left aligned & white no bold -->
          <p style="font-size:16px; line-height:1.5; color:#fff; text-align:left; margin-top:30px;">
            See you soon,<br>
            The sociale team
          </p>
        </td>
      </tr>
      <tr>
        <td align="center" style="padding:20px; background:#1a1124;
          border-bottom-left-radius:18px; border-bottom-right-radius:18px;
          font-size:14px; color:#a081d3;">
          &copy; 2025 Sociale Fam
        </td>
      </tr>
    </table>
  </body>
</html>
"""

msg = MIMEMultipart("alternative")
msg["Subject"] = subject
msg["From"] = EMAIL_ADDRESS
msg["To"] = TO_EMAIL

html_body = body_template.format(name=TO_NAME if TO_NAME else "Guest")
msg.attach(MIMEText(html_body, "html"))

server = smtplib.SMTP(SMTP_SERVER, SMTP_PORT)
server.ehlo()
server.starttls()
server.login(EMAIL_ADDRESS, EMAIL_PASSWORD)
try:
    server.sendmail(EMAIL_ADDRESS, TO_EMAIL, msg.as_string())
    print(f"✅ Email sent to {TO_NAME} at {TO_EMAIL}")
except Exception as e:
    print(f"❌ Failed to send to {TO_EMAIL}: {e}")
server.quit()
print("🎉 Email send process completed!")
