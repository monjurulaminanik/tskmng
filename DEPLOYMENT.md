# 🚀 Deployment Guide
## Task Management & Tracking System

This guide covers deploying your application to production.

## 🌐 Deployment Options

### Option 1: Vercel (Recommended for Frontend/Admin)
### Option 2: Railway/Render (Backend + Database)
### Option 3: Docker (All Services)
### Option 4: Traditional VPS (Full Control)

---

## 📦 Option 1: Vercel + Railway (Easiest)

### Backend on Railway

1. **Create Railway Account**: https://railway.app
2. **Create New Project** → "Deploy from GitHub"
3. **Add PostgreSQL Database**:
   - Click "New" → "Database" → "PostgreSQL"
   - Railway automatically provides `DATABASE_URL`

4. **Deploy Backend**:
   - Click "New" → "GitHub Repo" → Select your backend repo
   - Add environment variables:
     ```
     NODE_ENV=production
     JWT_SECRET=your-production-secret-key
     FRONTEND_URL=https://your-frontend.vercel.app
     ADMIN_URL=https://your-admin.vercel.app
     ```
   - Railway will auto-detect and deploy

5. **Run Migrations**:
   ```bash
   # In Railway dashboard, go to your backend service
   # Settings → Deploy → Custom Start Command:
   npx prisma migrate deploy && npm start
   ```

### Frontend on Vercel

1. **Create Vercel Account**: https://vercel.com
2. **Import Project**:
   - Click "Add New" → "Project"
   - Import from GitHub
   - Select your frontend directory
   
3. **Configure Build**:
   - Framework: Next.js
   - Root Directory: `frontend`
   - Build Command: `npm run build`
   - Output Directory: `.next`

4. **Environment Variables**:
   ```
   NEXT_PUBLIC_API_URL=https://your-backend.railway.app
   ```

5. **Deploy**: Click "Deploy"

### Admin Panel on Vercel

Same as frontend, but:
- Root Directory: `admin-panel`
- Different domain/subdomain

---

## 🐳 Option 3: Docker Deployment

### Create Docker Files

**Backend Dockerfile** (`backend/Dockerfile`):
```dockerfile
FROM node:18-alpine

WORKDIR /app

COPY package*.json ./
RUN npm ci --only=production

COPY prisma ./prisma
RUN npx prisma generate

COPY . .

EXPOSE 5000

CMD ["npm", "start"]
```

**Frontend Dockerfile** (`frontend/Dockerfile`):
```dockerfile
FROM node:18-alpine AS builder

WORKDIR /app
COPY package*.json ./
RUN npm ci

COPY . .
RUN npm run build

FROM node:18-alpine
WORKDIR /app
COPY --from=builder /app/.next ./.next
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app/package.json ./package.json

EXPOSE 3000
CMD ["npm", "start"]
```

**Docker Compose** (`docker-compose.yml`):
```yaml
version: '3.8'

services:
  postgres:
    image: postgres:14-alpine
    environment:
      POSTGRES_USER: taskuser
      POSTGRES_PASSWORD: taskpass
      POSTGRES_DB: task_management_db
    volumes:
      - postgres_data:/var/lib/postgresql/data
    ports:
      - "5432:5432"

  backend:
    build: ./backend
    ports:
      - "5000:5000"
    environment:
      DATABASE_URL: postgresql://taskuser:taskpass@postgres:5432/task_management_db
      JWT_SECRET: ${JWT_SECRET}
      NODE_ENV: production
    depends_on:
      - postgres
    command: sh -c "npx prisma migrate deploy && npm start"

  frontend:
    build: ./frontend
    ports:
      - "3000:3000"
    environment:
      NEXT_PUBLIC_API_URL: http://localhost:5000
    depends_on:
      - backend

  admin:
    build: ./admin-panel
    ports:
      - "3001:3000"
    environment:
      NEXT_PUBLIC_API_URL: http://localhost:5000
    depends_on:
      - backend

volumes:
  postgres_data:
```

**Deploy with Docker**:
```bash
# Create .env file with secrets
echo "JWT_SECRET=your-secret-key" > .env

# Build and run
docker-compose up -d

# View logs
docker-compose logs -f

# Stop services
docker-compose down
```

---

## 🖥️ Option 4: VPS Deployment (Ubuntu/Debian)

### 1. Server Setup

```bash
# Update system
sudo apt update && sudo apt upgrade -y

# Install Node.js 18
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt install -y nodejs

# Install PostgreSQL
sudo apt install -y postgresql postgresql-contrib

# Install Nginx
sudo apt install -y nginx

# Install PM2 (Process Manager)
sudo npm install -g pm2
```

### 2. Database Setup

```bash
# Create database and user
sudo -u postgres psql
CREATE DATABASE task_management_db;
CREATE USER taskuser WITH ENCRYPTED PASSWORD 'your-password';
GRANT ALL PRIVILEGES ON DATABASE task_management_db TO taskuser;
\q
```

### 3. Deploy Backend

```bash
# Clone repository
git clone your-repo-url
cd task-management-system/backend

# Install dependencies
npm ci --only=production

# Set environment variables
cat > .env << EOF
NODE_ENV=production
PORT=5000
DATABASE_URL=postgresql://taskuser:your-password@localhost:5432/task_management_db
JWT_SECRET=your-production-secret
FRONTEND_URL=https://yourdomain.com
ADMIN_URL=https://admin.yourdomain.com
EOF

# Run migrations
npx prisma migrate deploy

# Seed database (optional)
npm run seed

# Start with PM2
pm2 start src/server.js --name task-backend
pm2 save
pm2 startup
```

### 4. Deploy Frontend

```bash
cd ../frontend
npm ci
npm run build

# Start with PM2
pm2 start npm --name task-frontend -- start
```

### 5. Deploy Admin Panel

```bash
cd ../admin-panel
npm ci
npm run build
pm2 start npm --name task-admin -- start
```

### 6. Nginx Configuration

**Backend** (`/etc/nginx/sites-available/api.yourdomain.com`):
```nginx
server {
    listen 80;
    server_name api.yourdomain.com;

    location / {
        proxy_pass http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

**Frontend** (`/etc/nginx/sites-available/yourdomain.com`):
```nginx
server {
    listen 80;
    server_name yourdomain.com;

    location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

**Admin** (`/etc/nginx/sites-available/admin.yourdomain.com`):
```nginx
server {
    listen 80;
    server_name admin.yourdomain.com;

    location / {
        proxy_pass http://localhost:3001;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

**Enable sites**:
```bash
sudo ln -s /etc/nginx/sites-available/api.yourdomain.com /etc/nginx/sites-enabled/
sudo ln -s /etc/nginx/sites-available/yourdomain.com /etc/nginx/sites-enabled/
sudo ln -s /etc/nginx/sites-available/admin.yourdomain.com /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl restart nginx
```

### 7. SSL with Let's Encrypt

```bash
sudo apt install -y certbot python3-certbot-nginx
sudo certbot --nginx -d yourdomain.com -d www.yourdomain.com
sudo certbot --nginx -d api.yourdomain.com
sudo certbot --nginx -d admin.yourdomain.com
```

---

## 🔐 Production Security Checklist

- [ ] Change all default passwords
- [ ] Use strong JWT secrets (min 32 characters)
- [ ] Enable HTTPS (SSL certificates)
- [ ] Set up firewall (UFW on Ubuntu)
- [ ] Configure CORS properly
- [ ] Enable rate limiting
- [ ] Regular database backups
- [ ] Monitor error logs
- [ ] Keep dependencies updated
- [ ] Use environment variables (never commit secrets)

---

## 📊 Monitoring & Maintenance

### PM2 Monitoring
```bash
pm2 status          # View all processes
pm2 logs            # View logs
pm2 restart all     # Restart all services
pm2 monit          # Real-time monitoring
```

### Database Backups
```bash
# Automated backup script
cat > /usr/local/bin/backup-db.sh << 'EOF'
#!/bin/bash
BACKUP_DIR="/var/backups/postgres"
DATE=$(date +%Y%m%d_%H%M%S)
mkdir -p $BACKUP_DIR
pg_dump -U taskuser task_management_db > $BACKUP_DIR/backup_$DATE.sql
find $BACKUP_DIR -type f -mtime +7 -delete
EOF

chmod +x /usr/local/bin/backup-db.sh

# Add to crontab (daily at 2 AM)
(crontab -l; echo "0 2 * * * /usr/local/bin/backup-db.sh") | crontab -
```

---

## 🔄 CI/CD with GitHub Actions

Create `.github/workflows/deploy.yml`:
```yaml
name: Deploy to Production

on:
  push:
    branches: [ main ]

jobs:
  deploy:
    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v2
    
    - name: Deploy Backend
      uses: appleboy/ssh-action@master
      with:
        host: ${{ secrets.SERVER_HOST }}
        username: ${{ secrets.SERVER_USER }}
        key: ${{ secrets.SSH_KEY }}
        script: |
          cd /path/to/backend
          git pull
          npm ci
          npx prisma migrate deploy
          pm2 restart task-backend
```

---

## 📈 Performance Optimization

### Backend
- Enable compression middleware
- Use Redis for caching
- Optimize database queries
- Enable connection pooling

### Frontend
- Enable Next.js image optimization
- Use CDN for static assets
- Enable caching headers
- Minimize bundle size

---

## 🆘 Troubleshooting Production Issues

### Service won't start
```bash
pm2 logs task-backend --err  # Check error logs
pm2 describe task-backend     # View detailed info
```

### Database connection issues
```bash
# Test connection
psql -U taskuser -d task_management_db -h localhost
```

### Nginx errors
```bash
sudo nginx -t                 # Test configuration
sudo tail -f /var/log/nginx/error.log
```

---

**Deployment Complete!** 🎉

Your Task Management System is now live in production.
