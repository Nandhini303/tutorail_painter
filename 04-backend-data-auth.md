# Tutorial 4: Backend Data & Auth

### What you'll learn:
- MongoDB schema design with Mongoose
- JSON Web Token (JWT) Authentication
- Cloudinary Integration for Assets

**Prerequisites:** [Tutorial 3: Realtime Collaboration (Socket.IO)](./03-realtime-collaboration.md)

---

## 1. MongoDB Schema Design
We use Mongoose to define strict schemas. Here is how we define a Project that stores Konva layer data.

```typescript
// src/models/Project.ts
const ProjectSchema = new Schema({
  name: { type: String, required: true },
  owner: { type: Schema.Types.ObjectId, ref: 'User', required: true },
  canvasState: { type: String, default: '[]' },
  collaborators: [{ type: Schema.Types.ObjectId, ref: 'User' }]
}, { timestamps: true });
```

## 2. JWT Authentication
We protect our routes using a custom auth middleware that verifies JWTs.

```typescript
// src/middleware/authMiddleware.ts
export const protect = (req: AuthenticatedRequest, res: Response, next: NextFunction) => {
  const token = req.headers.authorization?.split(' ')[1];
  
  if (!token) return res.status(401).json({ message: 'Not authorized' });

  try {
    const decoded = jwt.verify(token, process.env.JWT_SECRET!);
    req.user = decoded;
    next();
  } catch (err) {
    res.status(401).json({ message: 'Token failed' });
  }
};
```

## 3. Cloudinary Upload Flow
When users upload custom wall textures in the Angular `asset-library-panel.html`, the Express API proxies it directly to Cloudinary using `multer-storage-cloudinary`.

```typescript
// src/routes/upload.routes.ts
import { upload } from '../config/cloudinary';

router.post('/upload', protect, upload.single('file'), (req, res) => {
  res.status(201).json({
    url: req.file.path,
    publicId: req.file.filename
  });
});
```

---

### Try it yourself!
*Exercise:* Extend the `User` schema to include a `role` field (e.g., 'admin' or 'designer') and implement a `protectAdmin` middleware!