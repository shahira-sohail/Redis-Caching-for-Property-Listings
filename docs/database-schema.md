# Database Schema 
## Database Technology
The application will use **MondoDB** as the primary database.
MongoDB stores data as documents inside collections rather than rows inside relational tables.
---
## 1. Users Collection
The `users` collection stores the staff and managers who use the system.
| Field     | Type      | Required | Description                     |
|-----------|-----------|----------|---------------------------------|
| `_id`     | ObjectId  | Yes      | Unique MongoDB identifier       |
| `name`    | String    | Yes      | User's name                     |
| `email`   | String    | Yes      | User's email address            |
| `role`    | String    | Yes      | User role: `staff` or `manager` |

### Validation Rules
- `name` must not be empty.
- `email` must have a valid email format.
- `email` must be unique.
- `role` must be either `staff` or `manager`.

---

## 2. Properties Collection

The `properties` collection stores property information.

| Field          | Type     | Required | Description                   |
|----------------|----------|----------|-------------------------------|
| `_id`          | ObjectId | Yes      | Unique MongoDB identifier     |
| `title`        | String   | Yes      | Property title                |
| `location`     | String   | Yes      | Property location             |
| `propertyType` | String   | Yes      | Apartment, House, Villa, etc. |
| `price`        | Number   | Yes      | Property price                |
| `status`       | String   | Yes      | Current property status       |

### Validation Rules
- `title` must not be empty.
- `location` must not be empty.
- `propertyType` must not be empty.
- `price` must be greater than or equal to `0`.
- `status` must contain a valid value such as `available` or `sold`.

---

## 3. Cache Entries Collection

The `cacheEntries` collection stores metadata about property data cached in Redis.

| Field        | Type     | Required | Description                            |
|--------------|----------|----------|----------------------------------------|
| `_id`        | ObjectId | Yes      | Unique MongoDB identifier              |
| `propertyId` | ObjectId | Yes      | Reference to the related property      |
| `cacheKey`   | String   | Yes      | Redis key used to identify cached data |
| `cacheStatus`| String   | Yes      | Current cache state                    |
| `createdAt`  | Date     | Yes      | Time when the cache entry was created  |
| `expiresAt`  | Date     | Yes      | Time when the cache entry expires      |

### Validation Rules

- `propertyId` must reference an existing property.
- `cacheKey` must not be empty.
- `cacheKey` should be unique.
- `cacheStatus` should contain a valid value such as `active` or `expired`.
- `expiresAt` must be later than `createdAt`.

---

## Relationships

MongoDB is a document database, so relationships are handled using references rather than traditional SQL foreign keys.

- A cache entry references a property through `propertyId`.
- One property may have multiple cache entry records if cache history is retained.
- Users are independent entities because they use the application but do not directly own properties.

---

## Redis Usage

MongoDB will act as the primary persistent database.

Redis will be used as a fast caching layer for frequently accessed property information.

Example Redis key:

`property:P101`

The Redis value may contain the cached property information.
MongoDB remains the primary source of truth, while Redis provides faster access to frequently requested data.

---

## Indexing Considerations

The following fields should be considered for indexing:

- `users.email`
- `properties.location`
- `properties.status`
- `cacheEntries.propertyId`
- `cacheEntries.cacheKey`

Indexes will improve the speed of frequently used searches and lookups.
