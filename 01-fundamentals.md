# Tutorial 1: Project Setup & Architecture

### What you'll learn:
- How this monorepo is structured (Angular 17 + Express)
- How to run both servers locally
- Angular 17 Signals & Standalone components in action
- The Express API bootstrap flow

**Prerequisites:** Basic knowledge of TypeScript and web development.

---

## 1. Repository Structure
This project is divided into two main folders:
- `angular-client/`: The frontend Angular 17 application using standalone components and Signals.
- `express-server/`: The Node.js Express backend using Socket.IO and MongoDB.

## 2. Running Locally

Before running, ensure you have a `.env` file in `express-server/` with your MongoDB URI, JWT Secret, and Cloudinary keys.

Start the backend:
```bash
cd express-server
npm install
npm run dev
```

Start the frontend:
```bash
cd angular-client
npm install
npm start
```

## 3. Angular 17 Standalone & Signals
In this repo, we use the latest Angular features. Instead of `NgModules`, components like the Canvas Editor pull in exactly what they need.

Notice how we use **Signals** in `canvas-editor.component.ts`:
```typescript
// src/app/features/canvas-editor/canvas-editor.component.ts
export class CanvasEditorComponent implements OnInit, OnDestroy {
  activeTool = signal<'select' | 'pan' | 'draw' | 'wall' | 'erase'>('select');
  canUndo = signal(false);
  canRedo = signal(false);
  
  // ...
```
Signals provide a simpler, more reactive way to manage state without complex RxJS observables!

## 4. Express Bootstrap
Our API is bootstrapped in `express-server/src/index.ts`. Here's a snippet showing how we combine Express, Rate Limiting, and Socket.IO:

```typescript
// src/index.ts
import express from 'express';
import http from 'http';
import { Server } from 'socket.io';

const app = express();
const server = http.createServer(app);
const io = new Server(server, {
  cors: { origin: '*', methods: ['GET', 'POST', 'PUT', 'DELETE'] }
});

// Setup Socket.IO
setupSocketHandlers(io);
```

---

### Try it yourself!
*Exercise:* Create a new Signal in `canvas-editor.component.ts` called `backgroundColor` and bind it to the stage background!