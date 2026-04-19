
## API Collection Overview

This Postman collection is designed to help developers and QA engineers test the Yosuite platform APIs efficiently. The collection includes:

- **Authentication Flow**: Secure login with automatic token management
- **Employee Management**: Create and onboard new employees
- **Announcements**: Create company-wide announcements
- **Awards Management**: Create and manage employee awards

### Key Features

- Auto-generated dynamic test data
- Automatic token persistence across requests
- Comprehensive test assertions for each endpoint
- Pre-request scripts for data generation
- Ready-to-use request templates

---

## Project Structure

```
taskAPi/
├── apiCollection.postman_collection.json  # Main Postman collection
├── README.md                               # Documentation (this file)
└── screenshots/                            # API response screenshots (Link)
```

### File Descriptions

| File | Description |
|------|-------------|
| `apiCollection.postman_collection.json` | Postman collection with all API requests, tests, and pre-request scripts |
| `README.md` | Comprehensive documentation for the API collection |
| `screenshots/` | [Link](https://drive.google.com/drive/u/0/folders/1g5vkvPtBsW2bTrxElP4Fmo-JK377xtX7) with API test screenshots |

---

## Screenshots

View API test screenshots and results:

**[Link - API Test Screenshots](https://drive.google.com/drive/u/0/folders/1g5vkvPtBsW2bTrxElP4Fmo-JK377xtX7)**

The folder contains:
- Authentication response screenshots
- Employee creation results
- Announcement creation results
- Award creation results
- Error handling examples

---

## Prerequisites

Before using this collection, ensure you have:

| Requirement | Version | Download Link |
|-------------|---------|---------------|
| Postman Desktop App | v10.0+ | [Download](https://www.postman.com/downloads/) |
| Valid Yosuite Account | N/A | Contact administrator |
| API Credentials | N/A | Email & Password |

### System Requirements

- **Windows**: Windows 10/11 (64-bit)

---

## Installation

### Step 1: Install Postman

1. Download Postman from [https://www.postman.com/downloads/](https://www.postman.com/downloads/)
2. Run the installer and follow the setup wizard
3. Launch Postman after installation

### Step 2: Import Collection

**Method A: Using File Import**

1. Open Postman
2. Click the **Import** button (top-left corner)
3. Select **File** tab
4. Click **Upload Files**
5. Navigate to `apiCollection.postman_collection.json`
6. Click **Import**

**Method B: Using Drag & Drop**

1. Open Postman
2. Drag `apiCollection.postman_collection.json` directly into the Postman window
3. The collection will be imported automatically

### Step 3: Create Environment (Optional but Recommended)

1. Click the **Environments** tab on the left sidebar
2. Click **+** to create a new environment
3. Name it `Yosuite-Dev`
4. Add variables:

| Variable | Initial Value | Current Value |
|----------|---------------|---------------|
| `baseUrl` | `https://api-backend.yosuite.net` | `https://api-backend.yosuite.net` |
| `authUrl` | `https://api-user-account.yosuite.net` | `https://api-user-account.yosuite.net` |

---

## Quick Start

### 5-Minute Setup Guide

```
Step 1: Import Collection    → Import JSON file
Step 2: Authenticate         → Run auth request
Step 3: Test Endpoints       → Run any other request
```

### Detailed Quick Start

1. **Import the Collection**
   - Follow the [Installation](#installation) steps above
   - Verify the collection appears in your workspace

2. **Authenticate**
   - Navigate to `apiCollection` → `auth`
   - Update the email and password in the request body
   - Click **Send**
   - Verify status code `200 OK`
   - Token is automatically saved

3. **Test Other Endpoints**
   - Select any request (createEmployee, announcementCreation, awardCreation)
   - The `{{token}}` variable is automatically used
   - Click **Send** and verify response

---

## API Endpoints

### 1. Authentication

Authenticate with the API and retrieve an access token for subsequent requests.

#### Request Details

| Property | Value |
|----------|-------|
| **Method** | `POST` |
| **Endpoint** | `https://api-user-account.yosuite.net/api/auth` |
| **Content-Type** | `application/json` |
| **Authorization** | None (public endpoint) |

#### Request Headers

| Header | Value | Required |
|--------|-------|----------|
| `accept` | `*/*` | No |
| `content-type` | `application/json` | Yes |
| `origin` | `https://vantara.yosuite.net` | No |

#### Request Body

```json
{
    "email": "your-email@example.com",
    "password": "your-password"
}
```

#### Request Body Parameters

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `email` | string | Yes | Valid email address registered in the system |
| `password` | string | Yes | Account password (minimum 8 characters) |

#### Success Response (200 OK)

```json
{
    "data": {
        "token": "AES-eyJjaXBoZXJ0ZXh0IjoiTmZ1c3B6VFpMNS9JZld5T1JZd0VFZW82UU5FYUdRWVJveGZHQjYrMzdlVnU
    }
}
```

#### Response Fields

| Field | Type | Description |
|-------|------|-------------|
| `data.token` | string | AES-encrypted bearer token for API authentication |

#### Error Responses

| Status Code | Description | Example Response |
|-------------|-------------|------------------|
| `401` | Invalid credentials | `{"error": "Invalid email or password"}` |
| `400` | Missing required fields | `{"error": "Email and password are required"}` |
| `429` | Too many attempts | `{"error": "Too many login attempts"}` |

#### Automated Behavior

- **Token Extraction**: The token is automatically extracted and saved to the `{{token}}` collection variable
- **Test Assertion**: Verifies status code is `200`

---

### 2. Create Employee

Create a new employee record with comprehensive onboarding information.

#### Request Details

| Property | Value |
|----------|-------|
| **Method** | `POST` |
| **Endpoint** | `https://api-backend.yosuite.net/component/modules/people/employee/e-onboarding-steps/form-component/store-content` |
| **Content-Type** | `multipart/form-data` |
| **Authorization** | `Bearer {{token}}` |

#### Request Headers

| Header | Value | Required |
|--------|-------|----------|
| `authorization` | `Bearer {{token}}` | Yes |
| `content-type` | `multipart/form-data` | Yes |
| `accept` | `*/*` | No |

#### Request Body (form-data)

The request body is sent as `multipart/form-data` with a single field named `data` containing a JSON string.

```json
{
    "first_name": "John",
    "middle_name": "William",
    "last_name": "Doe",
    "email": "john.doe@example.com",
    "phone": "8800TeestMSDN",
    "staff_id": "EMP001",
    "address_group": {
        "present_address": {
            "street_1": "123 Main Street",
            "street_2": "Apt 4B",
            "country": "United States",
            "state": "California",
            "city": "Los Angeles",
            "zip": "90001"
        },
        "permanent_address": {
            "same_as_present_address": true
        }
    },
    "employee_department_option_id": "69391644740acf1e0c0373e7",
    "employee_job_location_option_id": "695dde14f97ee294810d9823",
    "employee_status_option_id": "692fdff8397c6c094bfb44a5",
    "employee_gender_option_id": "692fdffb397c6c094bfb44cd",
    "employee_job_type_option_id": "692fdff8397c6c094bfb44ab",
    "employee_job_title_option_id": "695dd83f49eaf632130950b3",
    "manager_id": "69ad216652b0b8a3d20a9eb2",
    "who_to_contact_id": "69ad214f52b0b8a3d20a9ea6",
    "shift_option_id": "692fe275f348e6963301e216",
    "social_security_no": "SSN12345",
    "work_function": "remote",
    "date_of_birth": "1995-05-15",
    "hire_date": "2026-04-19",
    "joining_date": "2026-04-19",
    "employer_tasks": null,
    "joiner_tasks": null,
    "joiner_questions": {
        "695ddd1ce35026b82201d8b3": {
            "0": true,
            "1": true
        },
        "697b3ceef1f074b159047766": {
            "0": true
        }
    }
}
```

#### Request Body Parameters

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `first_name` | string | Yes | Employee's first name (2-50 characters) |
| `middle_name` | string | No | Employee's middle name |
| `last_name` | string | Yes | Employee's last name (2-50 characters) |
| `email` | string | Yes | Valid email address (must be unique) |
| `phone` | string | Yes | Phone number with country code (e.g., 8801...) |
| `staff_id` | string | Yes | Unique employee/staff identifier |
| `address_group` | object | Yes | Address information object |
| `address_group.present_address` | object | Yes | Current residential address |
| `address_group.permanent_address` | object | Yes | Permanent address (can use `same_as_present_address: true`) |
| `employee_department_option_id` | string | Yes | Department ID from system options |
| `employee_job_location_option_id` | string | Yes | Job location ID from system options |
| `employee_status_option_id` | string | Yes | Employment status ID (active, probation, etc.) |
| `employee_gender_option_id` | string | Yes | Gender option ID |
| `employee_job_type_option_id` | string | Yes | Job type ID (full-time, part-time, contract) |
| `employee_job_title_option_id` | string | Yes | Job title ID |
| `manager_id` | string | No | Direct manager's employee ID |
| `who_to_contact_id` | string | No | Emergency contact reference ID |
| `shift_option_id` | string | No | Work shift option ID |
| `social_security_no` | string | No | Social security number |
| `work_function` | string | No | Work arrangement: `remote`, `onsite`, `hybrid` |
| `date_of_birth` | string | Yes | Birth date in `YYYY-MM-DD` format |
| `hire_date` | string | Yes | Official hire date in `YYYY-MM-DD` format |
| `joining_date` | string | Yes | Actual joining date in `YYYY-MM-DD` format |
| `employer_tasks` | array | No | Employer-assigned tasks |
| `joiner_tasks` | array | No | New joiner tasks |
| `joiner_questions` | object | No | Onboarding questionnaire responses |

#### Address Structure

```json
{
    "street_1": "123 Main Street",
    "street_2": "Apt 4B",
    "country": "United States",
    "state": "California",
    "city": "Los Angeles",
    "zip": "90001"
}
```

#### Success Response (200 OK)

```json
{
    "message": {
        "success": [
            "Employee Added Successfully!",
            "You've successfully added a new employee, you can find the employee onboarding updates on their profile."
        ]
    },
    "data": {
        "_id": "67b3f4a2e8c5d2a1b3c4d5e6",
        "first_name": "John",
        "last_name": "Doe",
        "email": "john.doe@example.com",
        "created_at": "2026-04-19T12:00:00.000Z"
    }
}
```

#### Pre-Request Script

The collection includes an automated pre-request script that generates:

- Random first, middle, and last names
- Unique email addresses using timestamp
- Valid phone numbers in Bangladesh format
- Random staff IDs
- Valid date of birth (1990-2002)
- Dynamic addresses

This ensures unique data for each request without manual input.

#### Error Responses

| Status Code | Description | Example Response |
|-------------|-------------|------------------|
| `400` | Invalid request data | `{"error": "Email already exists"}` |
| `401` | Unauthorized | `{"error": "Invalid or expired token"}` |
| `422` | Validation error | `{"error": "Required fields missing"}` |

---

### 3. Create Announcement

Create a company-wide or targeted announcement.

#### Request Details

| Property | Value |
|----------|-------|
| **Method** | `POST` |
| **Endpoint** | `https://api-backend.yosuite.net/component/modules/announcements/announcement/form/form-component/store-content` |
| **Content-Type** | `multipart/form-data` |
| **Authorization** | `Bearer {{token}}` |

#### Request Body (form-data)

```json
{
    "title": "Company Update - New Policy",
    "attachments": null,
    "applicable_scope": "all",
    "exclude_with": null,
    "is_event": false,
    "pinned": true,
    "enable_comments": true,
    "color": null,
    "category": "general",
    "content": "<p class=\"PlaygroundEditorTheme__paragraph\" dir=\"ltr\"><span style=\"white-space: pre-wrap;\">This is the announcement content.</span></p>"
}
```

#### Request Body Parameters

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `title` | string | Yes | Announcement title (1-200 characters) |
| `attachments` | array | No | Array of attachment URLs or file IDs |
| `applicable_scope` | string | Yes | Target audience: `all`, `department`, `team` |
| `exclude_with` | array | No | Array of user/department IDs to exclude |
| `is_event` | boolean | No | Whether this is an event announcement |
| `pinned` | boolean | No | Pin announcement to top (default: false) |
| `enable_comments` | boolean | No | Allow comments on announcement |
| `color` | string | No | Hex color code for announcement card |
| `category` | string | Yes | Category: `general`, `hr`, `it`, `events` |
| `content` | string | Yes | HTML content of the announcement |

#### Applicable Scope Options

| Value | Description |
|-------|-------------|
| `all` | All employees in the organization |
| `department` | Specific departments only |
| `team` | Specific teams only |

#### Category Options

| Value | Description |
|-------|-------------|
| `general` | General company announcements |
| `hr` | Human Resources announcements |
| `it` | IT and system updates |
| `events` | Company events and activities |

#### Success Response (200 OK)

```json
{
    "message": {
        "success": ["Data Inserted Successfully"]
    },
    "data": {
        "_id": "67b3f4a2e8c5d2a1b3c4d5e7",
        "title": "Company Update - New Policy",
        "created_at": "2026-04-19T12:00:00.000Z"
    }
}
```

#### Pre-Request Script

Automatically generates:
- Random title with numeric suffix
- Dynamic content with timestamp
- Random boolean values for `pinned` and `enable_comments`

---

### 4. Create Award

Create an award for employee recognition.

#### Request Details

| Property | Value |
|----------|-------|
| **Method** | `POST` |
| **Endpoint** | `https://api-backend.yosuite.net/component/modules/appreciations/award/form/form-component/store-content` |
| **Content-Type** | `multipart/form-data` |
| **Authorization** | `Bearer {{token}}` |

#### Request Body (form-data)

```json
{
    "name": "Employee of the Month",
    "note": "Recognition for outstanding performance and dedication",
    "award_item": [
        {
            "temp": "Certificate",
            "currency": "USD",
            "gift_name": "Amazon Gift Card",
            "price": 100
        },
        {
            "temp": "Trophy",
            "currency": "USD",
            "gift_name": "Gold Trophy",
            "price": 50
        }
    ]
}
```

#### Request Body Parameters

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `name` | string | Yes | Award name/title |
| `note` | string | No | Additional notes or description |
| `award_item` | array | Yes | Array of award items |

#### Award Item Structure

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `temp` | string | No | Template/identifier for the item |
| `currency` | string | Yes | Currency code: `USD`, `EUR`, `BDT`, etc. |
| `gift_name` | string | Yes | Name of the gift/reward |
| `price` | number | Yes | Price/value of the item |

#### Success Response (200 OK)

```json
{
    "data": {
        "_id": "67b3f4a2e8c5d2a1b3c4d5e8",
        "name": "Employee of the Month",
        "note": "Recognition for outstanding performance and dedication",
        "award_item": [
            {
                "temp": "Certificate",
                "currency": "USD",
                "gift_name": "Amazon Gift Card",
                "price": 100
            }
        ],
        "created_at": "2026-04-19T12:00:00.000Z"
    },
    "message": {
        "success": ["Data Inserted Successfully"]
    }
}
```

#### Pre-Request Script

Automatically generates:
- Random award name
- Random note with timestamp
- Dynamic award items with random prices

#### Test Script

Verifies:
- Response `name` matches request
- Response `note` matches request
- All `award_item` fields match
- Success message is present

---

## Collection Variables

The collection uses the following variables for dynamic data management:

| Variable | Type | Scope | Description | Auto-Populated |
|----------|------|-------|-------------|----------------|
| `token` | string | Collection | Bearer token for API authentication | Yes (from auth) |
| `requestBody` | string | Environment | Temporary storage for dynamic request bodies | Yes (pre-request scripts) |

### Variable Usage Examples

**Using token in Authorization header:**
```
Authorization: Bearer {{token}}
```

**Using requestBody in form-data:**
```
Key: data
Value: {{requestBody}}
```

---

## Automated Testing

### Test Coverage Summary

| Request | Test Cases | Assertions |
|---------|-----------|------------|
| `auth` | 1 | Status code validation, token extraction |
| `createEmployee` | 1 | Success message validation |
| `announcementCreation` | 1 | Success message validation |
| `awardCreation` | 3 | Name/note matching, items validation, success message |


### HTTP Status Codes

| Status Code | Description | Action Required |
|-------------|-------------|-----------------|
| `200` | Success | Request completed successfully |
| `201` | Created | Resource created successfully |
| `400` | Bad Request | Check request parameters and format |
| `401` | Unauthorized | Re-authenticate with valid credentials |
| `403` | Forbidden | Insufficient permissions for this action |
| `404` | Not Found | Endpoint or resource does not exist |
| `409` | Conflict | Resource already exists (e.g., duplicate email) |
| `422` | Unprocessable Entity | Validation failed - check field requirements |
| `429` | Too Many Requests | Rate limit exceeded - wait and retry |
| `500` | Internal Server Error | Server-side issue - contact support |
| `502` | Bad Gateway | Upstream server error - retry later |
| `503` | Service Unavailable | Service temporarily down - retry later |

---