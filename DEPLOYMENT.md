# Deployment Guide - CGM Matrix PatchBay

## 🚀 Quick Deployment Options

### Option 1: Netlify (Recommended for Web)

1. **Fork/Clone this repository**
2. **Connect to Netlify**:
   - Go to [netlify.com](https://netlify.com)
   - Click "New site from Git"
   - Select this repository
3. **Build Settings**:
   - Build command: `cd app && npm install --legacy-peer-deps && npm run build`
   - Publish directory: `app/out`
   - Node version: 18 or higher
4. **Deploy**: Click "Deploy site"

**Result**: Your CGM PatchBay will be live at `https://your-site-name.netlify.app`

### Option 2: Vercel (Next.js Native)

```bash
# Install Vercel CLI
npm install -g vercel

# Deploy from app directory
cd app
vercel --prod
```

### Option 3: Abacus.AI Platform

1. **Push to GitHub**:
   ```bash
   git remote add origin https://github.com/Grantvs123/cgm-patchbay-complete.git
   git push -u origin main
   ```

2. **Connect to Abacus.AI**:
   - Use the GitHub repository URL
   - The platform will automatically detect Next.js
   - Deploy with default settings

## 🖥️ Desktop Application Distribution

### Windows Distribution

#### Installer Distribution
1. **Build the installer**:
   ```bash
   cd app
   npm run build:win
   ```
2. **Generated files**:
   - `dist/CGM-Matrix-PatchBay-Setup-1.0.0.exe` (Installer)
   - `dist/win-unpacked/` (Portable version)

#### Code Signing (Optional)
```bash
# Set environment variables
export CSC_LINK=path/to/certificate.p12
export CSC_KEY_PASSWORD=certificate_password

# Build with signing
npm run build:win
```

### Linux Distribution

#### AppImage Build
```bash
cd app
npm run build:linux
```

#### Snap Package
```bash
# Install snapcraft
sudo snap install snapcraft --classic

# Build snap package
cd app
snapcraft
```

### macOS Distribution

#### DMG Build
```bash
cd app
npm run build:mac
```

#### Code Signing and Notarization
```bash
# Set environment variables
export APPLE_ID=your-apple-id@example.com
export APPLE_ID_PASS=app-specific-password
export CSC_LINK=path/to/certificate.p12
export CSC_KEY_PASSWORD=certificate_password

# Build with signing and notarization
npm run build:mac
```

## 🔧 Build Configuration

### Electron Builder Configuration

The `electron-builder.json` file contains build settings:

```json
{
  "appId": "com.cgmmatrix.patchbay",
  "productName": "CGM Matrix PatchBay",
  "directories": {
    "output": "dist"
  },
  "files": [
    "out/**/*",
    "electron/**/*",
    "package.json"
  ],
  "win": {
    "target": [
      {
        "target": "nsis",
        "arch": ["x64"]
      },
      {
        "target": "portable",
        "arch": ["x64"]
      }
    ],
    "icon": "assets/icon.ico"
  },
  "linux": {
    "target": [
      {
        "target": "AppImage",
        "arch": ["x64"]
      },
      {
        "target": "snap",
        "arch": ["x64"]
      }
    ],
    "icon": "assets/icon.png"
  },
  "mac": {
    "target": [
      {
        "target": "dmg",
        "arch": ["x64", "arm64"]
      }
    ],
    "icon": "assets/icon.icns"
  }
}
```

## 🌐 Environment Variables

### Web Application
```env
# Database
DATABASE_URL="file:./dev.db"

# Authentication
NEXTAUTH_SECRET="your-secret-key-here"
NEXTAUTH_URL="https://your-domain.com"

# API Keys (optional)
OPENAI_API_KEY="your-openai-key"
ANTHROPIC_API_KEY="your-anthropic-key"

# Optional: CORS origins
ALLOWED_ORIGINS="https://yourdomain.com,https://www.yourdomain.com"
```

### Desktop Application
```env
# Node-RED Configuration
NODE_RED_PORT=1880
NODE_RED_ADMIN_AUTH=false

# Database
DATABASE_PATH="./data/app.db"

# Logging
LOG_LEVEL="info"
LOG_FILE="./logs/app.log"
```

## 🔒 Security Configuration

### SSL/TLS Setup (Web)
```nginx
server {
    listen 443 ssl;
    server_name your-domain.com;
    
    ssl_certificate /path/to/certificate.crt;
    ssl_certificate_key /path/to/private.key;
    
    location / {
        proxy_pass http://localhost:3000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```

### Firewall Configuration (Desktop)
```bash
# Allow Node-RED port
sudo ufw allow 1880/tcp

# Allow application port
sudo ufw allow 3000/tcp
```

## 📦 Docker Deployment

### Web Application Docker
```dockerfile
FROM node:18-alpine

WORKDIR /app
COPY app/package*.json ./
RUN npm install --legacy-peer-deps

COPY app/ .
RUN npm run build

EXPOSE 3000
CMD ["npm", "start"]
```

### Build and Run
```bash
# Build image
docker build -t cgm-patchbay .

# Run container
docker run -p 3000:3000 -e NEXTAUTH_SECRET=your-secret cgm-patchbay
```

### Docker Compose
```yaml
version: '3.8'
services:
  cgm-patchbay:
    build: .
    ports:
      - "3000:3000"
    environment:
      - NEXTAUTH_SECRET=your-secret-key
      - DATABASE_URL=file:./data/app.db
    volumes:
      - ./data:/app/data
```

## ☁️ Cloud Deployment

### AWS Deployment
```bash
# Install AWS CLI and configure
aws configure

# Deploy using AWS Amplify
amplify init
amplify add hosting
amplify publish
```

### Azure Deployment
```bash
# Install Azure CLI
az login

# Create web app
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name cgm-patchbay --runtime "NODE|18-lts"

# Deploy
az webapp deployment source config --name cgm-patchbay --resource-group myResourceGroup --repo-url https://github.com/Grantvs123/cgm-patchbay-complete --branch main
```

### Google Cloud Deployment
```bash
# Install gcloud CLI
gcloud auth login

# Deploy to App Engine
gcloud app deploy app.yaml
```

## 🔄 CI/CD Pipeline

### GitHub Actions
```yaml
name: Build and Deploy

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  build-web:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v3
    - name: Setup Node.js
      uses: actions/setup-node@v3
      with:
        node-version: '18'
    - name: Install dependencies
      run: cd app && npm install --legacy-peer-deps
    - name: Build application
      run: cd app && npm run build
    - name: Deploy to Netlify
      uses: netlify/actions/cli@master
      with:
        args: deploy --prod --dir=app/out
      env:
        NETLIFY_AUTH_TOKEN: ${{ secrets.NETLIFY_AUTH_TOKEN }}
        NETLIFY_SITE_ID: ${{ secrets.NETLIFY_SITE_ID }}

  build-desktop:
    runs-on: ${{ matrix.os }}
    strategy:
      matrix:
        os: [windows-latest, ubuntu-latest, macos-latest]
    steps:
    - uses: actions/checkout@v3
    - name: Setup Node.js
      uses: actions/setup-node@v3
      with:
        node-version: '18'
    - name: Install dependencies
      run: cd app && npm install --legacy-peer-deps
    - name: Build desktop app
      run: cd app && npm run build:electron
    - name: Upload artifacts
      uses: actions/upload-artifact@v3
      with:
        name: desktop-${{ matrix.os }}
        path: app/dist/
```

## 📊 Monitoring and Analytics

### Application Monitoring
```javascript
// Add to app/lib/monitoring.js
import { Analytics } from '@vercel/analytics/react';

export function Monitoring() {
  return <Analytics />;
}
```

### Error Tracking
```javascript
// Add to app/lib/error-tracking.js
import * as Sentry from "@sentry/nextjs";

Sentry.init({
  dsn: process.env.SENTRY_DSN,
});
```

## 🧪 Testing Deployment

### Pre-deployment Checklist
- [ ] All dependencies installed correctly
- [ ] Environment variables configured
- [ ] Database migrations completed
- [ ] SSL certificates valid
- [ ] API endpoints responding
- [ ] Authentication working
- [ ] Desktop builds successful
- [ ] Cross-platform compatibility verified

### Post-deployment Verification
```bash
# Test web application
curl -I https://your-domain.com
curl -X POST https://your-domain.com/api/health

# Test desktop application
./CGM-Matrix-PatchBay-1.0.0.AppImage --version
```

## 📞 Support and Troubleshooting

### Common Deployment Issues

#### Build Failures
- Use `--legacy-peer-deps` flag for npm install
- Ensure Node.js version 18 or higher
- Clear npm cache: `npm cache clean --force`

#### SSL Issues
- Verify certificate validity
- Check certificate chain
- Ensure proper nginx/Apache configuration

#### Database Issues
- Verify database permissions
- Check connection strings
- Ensure migrations are applied

### Getting Help
- **Documentation**: Complete guides in repository
- **GitHub Issues**: Bug reports and feature requests
- **Community**: Discussion forums and chat
- **Enterprise**: Professional deployment support

---

**Deployment Status**: ✅ **Production Ready** - Comprehensive deployment options for all platforms and environments.
