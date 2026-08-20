# Entity Relationship Diagram (ERD)

## Property Listings Redis Caching System

```text
┌─────────────────────────┐
│          USERS          │
├─────────────────────────┤
│ _id : ObjectId          │
│ name : String           │
│ email : String          │
│ role : String            │
└─────────────────────────┘


┌─────────────────────────┐
│       PROPERTIES        │
├─────────────────────────┤
│ _id : ObjectId          │
│ title : String          │
│ location : String       │
│ propertyType : String   │
│ price : Number          │
│ status : String         │
└────────────┬────────────┘
             │
             │ referenced by
             │ propertyId
             │
             ▼
┌─────────────────────────┐
│     CACHE_ENTRIES       │
├─────────────────────────┤
│ _id : ObjectId          │
│ propertyId : ObjectId   │
│ cacheKey : String       │
│ cacheStatus : String    │
│ createdAt : Date        │
│ expiresAt : Date        │
└─────────────────────────┘

## Relationships
Properties → Cache Entries

A property can have one or more cache entry records over its lifetime.

Each cache entry references one property using:

cacheEntries.propertyId → properties._id

## Users

Users are currently independent from the property and cache collections.

Users interact with the system but do not directly own a property or cache entry.

## Architecture overview

                    ┌──────────────┐
                    │    React     │
                    │   Frontend   │
                    └──────┬───────┘
                           │
                           │ HTTP Requests
                           ▼
                    ┌──────────────┐
                    │   Express    │
                    │   / Node.js  │
                    └──────┬───────┘
                           │
                 ┌─────────┴─────────┐
                 │                   │
                 ▼                   ▼
          ┌──────────────┐    ┌──────────────┐
          │   MongoDB    │    │    Redis     │
          │ Primary Data │    │    Cache     │
          └──────────────┘    └──────────────┘




