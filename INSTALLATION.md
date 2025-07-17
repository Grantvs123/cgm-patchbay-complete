# Installation Guide - CGM Matrix PatchBay

## 🖥️ Desktop Application Installation

### Windows Installation

#### Option 1: Installer (Recommended)
1. Download `CGM-Matrix-PatchBay-Setup-1.0.0.exe` from releases
2. Run the installer as Administrator
3. Follow the installation wizard
4. Launch from Start Menu or Desktop shortcut

#### Option 2: Portable Version
1. Download `CGM-Matrix-PatchBay-1.0.0-win.zip` from releases
2. Extract to desired folder
3. Run `CGM Matrix PatchBay.exe`

### Linux Installation

#### Option 1: AppImage (Universal)
1. Download `CGM-Matrix-PatchBay-1.0.0.AppImage` from releases
2. Make executable: `chmod +x CGM-Matrix-PatchBay-1.0.0.AppImage`
3. Run: `./CGM-Matrix-PatchBay-1.0.0.AppImage`

#### Option 2: Snap Package
1. Download `cgm-matrix-patchbay_1.0.0_amd64.snap` from releases
2. Install: `sudo snap install --dangerous cgm-matrix-patchbay_1.0.0_amd64.snap`
3. Run: `cgm-matrix-patchbay`

### macOS Installation
1. Download `CGM-Matrix-PatchBay-1.0.0.dmg` from releases
2. Open the DMG file
3. Drag CGM Matrix PatchBay to Applications folder
4. Launch from Applications or Launchpad

## 🌐 Web Application Installation

### Local Development
```bash
# Clone repository
git clone https://github.com/Grantvs123/cgm-patchbay-complete.git
cd cgm-patchbay-complete/app

# Install dependencies
npm install --legacy-peer-deps

# Setup database
npx prisma generate
npx prisma db push

# Run development server
npm run dev

# Open http://localhost:3000
```

### Production Deployment

#### Netlify
1. Connect GitHub repository to Netlify
2. Build command: `cd app && npm install --legacy-peer-deps && npm run build`
3. Publish directory: `app/out`
4. Deploy

#### Vercel
```bash
cd app
npm install -g vercel
vercel --prod
```

#### Docker
```bash
# Build image
docker build -t cgm-patchbay .

# Run container
docker run -p 3000:3000 cgm-patchbay
```

## 🔧 System Requirements

### Desktop Application
- **Windows**: Windows 10 or later (64-bit)
- **Linux**: Ubuntu 18.04+ or equivalent (64-bit)
- **macOS**: macOS 10.14+ (64-bit)
- **RAM**: 4GB minimum, 8GB recommended
- **Storage**: 500MB free space
- **Network**: Internet connection for initial setup

### Web Application
- **Node.js**: Version 18 or later
- **Browser**: Chrome 90+, Firefox 88+, Safari 14+, Edge 90+
- **RAM**: 2GB minimum
- **Storage**: 1GB free space for development

## 🚀 First-Time Setup

### Desktop Application
1. Launch the application
2. Complete the initial setup wizard:
   - Organization settings
   - User account creation
   - API key configuration
   - Workflow preferences
3. Access the main dashboard

### Web Application
1. Navigate to the application URL
2. Create admin account or use demo credentials:
   - **Admin**: admin@cgm-patchbay.com / admin123
   - **Demo**: demo@cgm-patchbay.com / demo123
3. Complete organization setup
4. Configure integrations

## 🔒 Security Configuration

### Environment Variables
```env
# Database
DATABASE_URL="file:./dev.db"

# Authentication
NEXTAUTH_SECRET="your-secret-key"
NEXTAUTH_URL="http://localhost:3000"

# API Keys (optional)
OPENAI_API_KEY="your-openai-key"
ANTHROPIC_API_KEY="your-anthropic-key"
```

### SSL/TLS Setup
For production deployments:
1. Obtain SSL certificate
2. Configure reverse proxy (nginx/Apache)
3. Update NEXTAUTH_URL to https://
4. Enable secure cookies

## 🛠️ Troubleshooting

### Desktop Application Issues

**App won't start:**
- Check system requirements
- Run as Administrator (Windows)
- Check antivirus software
- Verify file permissions

**Database errors:**
- Delete `app.db` file and restart
- Check write permissions in app directory
- Ensure SQLite is properly installed

### Web Application Issues

**Build errors:**
- Use `--legacy-peer-deps` flag
- Clear npm cache: `npm cache clean --force`
- Delete `node_modules` and reinstall

**Authentication issues:**
- Check NEXTAUTH_SECRET is set
- Verify database connection
- Clear browser cookies

**API errors:**
- Check network connectivity
- Verify API endpoints are accessible
- Review server logs

## 📋 Verification Checklist

After installation, verify:
- [ ] Application launches successfully
- [ ] User authentication works
- [ ] Database operations function
- [ ] API integrations connect
- [ ] Workflow engine responds
- [ ] All UI components render
- [ ] Settings can be saved
- [ ] Documentation is accessible

## 🔄 Updates and Maintenance

### Desktop Application
- Check for updates in Help menu
- Download new releases from GitHub
- Backup user data before updating

### Web Application
- Pull latest changes from repository
- Run database migrations if needed
- Update dependencies regularly
- Monitor security advisories

## 📞 Support

For installation support:
- Check documentation in `docs/` folder
- Review troubleshooting section
- Submit issues on GitHub repository
- Contact CGM Matrix Systems for enterprise support

---

**Installation Status**: ✅ Ready for deployment across all platforms
