# 🎨 Tutorial 1: Project Setup & Architecture (Beginner + Tanglish Guide)

![Mascot](./images/mascot.png)

*“Vanga palagalam! Welcome to the Smart Wall Painter tutorial series! Namma inikku zero-la irundhu hero aaga porom. Oru periya Full-Stack (Angular + Express) app-a eppadi scratch-la irundhu start panni run panrathu nu paapom!”*

📘 **What you'll learn (Enna kethuka porom):**
- Monorepo structure eppadi work aaguthu.
- `mkdir` la irundhu frontend/backend run panra varaikum EXACT terminal commands.
- Angular 17 Signals & Express basics.

**Prerequisites:** System-la Node.js install aagirkanum. Avlodhan!

> **📖 New terms in this chapter (Pudhu Varthaigal):**
> - **Monorepo:** Oru main folder-kulla rendu thani thani projects (namma frontend & backend) irukkurathu.
> - **Signals:** Angular-la pudhusa vantha oru trick. Data change aana udane screen-a update panna ithu romba easy.


> **📚 Official Links & Accounts (Munbe Ready Pannidunga!)**
> - **MongoDB Atlas:** Create a free cluster at [mongodb.com/cloud/atlas/register](https://www.mongodb.com/cloud/atlas/register). Get your `MONGODB_URI`.
> - **Cloudinary:** Create a free account at [cloudinary.com/users/register/free](https://cloudinary.com/users/register/free). Get your `CLOUDINARY_URL`.


---

## 📘 Learn: Working Flow (Eppadi work aaguthu?)

Oru chinna diagram paapom. Namma app eppadi pesikuthu nu puriyum:

```mermaid
graph LR
    A[Angular 17 (Browser)] <-->|Sockets (Real-time)| B(Express Server)
    B <-->|Mongoose| C[(MongoDB Database)]
    A -->|Upload| D[Cloudinary (Images)]
```

---

## 🛠️ Build: Step-by-Step Execution (Unga pc-la run pannuvom)

*“Ellam command um correct-a type pannunga, onnu kooda miss panna koodathu!”*

### Step 1. Folders Create Pannuvom
Terminal open panni, oru pudhu folder create pannunga:

```bash
# 1. Folder create pannunga
mkdir my-super-app

# 2. Antha folder-kulla ponga
cd my-super-app
```

*(Actually, neenga existing code-a download pannalam using `git clone https://github.com/Nandhini303/wall_painter.git`, but structure puriyurathukaga namma empty-a yosipom. Cloning dhaan best!)*

### Step 2. Backend Setup
Backend folder-kulla poittu `.env` file create pannanum.

```bash
cd express-server
```

File peru exactly `.env` nu irukanum. Itha ulle podunga:
```text
// file: express-server/.env
PORT=5000
MONGODB_URI=your_mongo_url
JWT_SECRET=supersecret
CLOUDINARY_URL=your_cloudinary_url
```

### Step 3. Backend-a Run Pannuvom
Dependencies install panni start pannalam!
```bash
npm install
npm run dev
```
*Terminal-la "Backend listening on port 5000" nu varum!*

### Step 4. Frontend-a Run Pannuvom
Pudhu terminal open pannunga! Pazhaiyatha close pannidathinga.
```bash
cd angular-client
npm install
npm start
```
*Browser-la `http://localhost:4200` open panni paathinga na, namma UI theriyum!*

![Enhanced UI](./images/ui_mockup.png)

---

## 🧪 Practice: Build It Yourself (Neengale Try Pannunga!)

**Goal:** Oru pudhu Express route add panni, Angular-la call pannunga.

1. Backend `index.ts`-la `app.get('/api/test')` add pannunga.
2. Angular-la fetch panni console.log-la antha data-va print pannunga.

**✅ Check yourself:**
- [ ] Backend terminal error illama run aagutha?
- [ ] Angular console-la data vanthuducha?