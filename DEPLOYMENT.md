# Milano SA Fleet - Deployment Guide

## 🚀 Quick Deployment Options

### 1. Vercel (Recommended)
[![Deploy with Vercel](https://vercel.com/button)](https://vercel.com/new)

```bash
# Install Vercel CLI
npm i -g vercel

# Deploy
vercel
```

### 2. Netlify
[![Deploy to Netlify](https://www.netlify.com/img/deploy/button.svg)](https://app.netlify.com/start)

```bash
# Install Netlify CLI
npm i -g netlify-cli

# Build and deploy
npm run build
netlify deploy --prod
```

### 3. GitHub Pages

1. Update `vite.config.ts` base path:
```typescript
export default defineConfig({
  base: '/your-repo-name/',
  // ... rest of config
})
```

2. Add to `package.json`:
```json
{
  "scripts": {
    "deploy": "npm run build && npx gh-pages -d dist"
  }
}
```

3. Deploy:
```bash
npm run deploy
```

### 4. AWS S3 + CloudFront

```bash
# Build
npm run build

# Upload to S3
aws s3 sync dist/ s3://your-bucket-name --delete

# Invalidate CloudFront cache
aws cloudfront create-invalidation --distribution-id YOUR_DIST_ID --paths "/*"
```

### 5. Docker

```dockerfile
# Dockerfile
FROM node:18-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

FROM nginx:alpine
COPY --from=builder /app/dist /usr/share/nginx/html
COPY nginx.conf /etc/nginx/conf.d/default.conf
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

```bash
# Build and run
docker build -t milano-sa-fleet .
docker run -p 80:80 milano-sa-fleet
```

## 📝 Pre-Deployment Checklist

- [ ] Update meta tags in `index.html`
- [ ] Configure environment variables (if any)
- [ ] Test build locally: `npm run build && npm run preview`
- [ ] Optimize images and assets
- [ ] Enable gzip/brotli compression
- [ ] Set up SSL certificate
- [ ] Configure custom domain
- [ ] Add analytics (Google Analytics, etc.)
- [ ] Test on multiple devices and browsers
- [ ] Set up monitoring (Sentry, LogRocket, etc.)

## 🔧 Environment Variables

Create `.env.production`:

```bash
# API Endpoints (if needed)
VITE_API_URL=https://api.milanosa.com

# Analytics
VITE_GA_ID=G-XXXXXXXXXX

# Contact Form
VITE_CONTACT_API=https://formspree.io/f/YOUR_FORM_ID
```

## 🌐 Domain Configuration

### DNS Settings

```
Type    Name    Value                   TTL
A       @       76.76.21.21            3600
CNAME   www     your-app.vercel.app    3600
```

## ⚡ Performance Optimization

The build is already optimized with:
- Code splitting
- Tree shaking
- Minification
- Compression
- Lazy loading

### Additional Optimizations:

1. **Enable CDN** for static assets
2. **Configure caching headers**
3. **Enable HTTP/2**
4. **Use WebP images** where possible
5. **Implement service workers** for offline support

## 📊 Build Statistics

Current production build:
- HTML: ~1.2 KB (gzipped: 0.6 KB)
- CSS: ~31 KB (gzipped: 5.4 KB)
- JS: ~305 KB (gzipped: 94.4 KB)
- Total: ~337 KB (gzipped: ~100 KB)

## 🔐 Security Headers

Add to your hosting platform:

```
X-Frame-Options: DENY
X-Content-Type-Options: nosniff
X-XSS-Protection: 1; mode=block
Referrer-Policy: strict-origin-when-cross-origin
Permissions-Policy: geolocation=(), microphone=(), camera=()
```

## 🧪 Testing Production Build

```bash
# Build
npm run build

# Preview locally
npm run preview

# Open http://localhost:4173
```

## 📱 Progressive Web App (Optional)

To convert to PWA, add:

1. Install PWA plugin:
```bash
npm install vite-plugin-pwa -D
```

2. Update `vite.config.ts`:
```typescript
import { VitePWA } from 'vite-plugin-pwa'

export default defineConfig({
  plugins: [
    react(),
    VitePWA({
      registerType: 'autoUpdate',
      manifest: {
        name: 'Milano SA Fleet',
        short_name: 'Milano SA',
        description: 'Solar-Powered Fleet Solutions',
        theme_color: '#0ea5e9',
        icons: [
          {
            src: 'favicon.svg',
            sizes: '192x192',
            type: 'image/svg+xml'
          }
        ]
      }
    })
  ]
})
```

## 🎯 Success!

Your Milano SA Fleet website is now deployed and ready for production! 🎉
