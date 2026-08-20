# API Contracts

## Base URL

```text
/api
```

The backend will use Express.js and Node.js to provide RESTful API endpoints.

---

## 1. Property APIs

| Method | Endpoint                      | Purpose                      |
| ------ | ----------------------------- | ---------------------------- |
| GET    | `/api/properties`             | Retrieve all properties      |
| GET    | `/api/properties/:propertyId` | Retrieve a specific property |
| POST   | `/api/properties`             | Create a new property        |
| PUT    | `/api/properties/:propertyId` | Update an existing property  |
| DELETE | `/api/properties/:propertyId` | Delete a property            |

### GET `/api/properties`

Retrieves all properties.

### GET `/api/properties/:propertyId`

Retrieves a specific property using its ID.

### POST `/api/properties`

Creates a new property.

**Request Body:**

```json
{
  "title": "2BHK Apartment",
  "location": "Varanasi",
  "propertyType": "Apartment",
  "price": 5000000,
  "status": "available"
}
```

### PUT `/api/properties/:propertyId`

Updates an existing property.

### DELETE `/api/properties/:propertyId`

Deletes an existing property.

---

## 2. Cache APIs

| Method | Endpoint                 | Purpose                                   |
| ------ | ------------------------ | ----------------------------------------- |
| GET    | `/api/cache/:propertyId` | Retrieve cache information for a property |
| POST   | `/api/cache`             | Create a cache entry                      |
| PUT    | `/api/cache/:cacheId`    | Update or refresh a cache entry           |
| DELETE | `/api/cache/:cacheId`    | Remove a cache entry                      |

### GET `/api/cache/:propertyId`

Retrieves cached information associated with a property.

### POST `/api/cache`

Creates a new cache entry.

**Request Body:**

```json
{
  "propertyId": "665abc123",
  "cacheKey": "property:P101",
  "cacheStatus": "active",
  "expiresAt": "2026-08-20T12:30:00Z"
}
```

### PUT `/api/cache/:cacheId`

Updates or refreshes an existing cache entry.

### DELETE `/api/cache/:cacheId`

Removes a cache entry.

---

## 3. Standard Success Response

```json
{
  "success": true,
  "data": {}
}
```

For list operations:

```json
{
  "success": true,
  "data": [],
  "message": "Properties retrieved successfully"
}
```

---

## 4. Standard Error Response

```json
{
  "success": false,
  "message": "Property not found",
  "error": "PROPERTY_NOT_FOUND"
}
```

---

## 5. HTTP Status Codes

| Status Code | Meaning                          |
| ----------- | -------------------------------- |
| 200         | Request completed successfully   |
| 201         | Resource created successfully    |
| 400         | Invalid or missing input         |
| 404         | Requested resource was not found |
| 409         | Duplicate or conflicting data    |
| 500         | Unexpected server error          |
| 503         | Service temporarily unavailable  |

---

## 6. Error Handling

The API should provide meaningful errors for:

* Missing required fields
* Invalid property IDs
* Invalid property data
* Property not found
* Cache entry not found
* Duplicate cache keys
* Database failures
* Redis connection failures
* Temporary service unavailability

---

## 7. Connectivity Handling

If MongoDB or Redis is temporarily unavailable, the backend should return:

```json
{
  "success": false,
  "message": "Service temporarily unavailable",
  "error": "SERVICE_UNAVAILABLE"
}
```

The frontend should display a user-friendly connection or retry message.

---

## 8. API Architecture

```text
React Frontend
      │
      │ HTTP Request
      ▼
Express.js / Node.js
      │
      ├──────────► MongoDB
      │
      └──────────► Redis
```

MongoDB is the primary persistent data store, while Redis provides fast access to frequently requested property data.
