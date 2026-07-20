# 🎨 Tutorial 4: Backend Data & Auth

📘 **What you'll learn:**
- MongoDB schema design with Mongoose
- JSON Web Token (JWT) Authentication
- Cloudinary Integration for Assets

**Prerequisites:** [Tutorial 3: Realtime Collaboration (Socket.IO)](./03-realtime-collaboration.md)

> **📖 New terms in this chapter:**
> - **JWT (JSON Web Token):** A secure, encrypted string that identifies a logged-in user without needing server sessions.
> - **Mongoose:** An ODM (Object Data Modeling) library for MongoDB that enforces strict schemas.
> - **Cloudinary:** A cloud platform that acts as a CDN for our uploaded image assets.

---


---
### 📚 Official Documentation & Account Creation Links
Before proceeding, make sure you have your required free accounts set up and documentation ready:

**📚 Official Documentation Links:**
- **[MongoDB Docs](https://www.mongodb.com/docs/)** - The NoSQL database used to store our project data.
- **[Express.js Docs](https://expressjs.com/)** - The web framework for our Node.js backend.
- **[Angular Docs](https://angular.dev/)** - The frontend framework for our UI.
- **[Node.js Docs](https://nodejs.org/en/docs/)** - The JavaScript runtime for our backend.
- **[Socket.IO Docs](https://socket.io/docs/v4/)** - Real-time communication for live collaboration.

**🔑 Account Creation Steps:**
1. **MongoDB Atlas (Database)**
   - Go to [mongodb.com/cloud/atlas/register](https://www.mongodb.com/cloud/atlas/register) and sign up for a free account.
   - Create a new "Cluster" (the free `M0` tier is perfect).
   - Once created, click "Connect", choose "Drivers", and copy your Connection String (it looks like `mongodb+srv://...`). This will be your `MONGODB_URI`.
   - *Make sure you replace `<password>` in the URL with your actual database user password!*

2. **Cloudinary (Image Hosting)**
   - Go to [cloudinary.com/users/register/free](https://cloudinary.com/users/register/free) and sign up.
   - On your dashboard, you will see a section called "API Environment variable".
   - Copy the URL (it looks like `cloudinary://API_KEY:API_SECRET@CLOUD_NAME`). This will be your `CLOUDINARY_URL`.
---

## 📘 Learn: Request & Auth Flow

```mermaid
graph TD
    A[Client UI] -->|1. POST /login| B[Express Auth Controller]
    B -->|2. Check Hash| C[(MongoDB Users)]
    B -->|3. Return JWT| A
    A -->|4. Bearer Token Header| D[Protected Routes]
    D -->|5. Verify Token| E[Access Granted]
```

---

## 🛠️ Build: Schemas and Auth

**Step 1. Defining a Mongoose Schema**
We define what a Project should look like in the database.

```typescript
// file: express-server/src/models/Project.ts
import mongoose, { Schema } from 'mongoose';

const ProjectSchema = new Schema({
  name: { type: String, required: true },
  owner: { type: Schema.Types.ObjectId, ref: 'User', required: true },
  canvasState: { type: String, default: '[]' }
}, { timestamps: true });

export const Project = mongoose.model('Project', ProjectSchema);
```

**Step 2. Creating the Auth Middleware**
This protects our routes by enforcing JWT checks.

```typescript
// file: express-server/src/middleware/authMiddleware.ts
export const protect = (req: AuthenticatedRequest, res: Response, next: NextFunction) => {
  const token = req.headers.authorization?.split(' ')[1];
  if (!token) return res.status(401).json({ message: 'Not authorized' });

  try {
    req.user = jwt.verify(token, process.env.JWT_SECRET!);
    next();
  } catch (err) {
    res.status(401).json({ message: 'Invalid token' });
  }
};
```
![step-2](./images/04-step-2.png)

**Step 3. Cloudinary Upload Route**
We proxy asset uploads directly to Cloudinary using `multer`.

```typescript
// file: express-server/src/routes/upload.routes.ts
import { upload } from '../config/cloudinary';

router.post('/upload', protect, upload.single('file'), (req, res) => {
  res.status(201).json({
    url: req.file.path,
    publicId: req.file.filename
  });
});
```
![step-3](https://images.unsplash.com/photo-1544197150-b99a580bb7a8?q=80&w=800&auto=format&fit=crop)

---

## 🧪 Practice: Build It Yourself

**Goal:** Add a new protected MongoDB collection + CRUD route with a JWT check.

1. Create a `Comment` schema (text, author, projectId).
2. Build an Express router for `GET /api/comments` and `POST /api/comments`.
3. Protect the `POST` route with the `protect` middleware.

**✅ Check yourself:**
- [ ] Does sending a POST request without a token fail with a 401 error?
- [ ] Does sending it *with* a valid token correctly save to the database?
- [ ] Can you fetch the comments via the GET route?