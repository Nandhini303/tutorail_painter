# 🎨 Tutorial 5: Production Deployment (Hosting - Tanglish)

![Mascot](./images/mascot.png)

*“Namma computer-la work aaguthu, but ulagathula ellarum pakkanum la? Vercel and Render use panni host panniduvom! (From localhost to global!)”*

📘 **What you'll learn (Enna kethuka porom):**
- Vercel (Frontend hosting).
- Render (Backend Node.js hosting).

**Prerequisites:** [Tutorial 4](./04-backend-data-auth.md).


> **📚 Official Links & Accounts (Munbe Ready Pannidunga!)**
> - **MongoDB Atlas:** Create a free cluster at [mongodb.com/cloud/atlas/register](https://www.mongodb.com/cloud/atlas/register). Get your `MONGODB_URI`.
> - **Cloudinary:** Create a free account at [cloudinary.com/users/register/free](https://cloudinary.com/users/register/free). Get your `CLOUDINARY_URL`.


---

## 📘 Learn: Hosting Architecture

```mermaid
graph LR
    A[World Wide Web] -->|Visits UI| B[Vercel (Angular)]
    B -->|API Calls| C[Render (Express)]
    C -->|Stores Data| D[(MongoDB Atlas)]
```

---

## 🛠️ Build: Deployment Steps

### Step 1. Vercel Configuration (Angular)
Angular SPA-va Vercel-la host panna, `vercel.json` mukkiyam, illati refresh panna 404 error varum.

```json
// file: angular-client/vercel.json
{
  "outputDirectory": "dist/angular-client/browser",
  "rewrites": [ { "source": "/(.*)", "destination": "/index.html" } ]
}
```

### Step 2. Render Environment
Render-la Node app host panna, build command correct-a kudukanum:

**Build Command:** `npm install --legacy-peer-deps && npm run build`
**Start Command:** `npm start`

*Kandippa `.env` variables ellam Render dashboard-la add pannidanum!*

### Step 3. Pointing Angular to Render
Localhost-ku badhila, live URL-ku mathanum.

```typescript
// file: angular-client/src/app/services/socket.service.ts
private serverUrl = 'https://wall-painter.onrender.com';
```

---

## 🧪 Practice: Build It Yourself

**Goal:** Unga sondha github repo-va Vercel-la connect panni deploy pannunga!

**✅ Check yourself:**
- [ ] Vercel link-la app load aagutha?
- [ ] Drawing panna, Render backend-la data poyitu varutha?