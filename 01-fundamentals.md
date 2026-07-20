# 🎨 Tutorial 1 – Project Setup & Architecture (Easy Tanglish)

## 🔥 Intha Tutorial-la Enna Learn Pannuvom?

Namma **Smart Wall Paint Visualizer** project eppadi work aguthu nu full understanding varum.

### Learn pannuvom:

- 📂 Project Folder Structure
- 🚀 Angular & Express Connection
- 🗄 MongoDB Usage
- ☁ Cloudinary Image Storage
- 🔄 Socket.IO Realtime Updates
- ⚡ Angular Signals
- 🧩 Standalone Components

---

# 🏗 Overall Architecture

```
                Angular Client
                      │
        HTTP API + Socket.IO
                      │
               Express Backend
               /             \
              /               \
      MongoDB Atlas       Cloudinary
```

Simple Flow

```
Frontend

↓

Backend API

↓

Database

↓

Image Storage
```

---

# 📂 Project Folder Structure

```
project/

│

├── angular-client/
│      Frontend

│

├── express-server/
│      Backend

│

└── README.md
```

---

# 🖥 Angular Client

Ithu than user browser-la paakura website.

Contains

- Login
- Register
- Dashboard
- Canvas Editor
- Admin Panel
- Projects

Runs on

```
http://localhost:4200
```

---

# ⚙ Express Backend

Backend ellaa request-um handle pannum.

Example

```
Login

↓

Check User

↓

MongoDB

↓

Generate JWT

↓

Return Response
```

Runs on

```
http://localhost:5000
```

---

# 🗄 MongoDB

Database.

Store pannuvom

- Users
- Projects
- Colors
- Textures
- Assets
- Audit Logs

Example

```
Register

↓

Save User

↓

MongoDB
```

---

# ☁ Cloudinary

Images local system-la save panna maatom.

Flow

```
Upload Image

↓

Cloudinary

↓

Returns URL

↓

Store URL in MongoDB
```

Example

```
Original Image

↓

Cloudinary

↓

https://res.cloudinary.com/....

↓

Database
```

---

# 🔄 Socket.IO

Realtime Communication.

Example

```
User A Paint

↓

Socket.IO

↓

User B Screen Update

↓

No Refresh Required
```

---

# ⚡ Angular Signals

Old Angular

```
Observable

BehaviorSubject

subscribe()
```

Latest Angular

```
signal()
```

Example

```ts
activeTool = signal('brush');
```

Change

```ts
activeTool.set('eraser');
```

Read

```ts
activeTool();
```

No subscribe needed.

---

# 🧩 Standalone Components

Old Angular

```
AppModule
```

Latest Angular

Every Component Independent.

Example

```
LoginComponent

DashboardComponent

CanvasEditorComponent

AdminComponent
```

Benefits

- Less Code
- Faster Loading
- Easy Maintenance

---

# 🚀 Local Setup

## Step 1

Open

```
express-server
```

Create

```
.env
```

Paste

```env
PORT=5000

MONGODB_URI=your_mongodb_url

JWT_SECRET=your_secret

CLOUDINARY_CLOUD_NAME=xxxx

CLOUDINARY_API_KEY=xxxx

CLOUDINARY_API_SECRET=xxxx
```

---

## Step 2

Run Backend

```bash
cd express-server

npm install

npm run dev
```

Expected Output

```
Server Running

MongoDB Connected

Socket.IO Ready

Port 5000
```

---

## Step 3

Open another terminal

```bash
cd angular-client

npm install

npm start
```

Browser

```
http://localhost:4200
```

---

# 🔑 Login Flow

```
Angular Login Page

↓

POST /api/auth/login

↓

Express API

↓

MongoDB

↓

Password Verify

↓

Generate JWT

↓

Return Token

↓

Dashboard Opens
```

---

# 🎨 Canvas Flow

```
Upload Image

↓

Cloudinary Upload

↓

Store URL

↓

MongoDB

↓

Canvas Editor

↓

Apply Paint

↓

Autosave

↓

MongoDB Update
```

---

# 💾 Save Flow

```
Draw

↓

Canvas JSON

↓

Express API

↓

MongoDB

↓

Saved Successfully
```

---

# 🧪 Practice

## Create Health API

```ts
app.get("/api/health", (req, res) => {
  res.json({
    status: "ok"
  });
});
```

Open Browser

```
http://localhost:5000/api/health
```

Output

```json
{
  "status": "ok"
}
```

---

## Angular Service

```ts
this.http.get('/api/health');
```

Display

```
🟢 Server Status : OK
```

---

# 🧠 Easy Memory Trick

```
Angular

↓

Express

↓

MongoDB

↓

Cloudinary

↓

Socket.IO
```

---

# 📌 One-Line Summary

| Technology | Purpose |
|------------|----------|
| Angular | User Interface (Frontend) |
| Express | Backend APIs & Business Logic |
| MongoDB | Database |
| Cloudinary | Image Storage |
| Socket.IO | Realtime Updates |
| JWT | Authentication |
| Signals | State Management |
| Standalone Components | Independent Angular Components |

---

# 🎯 Final Flow

```
User

↓

Angular UI

↓

HTTP Request

↓

Express API

↓

MongoDB

↓

Cloudinary (Images)

↓

Response

↓

Angular UI Update
```

---

## ✅ End of Tutorial 1

If you understand this architecture, the rest of the Smart Wall Paint Visualizer project will be much easier to learn and build.
