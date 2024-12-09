1. Install Required Packages
Install the necessary Python packages:
```bash
pip install fastapi uvicorn motor jinja2
```

2. Configure MongoDB Atlas
1. Log in to your MongoDB Atlas account.
2. Create a database cluster if not already available.
3. Add a database user with read/write access.
4. Update the connection string in the `app.py` file:
   ```python
   MONGO_URI = "mongodb+srv://<db_username>:<db_password>@cluster0.sgudd.mongodb.net/"
   ```
5. Ensure the database name is `cloud_services` and collections (`subscription_plans`, `permissions`, `user_subscriptions`, `user_usage_stats`) exist.

### 3. Directory Structure
Ensure your project directory has the following structure:
```
.
├── app.py                  # Main application file
├── templates/              # Directory for HTML templates
│   └── home.html           # Home page template
├── static/                 # Directory for static files (e.g., CSS, JS)
```

4. Add `home.html`
Create a `templates/home.html` file with the following content:
```html
<!DOCTYPE html>
<html>
<head>
    <title>Cloud Services</title>
</head>
<body>
    <h1>Subscription Plans</h1>
    <ul>
        {% for plan in plans %}
            <li>{{ plan['name'] }}: {{ plan['description'] }}</li>
        {% endfor %}
    </ul>
</body>
</html>
```

**Running the Application**
1. Start the application using `uvicorn`:
   ```bash
   uvicorn app:app --reload
   ```

2. Open your browser and navigate to:
   - Home page: [http://127.0.0.1:8000/](http://127.0.0.1:8000/)
   - Swagger UI (API documentation): [http://127.0.0.1:8000/docs](http://127.0.0.1:8000/docs)

**API Usage**

Admin Endpoints
Create Subscription Plan
- **Endpoint**: `/api/admin/subscription_plans`
- **Method**: POST
- **Request Body**:
  ```json
  {
      "name": "Basic Plan",
      "description": "Access to basic services",
      "api_permissions": ["service1", "service2"],
      "usage_limits": {"service1": 100, "service2": 50}
  }
  ```

List Subscription Plans
- **Endpoint**: `/api/admin/subscription_plans`
- **Method**: GET

Add Permission
- **Endpoint**: `/api/admin/permissions`
- **Method**: POST
- **Request Body**:
  ```json
  {
      "name": "Service 1 Access",
      "api_endpoint": "service1",
      "description": "Access to Service 1"
  }
  ```

Customer Endpoints
Subscribe to Plan
- **Endpoint**: `/api/customer/subscribe`
- **Method**: POST
- **Request Body**:
  ```json
  {
      "user_id": "user123",
      "plan_id": "<PLAN_ID>"
  }
  ```

View Usage Statistics
- **Endpoint**: `/api/customer/usage/{user_id}`
- **Method**: GET

Call a Service
- **Endpoint**: `/api/service/{api_name}`
- **Method**: POST
- **Request Body**:
  ```json
  {
      "user_id": "user123",
      "api": "service1"
  }
  ```

**Testing the Application**
1. Use Swagger UI ([http://127.0.0.1:8000/docs](http://127.0.0.1:8000/docs)) to interact with the API endpoints.
2. Monitor the MongoDB Atlas database to ensure data is being saved correctly.

**Error Handling**
- **404 Not Found**: Returned when a requested resource does not exist.
- **403 Forbidden**: Returned when a user attempts to access a restricted resource.
- **500 Internal Server Error**: Returned for unexpected issues.
