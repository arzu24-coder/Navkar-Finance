# Navkar Finance CRM API

A comprehensive Customer Relationship Management (CRM) system built with FastAPI, designed for Navkar Finance to manage leads, customers, employees, and business operations.

## 📋 Table of Contents

- [Features](#-features)
- [Technology Stack](#-technology-stack)
- [Project Structure](#-project-structure)
- [Installation](#-installation)
- [Configuration](#-configuration)
- [Running the Application](#-running-the-application)
- [API Documentation](#-api-documentation)
- [Authentication](#-authentication)
- [Integrations](#-integrations)
- [Development](#-development)
- [Contributing](#-contributing)

## ✨ Features

### Core CRM Features
- **Lead Management**: Track and manage leads from multiple sources
- **Customer Management**: Comprehensive customer data management
- **Follow-up System**: Automated and manual follow-up scheduling
- **Quotation Management**: Generate and manage quotes
- **Payment Tracking**: Monitor payments and expenses

### Employee Management
- **Employee Records**: Complete employee information management
- **Attendance System**: Track employee attendance and working hours
- **Leave Management**: Handle leave requests and approvals
- **HR Management**: Staff management and reporting

### Advanced Features
- **Role-Based Access Control**: Secure user permissions and roles
- **Multi-channel Lead Integration**: Google Sheets, Meta Ads, phone leads
- **Document Management**: File upload and document handling
- **Reporting System**: Comprehensive business reports
- **Task Management**: Assign and track tasks
- **Franchise Management**: Handle franchise operations
- **Stock Management**: Inventory tracking and management

### Communication & Notifications
- **Email Integration**: Gmail API integration for email management
- **SMS Integration**: Twilio integration for SMS notifications
- **Automated Reminders**: Background task notifications
- **Announcements**: Internal communication system

## 🛠️ Technology Stack

- **Backend**: FastAPI (Python)
- **Database**: MongoDB (with Motor for async operations)
- **Authentication**: JWT tokens with refresh mechanism
- **Task Queue**: Background task processing
- **Email**: Gmail API integration
- **SMS**: Twilio integration
- **File Storage**: Local/Google Drive integration
- **Documentation**: Auto-generated OpenAPI/Swagger docs

## 📁 Project Structure

```
app/
├── __init__.py                 # FastAPI application setup
├── __main__.py                 # Application entry point
├── config.py                   # Configuration settings
├── dependencies.py             # Dependency injection utilities
├── make_call.py               # Call management utilities
├── background/                # Background task processors
│   └── remainder_notifier.py
├── core/                      # Core application logic
│   └── security.py
├── credentials/               # API credentials and service accounts
│   ├── google_service_account.json
│   └── leads.csv
├── database/                  # Database layer
│   ├── async_db.py           # Database connection
│   ├── init_db.py            # Database initialization
│   ├── integration/          # External service integrations
│   ├── repositories/         # Data access layer
│   └── schemas/              # Data validation schemas
├── models/                    # Pydantic models
├── routes/                    # API route handlers
├── services/                  # Business logic layer
├── static/                    # Static files and frontend
├── utils/                     # Utility functions
└── .env                       # Environment variables
```

## 🚀 Installation

### Prerequisites

- Python 3.8 or higher
- MongoDB database
- Google Service Account (for Google Sheets/Drive integration)
- Twilio Account (for SMS features)
- Gmail API credentials (for email integration)

### Setup Steps

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd app
   ```

2. **Create virtual environment**
   ```bash
   python -m venv venv
   
   # On Windows
   venv\Scripts\activate
   
   # On macOS/Linux
   source venv/bin/activate
   ```

3. **Install dependencies**
   ```bash
   pip install fastapi uvicorn motor pymongo python-jose[cryptography] passlib[bcrypt] python-multipart pydantic-settings twilio google-api-python-client google-auth-httplib2 google-auth-oauthlib fastapi-mail python-dotenv
   ```

## ⚙️ Configuration

### Environment Variables

Create a `.env` file in the root directory with the following variables:

```env
# Database Configuration
DATABASE_URL=mongodb+srv://username:password@cluster.mongodb.net/?retryWrites=true&w=majority
DB_NAME=my_crm

# Security Configuration
SECRET_KEY=your-secret-key-here
ACCESS_TOKEN_EXPIRE_MINUTES=10080
REFRESH_TOKEN_EXPIRE_MINUTES=30
ALGORITHM=HS256

# Twilio Configuration
TWILIO_ACCOUNT_SID=your_twilio_account_sid
TWILIO_AUTH_TOKEN=your_twilio_auth_token

# Google API Configuration
GOOGLE_SERVICE_ACCOUNT_FILE=app/credentials/google_service_account.json

# Meta Ads API Configuration
META_APP_ID=your_meta_app_id
META_APP_SECRET=your_meta_app_secret
META_WEBHOOK_VERIFY_TOKEN=your_webhook_verify_token

# Email Configuration
SMTP_USERNAME=your_email@gmail.com
SMTP_PASSWORD=your_app_password
IMAP_USERNAME=your_email@gmail.com
IMAP_PASSWORD=your_app_password

# Gmail API Configuration
GMAIL_CLIENT_ID=your_gmail_client_id
GMAIL_CLIENT_SECRET=your_gmail_client_secret
GMAIL_REDIRECT_URI=http://localhost:8005/api/auth/google/callback
```

### Google Service Account Setup

1. Create a Google Cloud Project
2. Enable Google Sheets and Google Drive APIs
3. Create a service account and download the JSON credentials
4. Place the credentials file in `credentials/google_service_account.json`
5. Share your Google Sheets with the service account email

### Database Setup

The application uses MongoDB. The database will be automatically initialized on first run.

## 🏃‍♂️ Running the Application

### Development Mode

```bash
python -m app
```

Or using uvicorn directly:

```bash
uvicorn app:app --host 0.0.0.0 --port 8005 --reload
```

### Production Mode

```bash
python -m app
```

The application will start on `http://localhost:8005`

## 📚 API Documentation

Once the application is running, you can access:

- **Swagger UI**: `http://localhost:8005/docs`
- **ReDoc**: `http://localhost:8005/redoc`
- **OpenAPI JSON**: `http://localhost:8005/openapi.json`

### Main API Endpoints

- **Authentication**: `/api/auth/`
- **Users**: `/api/users/`
- **Leads**: `/api/leads/`
- **Customers**: `/api/customers/`
- **Employees**: `/api/employees/`
- **Follow-ups**: `/api/followups/`
- **Quotations**: `/api/quotations/`
- **Reports**: `/api/reports/`

## 🔐 Authentication

The application uses JWT-based authentication with:

- **Access Tokens**: Short-lived tokens (7 days by default)
- **Refresh Tokens**: Long-lived tokens (30 days by default)
- **Role-Based Permissions**: Fine-grained access control

### Login Process

1. POST to `/api/auth/login` with credentials
2. Receive access and refresh tokens
3. Include access token in `Authorization: Bearer <token>` header
4. Use refresh token to get new access tokens when expired

## 🔗 Integrations

### Google Sheets
- Automatic lead import from Google Sheets
- Real-time synchronization
- Configurable column mapping

### Meta Ads
- Lead capture from Facebook/Instagram ads
- Webhook integration for real-time leads
- Campaign performance tracking

### Gmail
- Send emails directly from the CRM
- Email template management
- Automatic email logging

### Twilio
- SMS notifications and alerts
- Two-way SMS communication
- Call logging and tracking

## 👨‍💻 Development

### Code Structure

- **Routes**: Handle HTTP requests and responses
- **Services**: Business logic and processing
- **Repositories**: Data access and database operations
- **Models**: Data validation and serialization
- **Utils**: Helper functions and utilities

### Adding New Features

1. Create appropriate models in `models/`
2. Add database schemas in `database/schemas/`
3. Create repository in `database/repositories/`
4. Implement business logic in `services/`
5. Create API routes in `routes/`
6. Update dependencies and permissions as needed

### Database Operations

The application uses Motor for async MongoDB operations:

```python
from app.database.async_db import get_async_database

async def example_function():
    db = await get_async_database()
    collection = db["collection_name"]
    result = await collection.find_one({"field": "value"})
```

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/new-feature`)
3. Commit your changes (`git commit -am 'Add new feature'`)
4. Push to the branch (`git push origin feature/new-feature`)
5. Create a Pull Request

### Code Standards

- Follow PEP 8 style guidelines
- Use type hints for all functions
- Add docstrings for public methods
- Write unit tests for new features
- Update documentation as needed

## 📝 License

This project is proprietary software developed for Navkar Finance.

## 🆘 Support

For support and questions, please contact the development team or create an issue in the project repository.

---

**Version**: 1.0.0  
**Last Updated**: November 2024  
**Developed for**: Navkar Finance
