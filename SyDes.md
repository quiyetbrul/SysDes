# System Design

## Proximity Service System

- GeoHashing Algorithm
- Requires high read and write throughput
  - databases that can be used
    - Cassandra for write-heavy workloads
    - Redis for read-heavy workloads

## Social Media System

- News Feed
  - Redis for caching new posts into followers' feeds
  - comments and likes are stored in
- Profile
  - MySQL for storing user data
  - Graph database for storing relationships between users
- Search
  - ElasticSearch for searching
- Notifications
  - Kafka for sending notifications
  - Redis for caching notifications

## Messaging & Chat System

- Chat
  - WebSockets for real-time communication
  - Redis for caching messages
- scalability of chat rooms/groups
  - Kafka for pub/sub messaging
  - Redis for caching messages
- end-to-end encryption and security

## Distributed File Storage & Content Delivery System

## Search & Recommendation System

## Payment & Financial System

## URL Shortening System

## Job  Scheduler

## Ticketmaster

## Parking Lot

### Functional Requirements

- be able to reserve a parking spot
- be able to pay for the parking spot
- be able to cancel the reservation
- be able to extend the reservation

### Non-Functional Requirements

- high availability, meaning the system should be up and running 24/7 or during the parking hours
- low latency, meaning the system should respond quickly to user requests especially if the parking lot is in a busy city or during peak hours
- high throughput, meaning the system should be able to handle a large number of requests per second
- scalability, meaning the system should be able to handle a large number of parking lots and users
