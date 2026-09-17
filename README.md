 # 🏨 Hotel Booking System

A full-featured hotel reservation platform inspired by Hotels.com, designed using **Service-Oriented Architecture (SOA)** and **Layered Architecture**. The system supports hotel room management, searching, booking, commenting, notification via queue systems, and an AI Agent for natural language interactions.

---

## ✅ Features

### Functional Requirements

#### 👤 Authentication
- JWT-based login and registration
- Admin and User roles

#### 🏨 Hotel Admin Service
- Add/update/delete hotels and rooms
- Room availability based on date ranges
- Admin-only access

#### 🔍 Hotel Search Service
- Search by location, date, and people count
- Only rooms with availability shown
- 15% discount for logged-in users

#### 📅 Book Hotel Service
- Room booking without payment
- Capacity decreases automatically
- Message sent to RabbitMQ

#### 💬 Comments Service
- Users can add/view comments per hotel
- Distribution chart of ratings (1-5 stars)

#### 🔔 Notification Service
- Reads from RabbitMQ queue
- Sends low-capacity email alerts to admins

#### 🤖 AI Agent Service
- Accepts natural language (e.g., “Book a room in İzmir”)
- Routes to correct services via API Gateway

---

## 🧰 Tech Stack

| Layer        | Technology               |
|--------------|--------------------------|
| Frontend     | React.js, Tailwind CSS   |
| Backend API  | .NET 8 Web API           |
| Gateway      | Ocelot                   |
| Database     | PostgreSQL, MongoDB      |
| Messaging    | RabbitMQ                 |
| Cache        | Redis                    |
| Hosting      | Azure, Vercel            |

---


## 🧪 Sample Tests

Below are sample test cases and payloads to verify the core features of the Hotel Booking System.

---

### 🏨 Add a Hotel

**POST** `/api/v1/Hotel`

```
{
  "name": "Alpine Retreat",
  "location": "Swiss Alps",
  "description": "Cozy chalet-style hotel with direct access to ski slopes"
}
```
---

### 🚪 Add a Room

**POST** `/api/v1/Room`

```
{
  "roomNumber": "112",
  "capacity": 10,
  "pricePerNight": 1000,
  "availableFrom": "2025-07-05",
  "availableTo": "2025-07-20",
  "hotelId": 4
}
```

---


### 🔍 Search Available Rooms

**GET** `/api/v1/Room/search`
```
#### Query Parameters

| Parameter     | Type   | Example        | Description                              |
|---------------|--------|----------------|------------------------------------------|
| `location`    | string | `Rome`         | City or area where the room is located   |
| `startDate`   | date   | `2025-07-05`   | Start date of availability               |
| `endDate`     | date   | `2025-07-20`   | End date of availability                 |
| `peopleCount` | int    | `1`            | Number of people the room must support   |
| `page`        | int    | `1`            | Page number for pagination               |
| `pageSize`    | int    | `10`           | Number of results per page               |

```
---


### 🛏️ Make a Reservation

**POST** `/api/v1/Reservation`


```
{
  "roomId": 6,
  "username": "mehmet",
  "startDate": "2025-08-06",
  "endDate": "2025-08-15",
  "peopleCount": 9
}

Description

Creates a reservation for the specified room (roomId: 6) under the username mehmet, between 2025-08-06 and 2025-08-15, for 9 people.

✉️ Note:
If the remaining capacity of the room after the reservation drops below 20%, a notification email will be sent automatically to hotel admins.

This behavior is handled by the LowCapacityCheckerService.

```

---

### 💬 Add a Comment

**POST** `/api/v1/Comments`

```
{
  "userName": "Ahmet",
  "text": "Otel inanılmazdı, çok memnun kaldım!",
  "rating": 5,
  "hotelId": 2
}


Adds a new user comment and rating for the hotel with ID 2.

📝 Note:
You can verify that the comment appears on the /comments/:hotelId page,
along with the updated rating distribution chart reflecting the new feedback.
```
---

## 🧑‍💼 Agent Actions

---

### 🛏️ Book a Room at Ocean Breeze Resort (Maldives)

**Intent:** Book a room at **Ocean Breeze Resort** hotel in **Maldives**  
**For:** 2 people  
**Date Range:** July 6, 2025 → July 15, 2025  
**Username:** koray

#### 1️⃣ Step 1: Search Available Rooms

**GET** `/api/v1/Room/search`

```http
GET /api/v1/Room/search?location=Maldives&startDate=2025-07-06&endDate=2025-07-15&peopleCount=2&page=1&pageSize=10

```
➡️ **Filter by hotel name:** *Ocean Breeze Resort*

---

### 2️⃣ Step 2: Make a Reservation

Once a suitable room (e.g., `roomId: 12`) is found at **Ocean Breeze Resort**:

**POST** `/api/v1/Reservation`

```json
{
  "roomId": 12,
  "username": "koray",
  "startDate": "2025-07-06",
  "endDate": "2025-07-15",
  "peopleCount": 2
}
```

---

### 🔍 Search a Room in Rome

- 📍 **Konum:** Rome  
- 👤 **Kişi Sayısı:** 1  
- 📅 **Tarih Aralığı:** 2025-07-06 → 2025-07-15

**GET** `/api/v1/Room/search`

```http
GET /api/v1/Room/search?location=Rome&startDate=2025-07-06&endDate=2025-07-15&peopleCount=1&page=1&pageSize=10
```
---

### 🔍 Search a Room in Maldives (August)

- 📍 **Konum:** Maldives  
- 👤 **Kişi Sayısı:** 1  
- 📅 **Tarih Aralığı:** 2025-08-05 → 2025-08-10

**GET** `/api/v1/Room/search`

```http
GET /api/v1/Room/search?location=Maldives&startDate=2025-08-05&endDate=2025-08-10&peopleCount=1&page=1&pageSize=10
```


## 📹 Video Demo

🎥 **Watch the video here:**  
[Demo Video]()

---

## 🌍 Deployment Links

| Layer / Service             | URL                           |
|----------------------------|-------------------------------|
| 🖥️ Frontend                 | [link](#)                     |
| 🌐 API Gateway             | [link](#)                     |
| 🏨 Hotel Service           | [link](#)                     |
| 💬 Comment Service         | [link](#)                     |
| 🔔 Notification Service    | [link](#)                     |
| 🤖 AI Agent Service        | [link](#)                     |
