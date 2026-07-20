# 🎨 Tutorial 5: Advanced & Production Considerations

📘 **What you'll learn:**
- Deploying frontend to Vercel
- Deploying backend to Render
- Environment variable management

**Prerequisites:** [Tutorial 4: Backend Data & Auth](./04-backend-data-auth.md)

> **📖 New terms in this chapter:**
> - **Vercel:** A modern hosting platform optimized for frontend frameworks (like Angular).
> - **Render:** A cloud provider optimized for running backend services (like Node.js servers) via Docker or native runtimes.
> - **SPA (Single Page Application):** Web apps that dynamically rewrite the current page rather than loading new pages from a server.

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

## 📘 Learn: Production Architecture

```mermaid
graph LR
    A[Browser] -->|HTTPS| B[Vercel Edge Network]
    B -->|Serves Angular| A
    A -->|API Calls / WSS| C[Render Node.js Instance]
    C -->|Secure Connection| D[(MongoDB Atlas)]
```

---

## 🛠️ Build: Deployment Configs

**Step 1. Vercel SPA Routing**
Because Angular is an SPA, if a user refreshes the page on `/dashboard`, Vercel will throw a 404. We fix this by rewriting all requests to `index.html`.

```json
// file: angular-client/vercel.json
{
  "outputDirectory": "dist/angular-client/browser",
  "rewrites": [
    {
      "source": "/(.*)",
      "destination": "/index.html"
    }
  ]
}
```
![step-1](./images/05-step-1.png)

**Step 2. Render Build Command**
Render builds Node apps automatically, but it defaults to a production environment that strips out TypeScript compiler dependencies. We force it to keep them!

In your Render dashboard settings:
```bash
# Build Command
npm install --legacy-peer-deps && npm run build

# Start Command
npm start
```

**Step 3. Environment Variable Routing**
Your frontend needs to know to talk to Render, not localhost.

```typescript
// file: angular-client/src/app/services/socket.service.ts
import { isDevMode } from '@angular/core';

@Injectable({ providedIn: 'root' })
export class SocketService {
  private serverUrl = isDevMode() 
    ? 'http://localhost:5000' 
    : 'https://wall-painter.onrender.com';
    
  // ...
}
```
![step-3](https://images.unsplash.com/photo-1451187580459-43490279c0fa?q=80&w=800&auto=format&fit=crop)

---

## 🧪 Practice: Build It Yourself

**Goal:** Deploy your fork to Vercel + Render and verify env vars work.

1. Connect your GitHub repo to Render and deploy the Express app.
2. Connect your GitHub repo to Vercel and deploy the Angular app.
3. Test a full round-trip drawing operation on the live URLs.

**✅ Check yourself:**
- [ ] Does the Vercel site load without 404 errors?
- [ ] Does drawing on the Vercel site emit socket events to the Render backend?
- [ ] Can a friend on a different computer see your drawings in real-time?