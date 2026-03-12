# 🚀 Deployment Guide - 3D UNO

Deploy your 3D UNO game to the world!

## 🌐 Deployment Options

### Option 1: Vercel (Recommended)

**Pros:** Free, fast, automatic updates, great for Node.js

```bash
# Install Vercel CLI
npm install -g vercel

# Deploy
vercel

# Follow the prompts
# Your game will be at: https://your-project.vercel.app
```

**vercel.json configuration:**

```json
{
  "buildCommand": "echo 'Build skipped'",
  "outputDirectory": "public"
}
```

### Option 2: Heroku

**Pros:** Simple deployment, free tier available

```bash
# Install Heroku CLI
# (Download from heroku.com)

# Login
heroku login

# Create app
heroku create your-uno-game

# Deploy
git push heroku main

# View logs
heroku logs --tail
```

**Procfile:**
```
web: node server.js
```

### Option 3: Railway.app

**Pros:** Modern, simple, generous free tier

```bash
# Install Railway CLI
npm i -g @railway/cli

# Login
railway login

# Init project
railway init

# Deploy
railway up
```

### Option 4: Render

**Pros:** Good free tier, simple setup

1. Push code to GitHub
2. Go to render.com
3. Connect GitHub repository
4. Select "Node"
5. Set start command: `npm start`
6. Deploy!

### Option 5: Self-Hosted (VPS)

**Pros:** Full control, better performance

#### Using DigitalOcean:

```bash
# 1. Create droplet (Ubuntu 20.04)
# 2. SSH into droplet
ssh root@your_ip

# 3. Install Node.js
curl -fsSL https://deb.nodesource.com/setup_16.x | sudo -E bash -
sudo apt-get install -y nodejs

# 4. Clone repository
git clone your-repo-url
cd your-repo

# 5. Install dependencies
npm install --production

# 6. Install PM2 (process manager)
sudo npm install -g pm2

# 7. Start with PM2
pm2 start server.js --name "uno-game"
pm2 startup
pm2 save

# 8. Install Nginx (reverse proxy)
sudo apt-get install nginx

# Configure Nginx...
```

**Nginx Configuration:**

```nginx
server {
    listen 80;
    server_name yourdomain.com;

    location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

#### Using AWS:

1. Create EC2 instance (t3.micro free tier)
2. Allow ports 80, 443 in security group
3. SSH and install Node.js (same as DigitalOcean)
4. Use AWS Elastic IP for static IP
5. Use Route 53 for DNS

#### Using Google Cloud:

1. Create VM instance
2. SSH into instance
3. Install Node.js
4. Install gcloud CLI on local machine
5. Deploy using Cloud Run (serverless)

---

## 🔒 SSL/HTTPS Setup

### Using Let's Encrypt (Free SSL)

```bash
# Install Certbot
sudo apt-get install certbot python3-certbot-nginx

# Generate certificate
sudo certbot certonly --standalone -d yourdomain.com

# Auto-renew
sudo systemctl start certbot.timer
```

### Update Nginx for HTTPS:

```nginx
server {
    listen 443 ssl http2;
    server_name yourdomain.com;

    ssl_certificate /etc/letsencrypt/live/yourdomain.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/yourdomain.com/privkey.pem;

    location / {
        proxy_pass http://localhost:3000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}

# Redirect HTTP to HTTPS
server {
    listen 80;
    server_name yourdomain.com;
    return 301 https://$server_name$request_uri;
}
```

---

## 🔧 Environment Variables

### Production Environment Variables

Create `.env` file:

```env
NODE_ENV=production
PORT=3000
HOST=0.0.0.0
```

### Update server.js to use .env:

```javascript
require('dotenv').config();
const PORT = process.env.PORT || 3000;
const HOST = process.env.HOST || 'localhost';

server.listen(PORT, HOST, () => {
    console.log(`Server running at http://${HOST}:${PORT}`);
});
```

Install dotenv:
```bash
npm install dotenv
```

---

## 📦 Production Build Optimization

### Compress files:

```bash
# Install compression
npm install compression
```

Add to server.js:

```javascript
const compression = require('compression');
app.use(compression());
```

### Minify frontend:

Already minified by browsers, but you can use:

```bash
npm install -g terser
terser public/index.html -o public/index.min.html
```

---

## 🔄 Continuous Deployment

### GitHub Actions

Create `.github/workflows/deploy.yml`:

```yaml
name: Deploy to Vercel

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: actions/setup-node@v2
        with:
          node-version: '16'
      - run: npm install
      - run: npm test
      - uses: BetaHuhn/deploy-to-vercel-action@v1
        with:
          VERCEL_TOKEN: ${{ secrets.VERCEL_TOKEN }}
          VERCEL_ORG_ID: ${{ secrets.VERCEL_ORG_ID }}
          VERCEL_PROJECT_ID: ${{ secrets.VERCEL_PROJECT_ID }}
```

### GitLab CI/CD

Create `.gitlab-ci.yml`:

```yaml
stages:
  - deploy

deploy:
  stage: deploy
  image: node:16
  script:
    - npm install
    - npm test
  deploy_script:
    - curl https://deploy-hook.example.com
```

---

## 📊 Monitoring & Logs

### PM2 Monitoring:

```bash
# Monitor CPU/Memory
pm2 monit

# View logs
pm2 logs uno-game

# Save logs
pm2 start server.js --log /var/log/uno-game.log
```

### Using DataDog or New Relic:

```javascript
// Add monitoring to server.js
require('newrelic');  // After require statements
```

---

## 🔍 Performance Optimization

### Node.js Clustering:

```javascript
const cluster = require('cluster');
const os = require('os');

if (cluster.isMaster) {
    const numCPUs = os.cpus().length;
    
    for (let i = 0; i < numCPUs; i++) {
        cluster.fork();
    }
} else {
    server.listen(PORT);
}
```

### Redis for Session Storage:

```bash
npm install redis connect-redis express-session
```

```javascript
const redis = require('redis');
const RedisStore = require('connect-redis').default;
const client = redis.createClient();

app.use(session({
    store: new RedisStore({ client }),
    secret: process.env.SESSION_SECRET
}));
```

---

## 🐛 Debugging in Production

### Enable Debug Logging:

```bash
# Set environment variable
DEBUG=*

# Or use
DEBUG=uno* npm start
```

### Using PM2 Plus:

```bash
pm2 install pm2-auto-pull
pm2 install pm2-server-monit
```

---

## 🚨 Troubleshooting

### Game Not Loading

```bash
# Check server is running
curl http://localhost:3000

# Check logs
pm2 logs uno-game

# Check port availability
netstat -tulpn | grep 3000
```

### WebSocket Connection Issues

```javascript
// In server.js, check CORS
const io = new Server(server, {
    cors: {
        origin: 'https://yourdomain.com',
        methods: ['GET', 'POST']
    }
});
```

### Model Not Loading

1. Check CDN URL is accessible
2. Verify CORS headers
3. Check browser console for errors
4. Ensure model file exists

### Slow Performance

```bash
# Monitor with PM2
pm2 monit

# Check Node memory
ps aux | grep node

# Increase memory
node --max-old-space-size=4096 server.js
```

---

## 🎯 Pre-Deployment Checklist

- [ ] Test on production-like environment
- [ ] Check all sound effects work
- [ ] Test 3D model loading
- [ ] Verify multiplayer functionality
- [ ] Test on mobile devices
- [ ] Check UI responsiveness
- [ ] Verify SSL certificate
- [ ] Set up monitoring
- [ ] Configure logging
- [ ] Update DNS records
- [ ] Test disaster recovery
- [ ] Document deployment steps
- [ ] Set up backups
- [ ] Configure auto-restart
- [ ] Test under load

---

## 📞 Support Resources

- **Vercel Docs:** vercel.com/docs
- **Heroku Docs:** devcenter.heroku.com
- **PM2 Docs:** pm2.keymetrics.io
- **Node.js Docs:** nodejs.org/docs
- **Socket.IO Docs:** socket.io/docs
- **Three.js Docs:** threejs.org/docs

---

**Happy deploying! 🚀**
