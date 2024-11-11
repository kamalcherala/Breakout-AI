# Email Sender API

Email Sender API is a simple, open-source RESTful API designed for sending customized emails programmatically. This API can be used to automate email notifications, send scheduled greetings, and support any application requiring email functionality. Built with Express.js and configurable SMTP settings, the API is lightweight and ready for cloud deployment.

## 📜 Features

- **Generate and Revoke API Keys**: Each company/user can generate a unique API key for secure access.
- **SMTP Configuration**: Use your SMTP provider's credentials for safe and authenticated email sending.
- **Flexible Email Sending**: Customize sender, recipient, subject, and body content.
- **Daily Scheduler Example**: Set up recurring emails (e.g., a "Good Morning" email sent at 8:00 AM).

## 🛠️ Getting Started

### Prerequisites

- Node.js (v14+ recommended)
- Nodemailer SMTP Account (e.g., Gmail, SendGrid, etc.)
- SQLite (default, but can be configured for a cloud DB if needed)

### Installation

#### Clone the Repository

```bash
git clone https://github.com/kamalcherala/Breakout-AI
cd custom-email-sender
```

#### Install Dependencies

```bash
npm install
```

#### Configure Environment Variables

Create a `.env` file in the root directory with your SMTP and app configurations.

```plaintext
PORT=3000
NODE_ENV=development
SMTP_HOST=smtp.gmail.com
SMTP_PORT=587
SMTP_SECURE=false
SMTP_USER=maheshsharan28@gmail.com
SMTP_PASS=your-app-password  # Get this from Google (see below)
```

**Note**: If you're using Gmail as your SMTP provider, you will need to generate an App Password to use as SMTP_PASS. You can do this by following these steps:

1. Go to your [GoogleAppPassword](https://myaccount.google.com/apppasswords) page.
2. Select Mail as the app and Other (Custom name) for the device.
3. Generate your password and use it as `SMTP_PASS` in the .env file.


This is necessary because Gmail requires 2-Step Verification to allow app-specific passwords for external services.

#### Run Database Migrations

```bash
npx knex migrate:latest
```

#### Start the Server

```bash
npm run dev
```

The server should now be running on http://localhost:3000.

## 🔑 Authentication and API Key Management

### Generate API Key

Each company/user requires an API key for access. The `/auth/keys` endpoint generates a unique key based on a companyId.

#### Request

```http
POST /api/auth/keys
Content-Type: application/json

{
  "companyId": "your-company-id"
}
```

#### Response

```json
{
  "success": true,
  "data": {
    "key": "generated-api-key",
    "companyId": "company-id",
    "createdAt": "timestamp",
    "expiresAt": "timestamp"
  },
  "message": "API key generated successfully"
}
```

## ✉️ Sending an Email

### Request

Use the `/api/messages` endpoint to send an email. Include your API key in the request header for authorization.

```http
POST /api/messages
X-API-Key: your-api-key
Content-Type: application/json

{
  22071a1012@vnrvjiet.in "from": "s",
  "to": "recipient@example.com",
  "subject": "Hello!",
  "content": "This is a test email sent via Email Sender API."
}
```

### Response

```json
{
  "success": true,
  "data": { "sent": true },
  "message": "Message sent successfully"
}
```

If the email fails, a relevant error message will be included in the response.

## 🕒 Example: Scheduled "Good Morning" Email in Python

This example script sends a "Good Morning" email every day at 8:00 AM using the Email Sender API and Python's schedule library.

### Requirements

```bash
pip install requests schedule
```

### Python Script

```python
import requests
import schedule
import time

# API Configuration
API_URL = "http://localhost:3000/api/messages"  # Update to your deployed API URL
API_KEY = "your-api-key"  # Replace with your generated API key

# Email Configuration
FROM_EMAIL = "22071a1012@vnrvjiet.in"
TO_EMAIL = "recipient@example.com"
SUBJECT = "Good Morning!"
CONTENT = "Good morning! Hope you have a wonderful day ahead."

# Function to send the email
def send_morning_email():
    headers = {
        "X-API-Key": API_KEY,
        "Content-Type": "application/json"
    }
    payload = {
        "from": FROM_EMAIL,
        "to": TO_EMAIL,
        "subject": SUBJECT,
        "content": CONTENT
    }

    try:
        response = requests.post(API_URL, json=payload, headers=headers)
        response_data = response.json()
        
        if response.status_code == 200 and response_data.get("success"):
            print("Email sent successfully!")
        else:
            print("Failed to send email:", response_data.get("message", "Unknown error"))
    except Exception as e:
        print("An error occurred:", e)

# Schedule the email to be sent at 8:00 AM every day
schedule.every().day.at("08:00").do(send_morning_email)

print("Email scheduler is running...")

# Keep the script running
while True:
    schedule.run_pending()
    time.sleep(60)  # Check every minute
```

## 🔐 Security

- **Environment Variables**: Store sensitive information such as SMTP credentials and API keys in environment variables.
- **CORS**: Configure CORS to allow only trusted origins to access the API.
- **Input Validation**: The API includes basic input validation to prevent malformed requests.

## 📝 License

This project is licensed under the MIT License - see the LICENSE file for details.

## 🤝 Contributing

We welcome contributions! Please submit a pull request or create an issue for any feature requests or improvements.

## 📞 Support

If you encounter any issues, please open an issue in the GitHub repository or contact us directly at maheshsharan28@gmail.com.
