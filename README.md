# Redis-Caching-for-Property-Listings

## Overview

Property Listings currently manages property and Redis cache information through manual paper-based processes and Excel sheets. This can lead to data loss, inconsistent records, and operational delays.

This project proposes a digital Redis Caching system that allows floor staff to access property information quickly and consistently.

## Project Status

**Capstone 1 — Architectural Planning**

This phase focuses on database architecture, ERD design, Redis caching strategy, and API contracts.

No application feature implementation is included in this phase.

## Objectives

* Digitize property and cache information.
* Provide a structured data model using MongoDB.
* Use Redis for fast access to frequently requested property data.
* Define consistent REST API contracts.
* Design the system to handle invalid input, empty results, and connectivity failures.
* Maintain an accessible and user-friendly interface in the implementation phase.

## Technology Stack

* **MongoDB** — Primary persistent database
* **Express.js** — Backend API framework
* **React.js** — Frontend interface
* **Node.js** — Backend runtime
* **Redis** — Caching layer

## Architecture

```text
React.js
   │
   │ HTTP Requests
   ▼
Express.js + Node.js
   │
   ├──────────────► MongoDB
   │                Primary Database
   │
   └──────────────► Redis
                    Caching Layer
```

## Data Model

The initial MongoDB collections are:

### Users

Stores staff and manager information.

### Properties

Stores property information such as title, location, type, price, and availability status.

### Cache Entries

Stores metadata about property data cached in Redis, including the Redis cache key and expiration information.

## Redis Caching Strategy

MongoDB acts as the primary source of persistent property data.

Redis is used as a temporary, high-speed caching layer for frequently requested property information.

Example cache key:

```text
property:P101
```

Cached information will have an expiration time to prevent outdated data from being treated as current.

## API

The planned REST API provides endpoints for:

* Managing properties
* Retrieving property information
* Creating and managing cache entries
* Handling API errors consistently

Detailed API definitions are available in:

`docs/api-contracts.md`

## Edge Case Handling

The planned implementation will handle:

* Empty search results
* Invalid or missing input
* Slow or unavailable services
* MongoDB failures
* Redis connection failures
* User-friendly error messages
* Loading states during asynchronous operations

## Security

The implementation will:

* Validate user input.
* Sanitize text input before storing it in application state.
* Avoid hardcoding API keys or sensitive credentials.
* Store sensitive configuration through environment variables.

## Accessibility

The frontend implementation will target a **100% Lighthouse accessibility score**.

Interactive elements will have appropriate labels and keyboard accessibility.

## Telemetry Simulation

Primary user actions will trigger a simulated analytics message:

```text
[Analytics] User interacted with Redis Caching
```

## Documentation

* [Database Schema](docs/database-schema.md)
* [ERD](docs/erd.md)
* [API Contracts](docs/api-contracts.md)

## Current Scope

This repository currently contains the architectural planning for Capstone 1.

Application implementation will be developed in a later phase.
