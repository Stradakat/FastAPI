# FastAPI Issue Tracker

A RESTful API for issue tracking built with FastAPI. This application provides full CRUD operations for managing issues with priority levels, status tracking, and JSON-based persistence.

## Features

- ✅ Full CRUD operations (Create, Read, Update, Delete) for issues
- 🏷️ Issue status tracking (open, in_progress, closed)
- ⚡ Priority levels (low, medium, high)
- 📝 Automatic data validation using Pydantic
- 🕐 Request timing middleware
- 🌐 CORS support for web integration
- 📚 Interactive API documentation (Swagger UI)
- 💾 JSON-based file storage

## Tech Stack

- **FastAPI** - Modern, fast web framework for building APIs
- **Pydantic** - Data validation using Python type annotations
- **Uvicorn** - ASGI server for running the application
- **Python 3.9+** - Programming language

## Installation

### Prerequisites

- Python 3.9 or higher
- pip (Python package manager)

### Setup

1. Clone the repository:
```bash
git clone <repository-url>
cd FastAPI
```

2. Create a virtual environment:
```bash
python3 -m venv .venv
```

3. Activate the virtual environment:
```bash
# On macOS/Linux:
source .venv/bin/activate

# On Windows:
.venv\Scripts\activate
```

4. Install dependencies:
```bash
pip install -r requirements.txt
```

## Running the Application

Start the development server:
```bash
uvicorn main:app --reload
```

The API will be available at:
- **API Base URL**: http://localhost:8000
- **Interactive API Docs (Swagger)**: http://localhost:8000/docs
- **Alternative API Docs (ReDoc)**: http://localhost:8000/redoc

## API Endpoints

All endpoints are prefixed with `/api/v1/issues`

### Get All Issues
```http
GET /api/v1/issues/
```
Returns a list of all issues.

**Response**: `200 OK`
```json
[
  {
    "id": "uuid-string",
    "title": "Issue title",
    "description": "Issue description",
    "priority": "medium",
    "status": "open"
  }
]
```

### Get Issue by ID
```http
GET /api/v1/issues/{issue_id}
```
Returns a specific issue by its ID.

**Parameters**:
- `issue_id` (string): The unique identifier of the issue

**Response**: `200 OK` or `404 Not Found`

### Create Issue
```http
POST /api/v1/issues/
```
Creates a new issue.

**Request Body**:
```json
{
  "title": "Fix authentication bug",
  "description": "Users cannot log in with their credentials",
  "priority": "high"
}
```

**Validation Rules**:
- `title`: 3-100 characters
- `description`: 5-1000 characters
- `priority`: "low", "medium", or "high" (defaults to "medium")

**Response**: `201 Created`

### Update Issue
```http
PUT /api/v1/issues/{issue_id}
```
Updates an existing issue. All fields are optional.

**Request Body**:
```json
{
  "title": "Updated title",
  "description": "Updated description",
  "priority": "high",
  "status": "in_progress"
}
```

**Response**: `200 OK` or `404 Not Found`

### Delete Issue
```http
DELETE /api/v1/issues/{issue_id}
```
Deletes an issue by its ID.

**Response**: `204 No Content` or `404 Not Found`

## Example Usage

### Using cURL

**Create an issue**:
```bash
curl -X POST "http://localhost:8000/api/v1/issues/" \
  -H "Content-Type: application/json" \
  -d '{
    "title": "Fix bug in authentication",
    "description": "Users cannot log in with their credentials",
    "priority": "high"
  }'
```

**Get all issues**:
```bash
curl http://localhost:8000/api/v1/issues/
```

**Get a specific issue**:
```bash
curl http://localhost:8000/api/v1/issues/{issue_id}
```

**Update an issue**:
```bash
curl -X PUT "http://localhost:8000/api/v1/issues/{issue_id}" \
  -H "Content-Type: application/json" \
  -d '{
    "status": "in_progress",
    "priority": "high"
  }'
```

**Delete an issue**:
```bash
curl -X DELETE "http://localhost:8000/api/v1/issues/{issue_id}"
```

### Using Python

```python
import requests

# Create an issue
response = requests.post(
    "http://localhost:8000/api/v1/issues/",
    json={
        "title": "Fix authentication bug",
        "description": "Users cannot log in",
        "priority": "high"
    }
)
issue = response.json()

# Get all issues
issues = requests.get("http://localhost:8000/api/v1/issues/").json()

# Update an issue
requests.put(
    f"http://localhost:8000/api/v1/issues/{issue['id']}",
    json={"status": "in_progress"}
)
```

## Project Structure

```
FastAPI/
├── app/
│   ├── __init__.py
│   ├── schemas.py          # Pydantic models for data validation
│   ├── storage.py          # JSON file storage utilities
│   ├── middleware/
│   │   └── timer.py        # Request timing middleware
│   └── routes/
│       ├── __init__.py
│       └── issues.py       # Issue CRUD endpoints
├── data/
│   └── issues.json         # JSON data storage (auto-created)
├── main.py                 # FastAPI application entry point
├── requirements.txt        # Python dependencies
└── README.md              # This file
```

## Data Models

### Issue Status
- `open` - Issue is newly created
- `in_progress` - Issue is being worked on
- `closed` - Issue is resolved

### Issue Priority
- `low` - Low priority issue
- `medium` - Medium priority issue (default)
- `high` - High priority issue

## Development

### Running in Development Mode

The `--reload` flag enables auto-reload on code changes:
```bash
uvicorn main:app --reload
```

### Data Storage

Issues are stored in `data/issues.json`. This file is automatically created when the first issue is saved.

## License

This project is open source and available under the MIT License.
