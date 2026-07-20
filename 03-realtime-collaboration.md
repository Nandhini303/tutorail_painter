# 🎨 Tutorial 3: Realtime Collaboration (Socket.IO)

📘 **What you'll learn:**
- Emitting and listening to events in Angular
- Syncing live drawing data across multiple clients

**Prerequisites:** [Tutorial 2: Canvas Engine (Konva.js)](./02-canvas-engine.md)

> **📖 New terms in this chapter:**
> - **Socket.IO:** A library that enables persistent, bi-directional communication between a web client and server (using WebSockets).
> - **Emit:** Sending an event/data message through the socket.
> - **Broadcast:** Sending a message to everyone *except* the sender.

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

## 📘 Learn: Event Sequence

When one user draws, everyone else sees it instantly without refreshing.

```mermaid
sequenceDiagram
    participant User A
    participant Server
    participant User B
    
    User A->>Server: emit('layer-update', canvasData)
    Server-->>User B: emit('layer-update-broadcast', canvasData)
    Note over User B: Updates Konva Layer
```

---

## 🛠️ Build: Real-time Architecture

**Step 1. Server Event Handlers**
The backend needs to receive data from one client and bounce it to others in the same room.

```typescript
// file: express-server/src/sockets/socketHandler.ts
export const setupSocketHandlers = (io: Server) => {
  io.on('connection', (socket) => {
    socket.on('join-project', (projectId) => {
      socket.join(projectId);
    });

    socket.on('layer-update', (data) => {
      // Broadcast to everyone else in the room!
      socket.to(data.projectId).emit('layer-update-broadcast', data);
    });
  });
};
```
![step-1](./images/03-step-1.png)

**Step 2. Client Socket Service**
In Angular, we centralize this logic into a reusable service.

```typescript
// file: angular-client/src/app/services/socket.service.ts
import { Injectable } from '@angular/core';
import { io, Socket } from 'socket.io-client';

@Injectable({ providedIn: 'root' })
export class SocketService {
  private socket: Socket = io('https://wall-painter.onrender.com');

  joinProjectRoom(projectId: string): void {
    this.socket.emit('join-project', projectId);
  }

  on(eventName: string, callback: any): void {
    this.socket.on(eventName, callback);
  }
}
```

**Step 3. Receiving Data in the Canvas**
When the canvas receives an update, it must redraw itself.

```typescript
// file: angular-client/src/app/features/canvas-editor/canvas-editor.component.ts
ngOnInit() {
  this.socketService.on('layer-update-broadcast', (data) => {
    // Prevent infinite loop by flagging this as a remote update!
    this.isRemoteUpdate = true; 
    this.loadCanvasData(data.layers);
  });
}
```
![step-3](https://images.unsplash.com/photo-1522071820081-009f0129c71c?q=80&w=800&auto=format&fit=crop)

---

## 🧪 Practice: Build It Yourself

**Goal:** Broadcast a new custom Socket.IO event (e.g., user typing indicator or live cursors).

1. Listen to the Konva stage `mousemove` event.
2. Throttle the event (so you don't spam the server) and emit the X/Y coordinates.
3. Have other clients listen for these coordinates and render a tiny colored circle at that spot.

**✅ Check yourself:**
- [ ] Did you remember to throttle the `mousemove` event?
- [ ] Are coordinates being broadcasted successfully to the server?
- [ ] Do you see the other user's cursor moving on your screen?