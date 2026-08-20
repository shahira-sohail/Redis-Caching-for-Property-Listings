# Database Schema

## 1. Users

| Field    | Data Type    | Key         | Required |          Description            |
|----------|--------------|-------------|----------|---------------------------------|
| user_id  | VARCHAR(20)  | Primary Key | Yes      | Unique identifier for each user |
| name     | VARCHAR(100) |      —      | Yes      | User's name                     |
| email    | VARCHAR(150) |      —      | Yes      | User's email address            |
| role     | VARCHAR(20)  |      —      | Yes      | User role: Staff or Manager     |

## 2. Properties

| Field         | Data Type     | Key         | Required | Description                         |
|---------------|---------------|-------------|----------|-------------------------------------|
| property_id   | VARCHAR(20)   | Primary Key | Yes      | Unique identifier for each property |
| title         | VARCHAR(150)  |      -      | Yes      | Property title                      |
| location      | VARCHAR(150)  |      —      | Yes      | Property location                   |
| property_type | VARCHAR(50)   |      —      | Yes      | Type of property                    |
| price         | DECIMAL(12,2) |      —      | Yes      | Property price                      |
| status        | VARCHAR(30)   |      —      | Yes      | Current property status             |

## 3. Cache Entries

| Field        | Data Type     | Key         | Required | Description                               |
|--------------|---------------|-------------|----------|-------------------------------------------|
| cache_id     | VARCHAR(20)   | Primary Key |   Yes    | Unique identifier for the cache entry     |
| property_id  | VARCHAR(20)   | Foreign Key |   Yes    | References the related property           |
| cache_key    | VARCHAR(255)  |      —      |   Yes    | Key used to identify cached property data |
| cache_status | VARCHAR(20)   |      —      |   Yes    | Current cache state                       |
| created_at   | TIMESTAMP     |      —      |   Yes    | Time when the cache entry was created     |
| expires_at   | TIMESTAMP     |      —      |   Yes    | Time when the cache entry expires         |

## Relationships

- One property can have one or more cache entries over its lifetime.
- Each cache entry belongs to one property.
- `properties.property_id` is referenced by `cache_entries.property_id`.
- Users are currently kept separate because they use the system but do not directly own a property or cache entry.

## Constraints

- `user_id`, `property_id`, and `cache_id` must be unique.
- Required fields must not be empty.
- Property price must not be negative.
- `cache_key` must be unique.
- `expires_at` must be later than `created_at`.
- `cache_status` should contain only valid states such as `Active` or `Expired`.
- `role` should contain only `Staff` or `Manager`.
