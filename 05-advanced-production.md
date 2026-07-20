# Tutorial 5: Advanced & Production Considerations

### What you'll learn:
- Deploying frontend to Vercel
- Deploying backend to Render
- Environment variable management

**Prerequisites:** [Tutorial 4: Backend Data & Auth](./04-backend-data-auth.md)

---

## 1. Vercel Frontend Deployment
Angular Single Page Applications (SPAs) need specific routing configurations on Vercel so that deep links don't return 404s. We solve this by adding a `vercel.json` to `angular-client/`:

```json
// angular-client/vercel.json
{
  "rewrites": [
    {
      "source": "/(.*)",
      "destination": "/index.html"
    }
  ]
}
```
*Pro-tip:* Ensure the Vercel **Output Directory** is set to `dist/angular-client/browser` (for Angular 17+)!

## 2. Render Backend Deployment
Render natively builds Node.js apps. However, it requires all TypeScript devDependencies to be installed during the production build. 

We ensure our `package.json` has `typescript` in dependencies, and we set our build command to:
```bash
npm install --legacy-peer-deps && npm run build
```

## 3. Environment Variables
In production, never hardcode URLs. In `socket.service.ts` and `auth.service.ts`, we updated the API targets to point to the live Render URL:

```typescript
// src/app/services/socket.service.ts
private serverUrl = 'https://wall-painter.onrender.com';
```
For security, tokens and database URIs must be stored securely in Render/Vercel dashboard environment settings.

---

### Try it yourself!
*Exercise:* Create an Angular `environment.ts` file to dynamically switch `serverUrl` between `http://localhost:5000` and `https://wall-painter.onrender.com` based on the build type!