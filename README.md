# Contest-Tracker

## 1. Functional Requirements

### Core Features

1. **Fetch Upcoming & Past Contests**  
   - Integrate with **Codeforces, CodeChef, and Leetcode APIs** (or scrape if needed).  
   - Store contest data in the database.  
   - Show **contest name, platform, start time, end time, countdown timer**.  
   - Display **past contests (only from the last 1 week)**.  

2. **Filtering & Bookmarking**  
   - Allow users to filter by **platform** (Codeforces, CodeChef, Leetcode).  
   - Users should be able to **bookmark contests** for easy access later.  

3. **Add Solution Links**  
   - Provide a form where **admins** can manually attach **YouTube solution links** for past contests.  
   - **(Bonus)** Automatically fetch YouTube links when solutions are uploaded.  

4. **Reminders for Upcoming Contests**  
   - Allow users to **subscribe for reminders** via **email or SMS**.  
   - Users can **select when to receive notifications** (e.g., 1 hour or 30 minutes before the contest).  
   - Use **scheduled jobs** to send reminders.  

---

## 2. Non-Functional Requirements

- **Scalability:** Should handle multiple users fetching contests at the same time.  
- **Performance:** Use caching (Redis) to reduce API calls.  
- **Security:** Secure APIs with JWT authentication (for user actions like bookmarking, reminders).  
- **Availability:** Deploy backend (Spring Boot) on **AWS/Render** and frontend (React) on **Vercel/Netlify**.  
- **Database Choice:** PostgreSQL or MySQL for structured contest/user data.  

---

## 3. High-Level Design (HLD)

### Architecture Diagram

```
                        +--------------------+
                        |   User (Frontend)  |
                        +---------+----------+
                                  |
                                  ▼
                        +--------------------+
                        |   React Frontend   |
                        | (Next.js / Vite +  |
                        | Tailwind CSS)      |
                        +---------+----------+
                                  |
                                  ▼
                        +--------------------+
                        |  Spring Boot API   |
                        | (RESTful Services) |
                        +---------+----------+
                                  |
      ---------------------------------------------------
      |                  |                |            |
      ▼                  ▼                ▼            ▼
+-------------+    +-------------+  +-------------+  +----------------+
| Contest API |    | User API    |  | Reminder API|  | YouTube API    |
+-------------+    +-------------+  +-------------+  +----------------+
      |                  |                |            |
      ▼                  ▼                ▼            ▼
+-------------+    +-------------+  +-------------+  +----------------+
|  PostgreSQL |    | Redis Cache |  | Twilio API  |  | YouTube Data   |
+-------------+    +-------------+  +-------------+  +----------------+
      |                 
      ▼                 
+--------------------------------+
|  External Contest APIs        |
|  (Codeforces, CodeChef, LC)   |
+--------------------------------+
```

---

## 4. API Endpoints

| Method | Endpoint | Description |
|--------|---------|-------------|
| `GET`  | `/contests/upcoming` | Fetch upcoming contests from Codeforces, CodeChef, Leetcode |
| `GET`  | `/contests/past` | Fetch past contests (last 1 week) |
| `GET`  | `/contests?platform=codeforces` | Fetch contests from a specific platform |
| `POST` | `/bookmarks` | Bookmark a contest |
| `GET`  | `/bookmarks` | Get bookmarked contests |
| `POST` | `/youtube-links` | Attach YouTube solution to past contest |
| `POST` | `/subscribe` | Subscribe for contest reminders (email/SMS) |
| `POST` | `/reminders/send` | Send reminders before contest |

---
