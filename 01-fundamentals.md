# 🎨 Tutorial 1: Project Setup & Architecture (Beginner Guide)

📘 **What you'll learn:**
- How a "monorepo" is structured (Angular 17 + Express)
- Exact terminal commands to install and run both the frontend and backend locally
- An introduction to Angular 17 Signals & Standalone components
- An introduction to the Express API bootstrap flow

**Prerequisites:** Absolutely none! Just basic knowledge of what a website is.

> **📖 New terms in this chapter:**
> - **Monorepo:** A single main folder (repository) that contains two separate projects inside it (in our case, the frontend Angular app and the backend Express server).
> - **Signals:** A new, easy way in Angular to store changing data (like whether a tool is selected) so the screen updates instantly.
> - **Standalone Components:** Modern Angular pieces that work all by themselves without needing confusing `NgModule` configuration files.
> - **NPM (Node Package Manager):** A tool that downloads other people's code (packages) so you can use them in your app.

---

## 📘 Learn: System Architecture

Before we type any code, let's look at how the Smart Wall Painter app is built. It has two halves that talk to each other:

```mermaid
graph LR
    A[Angular 17 Client<br/>(Your Browser)] <-->|Sends drawing data| B(Express Server<br/>(The Backend))
    B <-->|Saves designs| C[(MongoDB Atlas<br/>Database)]
    B <-->|Uploads pictures| D[Cloudinary<br/>Image Hosting]
    A -->|Uploads| D
```

---

## 🛠️ Build: Running Locally Step-by-Step

Let's get this app running on your own computer! Follow every single command carefully.

### Step 1. Get the Code
First, open your computer's terminal (or Command Prompt / PowerShell) and type these exact commands to download the project and go inside the folder:

```bash
# 1. Download the code from GitHub
git clone https://github.com/Nandhini303/wall_painter.git

# 2. Go into the folder you just downloaded
cd wall_painter
```

### Step 2. Configure the Backend Environment
The backend needs a `.env` file to know your passwords and database links. 

Inside your terminal, go into the backend folder and create a `.env` file. You can do this in VS Code or by typing:

```bash
# 1. Go into the backend server folder
cd express-server
```

Create a file named EXACTLY `.env` inside the `express-server` folder, and paste this inside:

```text
// file: express-server/.env
PORT=5000
MONGODB_URI=your_mongo_url
JWT_SECRET=supersecret
CLOUDINARY_URL=your_cloudinary_url
```
![step-1](https://images.unsplash.com/photo-1555066931-4365d14bab8c?q=80&w=800&auto=format&fit=crop)

### Step 3. Start the Backend Server
Now that we are inside the `express-server` folder, we need to install the required packages and start the engine!

```bash
# 1. Download all the required Node packages for the backend
npm install

# 2. Start the backend server!
npm run dev
```
*If it works, your terminal will say: "Smart Wall Paint Visualizer Backend listening on port 5000". Leave this terminal open!*

### Step 4. Start the Frontend Client
We need a **second** terminal to run the visual frontend. 

1. Open a **new terminal tab** (or window).
2. Make sure you navigate back to the main `wall_painter` folder.
3. Then type these commands:

```bash
# 1. Go into the frontend Angular folder
cd angular-client

# 2. Download all the required Node packages for the frontend
npm install

# 3. Start the Angular website!
npm start
```
*If it works, your terminal will say it compiled successfully. Open your web browser and go to `http://localhost:4200` to see your app!*

![step-3](./images/01-step-3.png)

---

## 📘 Learn: The Code Setup

Now that it's running, let's look at the files that make it work.

### Angular 17 Signals (The Frontend)
In this app, we use Signals to manage what the user is doing. Look at the canvas editor file. Notice how simple `signal(...)` makes it to store the currently selected drawing tool:

```typescript
// file: angular-client/src/app/features/canvas-editor/canvas-editor.component.ts
export class CanvasEditorComponent {
  // This signal remembers what tool you clicked on!
  activeTool = signal<'select' | 'pan' | 'draw' | 'wall' | 'erase'>('select');
  
  // This signal remembers if you can click the Undo button
  canUndo = signal(false);
}
```

### Express Bootstrap (The Backend)
Our backend is initialized cleanly with Express and Socket.IO working together on the same port. Open `index.ts` to see how it starts up:

```typescript
// file: express-server/src/index.ts
import express from 'express';
import http from 'http';
import { Server } from 'socket.io';

// 1. Create the backend app
const app = express();
const server = http.createServer(app);

// 2. Attach Socket.IO for real-time collaboration
const io = new Server(server, { cors: { origin: '*' } });
```

---

## 🧪 Practice: Build It Yourself

**Goal:** Add a new Express route and call it from Angular.

1. **Backend:** Open `express-server/src/index.ts`. Add a new route right before `app.listen()` that looks like this:
   ```typescript
   app.get('/api/hello', (req, res) => {
     res.json({ message: "Hello from the backend!" });
   });
   ```
2. **Frontend:** Open any Angular component and try to fetch `http://localhost:5000/api/hello` using the browser's `fetch()` API.

**✅ Check yourself:**
- [ ] Did you restart the Express server after adding the route?
- [ ] Does navigating to `http://localhost:5000/api/hello` work in your web browser?
- [ ] Were you able to display "Hello from the backend!" on your Angular dashboard?
