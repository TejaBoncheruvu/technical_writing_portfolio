# **FitTrack API Documentation** 
FitTrack is a simple, easy-to-use app that helps users build and maintain their fitness habits. Users create habits like drinking water, walking steps, doing workouts, or meditating. Each day, they open the app, mark their progress, and view how consistently they’re improving. The app also provides weekly and monthly summaries that help users stay motivated and monitor long-term trends.

**Base URL**
https://api.fittrack.com/v1

## **1. Register** 
Create a new FitTrack user account

### **End point**
`POST /auth/register`

### **Parameters**
| Name     | Type   | Required | Description          |
| -------- | ------ | -------- | -------------------- |
| name     | string | Yes      | Full name            |
| email    | string | Yes      | Unique email address |
| password | string | Yes      | Minimum 8 characters |

### **Request (Python)**
```python
import requests
response = requests.post(
    "https://api.fittrack.com/v1/auth/register",
    headers={"Content-Type": "application/json"},
    json={
        "name": "Teja",
        "email": "teja@example.com",
        "password": "Admin@123"
    }
)
print(response.json())
```

### **Response**
```json
{
  "user": {
    "id": "uuid",
    "name": "Teja",
    "email": "teja@example.com",
    "created_at": "2025-12-03T10:00:00Z"
  }
}
```


## **2. Login**
Authenticate a user and return access + refresh tokens.

### **Endpoint**
`POST /auth/login`

### **Parameters**
| Name     | Type   | Required | Description |
| -------- | ------ | -------- | ----------- |
| email    | string | Yes      | Login email |
| password | string | Yes      | Password    |

### **Request (Python)**
```python
import requests
response = requests.post(
    "[https://api.fittrack.com/v1/auth/login](https://api.fittrack.com/v1/auth/login)",
    headers={"Content-Type": "application/json"},
    json={
        "email": "teja@example.com",
        "password": "Admin@123"
    }
)
print(response.json())
```

### **Response**
```json
{
  "access_token": "jwt_access_token_here",
  "access_expires_in": 1800,
  "refresh_token": "long_lived_refresh_token",
  "refresh_expires_in": 1209600,
  "user": {
    "id": "uuid",
    "name": "Teja",
    "email": "teja@example.com"
  }
}
```


## **3. Logout**
Invalidate a refresh token.

### **Endpoint**
`POST /auth/logout`

### **Parameters**
| Name          | Type   | Required | Description         |
| ------------- | ------ | -------- | ------------------- |
| refresh_token | string | Yes      | Token to invalidate |

### **Request (Python)**
```python
import requests
response = requests.post(
    "[https://api.fittrack.com/v1/auth/logout](https://api.fittrack.com/v1/auth/logout)",
    headers={"Content-Type": "application/json"},
    json={"refresh_token": "long_lived_refresh_token"}
)
print(response.status_code)
```

### **Response**
*No content returned.*


## **4. Create Habit**
Create a new habit for the authenticated user.

### **Endpoint**
`POST /habits`

### **Parameters**
| Name          | Type   | Required | Description         |
|-------------- |------- |---------|----------------------|
| name        | string   | Yes     | Habit name           |
| frequency   | string | Yes       | `daily` or `weekly`  |
| description | string | No        | Optional details|
| target      | object | No        | Contains `type` and `value` (e.g., liters, minutes) |
| reminder    | object | No        | Contains `enabled` and `time`|

### **Request (Python)**
```python
import requests
response = requests.post(
    "[https://api.fittrack.com/v1/habits](https://api.fittrack.com/v1/habits)",
    headers={
        "Authorization": "Bearer <access_token>",
        "Content-Type": "application/json"
    },
    json={
        "name": "Drink Water",
        "frequency": "daily",
        "target": {"type": "liters", "value": 3},
        "reminder": {"enabled": True, "time": "08:00"}
    }
)
print(response.json())
```

### **Response**
```json
{
  "habit": {
    "id": "uuid",
    "name": "Drink Water",
    "frequency": "daily",
    "target": { "type": "liters", "value": 3 },
    "reminder": { "enabled": true, "time": "08:00" },
    "created_at": "2025-12-03T10:00:00Z"
  }
}
```


## **5. List Habits**
Retrieve all habits for the authenticated user.

### **Endpoint**
`GET /habits`

### **Request (Python)**
```python
import requests
response = requests.get(
    "[https://api.fittrack.com/v1/habits](https://api.fittrack.com/v1/habits)",
    headers={
        "Authorization": "Bearer <access_token>",
        "Content-Type": "application/json"
    }
)
print(response.json())
```

### **Response**
```json
{
  "habits": [
    {
      "id": "uuid",
      "name": "Drink Water",
      "frequency": "daily"
    }
  ]
}
```


## **6. Update Habit**
Modify an existing habit.

### **Endpoint**
`PUT /habits/{habitId}`

### **Parameters**

| Name      | Type   | Required | Description                |
| --------- | ------ | -------- | -------------------------- |
| name      | string | No       | New habit name             |
| frequency | string | No       | Updated frequency          |
| target    | object | No       | Contains `type`, `value`   |
| reminder  | object | No       | Contains `enabled`, `time` |

### **Request (Python)**
```python
import requests
response = requests.put(
    "[https://api.fittrack.com/v1/habits/](https://api.fittrack.com/v1/habits/)<habitId>",
    headers={
        "Authorization": "Bearer <access_token>",
        "Content-Type": "application/json"
    },
    json={
        "name": "Water Intake",
        "frequency": "daily",
        "target": {"type": "liters", "value": 2}
    }
)
print(response.json())
```

### **Response**
```json
{
  "habit": {
    "id": "uuid",
    "name": "Water Intake",
    "frequency": "daily",
    "target": { "type": "liters", "value": 2 }
  }
}
```


## **7. Delete Habit**
Remove a habit.

### **Endpoint**
`DELETE /habits/{habitId}`

### **Request (Python)**
```python
import requests
response = requests.delete(
    "[https://api.fittrack.com/v1/habits/](https://api.fittrack.com/v1/habits/)<habitId>",
    headers={
        "Authorization": "Bearer <access_token>",
        "Content-Type": "application/json"
    }
)
print(response.status_code)
```

### **Response**
*No content returned.*


## **8. Log Habit Progress**
Add progress for a specific day.

### **Endpoint**
`POST /habits/{habitId}/log`

### **Parameters**
| Name     | Type   | Required | Description          |
| -------- | ------ | -------- | -------------------- |
| date     | string | Yes      | Format: `YYYY-MM-DD` |
| progress | number | Yes      | Daily progress value |
| note     | string | No       | Optional note        |

### **Request (Python)**
```python
import requests
response = requests.post(
    "[https://api.fittrack.com/v1/habits/](https://api.fittrack.com/v1/habits/)<habitId>/log",
    headers={
        "Authorization": "Bearer <access_token>",
        "Content-Type": "application/json"
    },
    json={
        "date": "2025-12-03",
        "progress": 1,
        "note": "Completed after workout"
    }
)
print(response.json())
```

### **Response**
```json
{
  "log": {
    "id": "uuid",
    "habit_id": "uuid",
    "date": "2025-12-03",
    "progress": 1,
    "note": "Completed after workout"
  }
}
```


## **9. Get Habit Logs**
Retrieve logs with optional date filtering.

### **Endpoint**
`GET /habits/{habitId}/log`

### **Parameters**
| Name | Type   | Description |
| ---- | ------ | ----------- |
| from | string | Start date  |
| to   | string | End date    |

### **Request (Python)**
```python
import requests
response = requests.get(
    "[https://api.fittrack.com/v1/habits/](https://api.fittrack.com/v1/habits/)<habitId>/log?from=2025-12-01&to=2025-12-10",
    headers={
        "Authorization": "Bearer <access_token>",
        "Content-Type": "application/json"
    }
)
print(response.json())
```

### **Response**
```json
{
  "logs": [
    { "id": "uuid", "date": "2025-12-01", "progress": 1 },
    { "id": "uuid", "date": "2025-12-02", "progress": 1 }
  ],
  "meta": {
    "from": "2025-12-01",
    "to": "2025-12-10",
    "count": 2
  }
}
```

> The `meta` object returns the applied filters and record count.


## **10. Analytics Summary**
Return performance analytics and streaks.

### **Endpoint**
`GET /analytics/summary`

### **Parameters**
| Name   | Type   | Description              |
| ------ | ------ | ------------------------ |
| period | number | 7 or 30 days (default 7) |

### **Request (Python)**
```python
import requests
response = requests.get(
    "[https://api.fittrack.com/v1/analytics/summary?period=7](https://api.fittrack.com/v1/analytics/summary?period=7)",
    headers={
        "Authorization": "Bearer <access_token>",
        "Content-Type": "application/json"
    }
)
print(response.json())
```

### **Response Fields**
| Field           | Type   | Description                               |
| --------------- | ------ | ----------------------------------------- |
| streak          | object | Current and best streak                   |
| summary         | object | Completed vs total counts for 7 & 30 days |
| habit_rates     | array  | Success rate per habit                    |
| most_consistent | array  | Ranked consistency                        |

### **Response**
```json
{
  "streak": { "current": 5, "best": 12 },
  "summary": {
    "7_days": { "completed": 18, "total": 21 },
    "30_days": { "completed": 72, "total": 90 }
  },
  "habit_rates": [
    { "habit_id": "uuid", "name": "Drink Water", "success_rate": 85.0 }
  ],
  "most_consistent": [
    { "habit_id": "uuid", "name": "Meditation", "consistency_score": 0.92 }
  ]
}
```


## **Global Error Reference**

### **Error Example**

```json
{
  "error": {
    "status": 400,
    "code": "INVALID_INPUT",
    "message": "The request contains invalid or missing fields.",
    "field": "email"
  }
}
```

### **Error Codes**

| Status | Code                  | Message                      |
| ------ | --------------------- | ---------------------------- |
| 400    | INVALID_INPUT         | Invalid or missing fields    |
| 400    | INVALID_EMAIL_FORMAT  | Email format incorrect       |
| 400    | WEAK_PASSWORD         | Password does not meet rules |
| 401    | INVALID_CREDENTIALS   | Incorrect email or password  |
| 401    | INVALID_REFRESH_TOKEN | Refresh token invalid        |
| 401    | UNAUTHORIZED          | Missing/invalid token        |
| 403    | FORBIDDEN             | Action not allowed           |
| 404    | NOT_FOUND             | Resource not found           |
| 409    | EMAIL_EXISTS          | Email already registered     |
| 409    | CONFLICT              | Duplicate log                |
| 429    | RATE_LIMITED          | Too many requests            |
| 500    | SERVER_ERROR          | Internal server error        |



