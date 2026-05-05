## Hi there;
# Gazette Exchange System
*A simple platform for hassle-free item swapping.*

---

## 📌 Introduction

In a world where we often have things we don't need and want things we don't have, finding a way to trade without using money can be difficult.  

This project explores a **desktop-based exchange system** that helps people swap goods and services directly. It focuses on how the **requester**, **provider**, and the **system** interact to complete a successful exchange.

---

## 📖 Case Study

In the Gazette Exchange community, people trade items they no longer use.

- **Emily** has vintage stamps and wants a graphic design textbook.
- She posts her items with descriptions and images.
- **Liam**, a student, has the exact textbook and wants stamps.
- Liam sends an **Exchange Request**.
- Emily reviews and accepts the request.
- The system enables secure messaging for coordination.
- The item status is marked as **Pending** during negotiation.
- After the exchange, both mark it as **Completed**.
- Liam leaves a positive review, building Emily’s reputation.

💡 This system helps users:
- Save money  
- Reuse items  
- Build trust in the community  

---

## 🗄️ Database Schema
<img width="1128" height="676" alt="Image" src="https://github.com/user-attachments/assets/61836530-be2a-41d0-abc0-7180badb602a" />

### Database
```sql
CREATE DATABASE GazetteExchangeDB;
USE GazetteExchangeDB;

CREATE TABLE Users (
    UserID INT PRIMARY KEY IDENTITY(1,1),
    Name NVARCHAR(100),
    Email NVARCHAR(100) UNIQUE,
    Password NVARCHAR(100)
);
CREATE TABLE Posts (
    PostID INT PRIMARY KEY IDENTITY(1,1),
    UserID INT,
    HaveItem NVARCHAR(200),
    WantItem NVARCHAR(200),
    DatePosted DATETIME DEFAULT GETDATE(),
    FOREIGN KEY (UserID) REFERENCES Users(UserID)
);

CREATE TABLE Requests (
    RequestID INT PRIMARY KEY IDENTITY(1,1),
    PostID INT,
    SenderID INT,
    ReceiverID INT,
    Status NVARCHAR(50) DEFAULT 'Pending',
    FOREIGN KEY (PostID) REFERENCES Posts(PostID),
    FOREIGN KEY (SenderID) REFERENCES Users(UserID),
    FOREIGN KEY (ReceiverID) REFERENCES Users(UserID)
);
CREATE TABLE Reviews (
    ReviewID INT PRIMARY KEY IDENTITY(1,1),
    UserID INT,
    Rating INT,
    Comment NVARCHAR(300),
    FOREIGN KEY (UserID) REFERENCES Users(UserID)
);
Normalization & Finalization
1. Posts (Users → Posts)
Relationship: One-to-Many

UNF / 1NF:
UserID, Name, Email, PostID, HaveItem, WantItem, DatePosted

2NF:

Users(UserID, Name, Email)
Posts(PostID, HaveItem, WantItem, DatePosted, UserID)

3NF:

Same as 2NF (No transitive dependency)
2. Requests (Users ↔ Posts)
Relationship: One-to-Many

UNF:
RequestID, PostID, HaveItem, SenderID, SenderName, ReceiverID, ReceiverName, Status

1NF:
RequestID, PostID, SenderID, ReceiverID, Status

2NF / 3NF:

Requests(RequestID, PostID, SenderID, ReceiverID, Status)
Users(UserID, Name)
Posts(PostID, HaveItem)
3. Reviews (Users → Reviews)
Relationship: One-to-Many

UNF:
ReviewID, UserID, Name, Rating, Comment

1NF:
ReviewID, UserID, Rating, Comment

2NF / 3NF:

Reviews(ReviewID, UserID, Rating, Comment)
Users(UserID, Name)
✅ Final Tables
User: user_id, name, email, password
Post: post_id, have_item, want_item, date_posted, user_id (FK)
Request: request_id, post_id (FK), sender_id (FK), receiver_id (FK), status
Review: review_id, rating, comment, user_id (FK)
🔁 Transition Table
<img width="515" height="624" alt="Image" src="https://github.com/user-attachments/assets/60e08f13-489b-4353-951e-96082a55ee5d" />

<img width="543" height="820" alt="Image" src="https://github.com/user-attachments/assets/33ad751d-2ec6-426c-a11a-2dd4c6527ec7" />

<img width="688" height="782" alt="Image" src="https://github.com/user-attachments/assets/4f424025-eeaa-4761-8d45-dea6477cf3bd" />

<img width="688" height="806" alt="Image" src="https://github.com/user-attachments/assets/8d2e351a-9c2f-4037-8fa4-eb7e07678f7c" />

<img width="535" height="814" alt="Image" src="https://github.com/user-attachments/assets/d3021ecb-fce0-45e9-88b7-ba0662db6b07" />

<img width="655" height="917" alt="Image" src="https://github.com/user-attachments/assets/deaa5002-d1af-44d6-bbf3-edee286cb4c4" />

<img width="526" height="906" alt="Image" src="https://github.com/user-attachments/assets/ff72b435-4d20-4754-b624-c1243a58171e" />
Conclusion

The Gazette Exchange System provides a simple and effective way to exchange goods without money.

It:

Improves resource utilization
Encourages community sharing
Demonstrates real-world database implementation
Showcases UI design and C# integration.

















