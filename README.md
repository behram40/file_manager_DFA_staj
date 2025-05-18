# File Management Web Application

A secure web application that allows users to manage their files (PDF, PNG, JPG) with user authentication.

## Features

- User registration and login with JWT authentication
- Secure file upload and management
- Support for PDF, PNG, and JPG files
- User-specific file storage
- File listing and deletion
- Bootstrap-based responsive UI

## Project Structure

```
file_manager/
├── app.py              # Main Flask application
├── database.py         # Database models and setup
├── requirements.txt    # Python dependencies
├── uploads/           # Directory for uploaded files
├── static/            # Static files (CSS, JS)
└── templates/         # HTML templates
    ├── base.html      # Base template
    ├── login.html     # Login page
    ├── register.html  # Registration page
    └── dashboard.html # File management dashboard
```

## Setup Instructions

1. Create a virtual environment:
   ```bash
   python -m venv venv
   ```

2. Activate the virtual environment:
   - Windows:
     ```bash
     .\venv\Scripts\activate
     ```
   - Unix/MacOS:
     ```bash
     source venv/bin/activate
     ```

3. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```

4. Initialize the database:
   ```bash
   python
   >>> from database import init_db
   >>> init_db()
   >>> exit()
   ```

5. Run the application:
   ```bash
   python app.py
   ```

6. Open your browser and navigate to:
   ```
   http://localhost:5000
   ```

## Security Features

- JWT-based authentication
- Password hashing
- User-specific file access
- File type validation
- Secure file storage

## File Types Supported

- PDF (.pdf)
- PNG (.png)
- JPG/JPEG (.jpg, .jpeg)

## Note

This application runs locally and does not require any external services or Docker. All data is stored in a SQLite database and files are stored in the local filesystem. 