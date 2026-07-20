# Tutorial 3: Realtime Collaboration (Socket.IO)

### What you'll learn:
- Emitting and listening to events in Angular
- Syncing live drawing data across multiple clients

**Prerequisites:** [Tutorial 2: Canvas Engine (Konva.js)](./02-canvas-engine.md)

---

## 1. Client-Side Socket Service
To keep our Angular components clean, we encapsulate all real-time logic in `socket.service.ts`.

```typescript
// src/app/services/socket.service.ts
import { Injectable } from '@angular/core';
import { io, Socket } from 'socket.io-client';

@Injectable({ providedIn: 'root' })
export class SocketService {
  private socket!: Socket;
  private serverUrl = 'https://wall-painter.onrender.com';

  constructor() {
    this.socket = io(this.serverUrl, { autoConnect: false });
  }

  joinProjectRoom(projectId: string): void {
    if (!this.socket.connected) this.socket.connect();
    this.socket.emit('join-project', projectId);
  }

  on(eventName: string, callback: any): void {
    if (this.socket) {
      this.socket.on(eventName, callback);
    }
  }
}
```

## 2. Server-Side Handlers
On the Express side, we listen for these events in `socketHandler.ts` and broadcast them to everyone else in the specific project room.

```typescript
// src/sockets/socketHandler.ts
export const setupSocketHandlers = (io: Server) => {
  io.on('connection', (socket) => {
    socket.on('join-project', (projectId) => {
      socket.join(projectId);
    });

    socket.on('layer-update', (data) => {
      // Broadcast to everyone else in the room
      socket.to(data.projectId).emit('layer-update-broadcast', data);
    });
  });
};
```

## 3. Avoiding Feedback Loops
When receiving a `layer-update-broadcast` in Angular, it is critical to update the canvas *without* re-triggering another `layer-update` emission, or you'll create an infinite loop!

---

### Try it yourself!
*Exercise:* Implement a "Live Cursors" feature by emitting mouse coordinates in `canvas-editor.component.ts` (`mousemove`) and rendering tiny cursors for other users.