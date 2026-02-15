# 🪟 WINDOWS SETUP GUIDE
## Task Management System - Step by Step

## ✅ Prerequisites

1. **Node.js 18+** - Download from https://nodejs.org/
2. **PostgreSQL 14+** - Download from https://www.postgresql.org/download/windows/
3. **VS Code** (Recommended) - https://code.visualstudio.com/

---

## 🚀 Step-by-Step Setup

### Step 1: Install PostgreSQL

1. Run the PostgreSQL installer
2. Remember the password you set for the `postgres` user
3. Default port is `5432` - keep it
4. After installation, open **pgAdmin 4**

### Step 2: Create Database

In pgAdmin:
1. Right-click on "Databases" → "Create" → "Database"
2. Name: `task_management_db`
3. Owner: `postgres`
4. Click "Save"

### Step 3: Setup Backend

Open PowerShell or Command Prompt in your project folder:

```powershell
# Navigate to backend folder
cd backend

# Install dependencies
npm install

# Create .env file
copy .env.example .env

# Open .env in notepad
notepad .env
```

**Edit the .env file:**
```env
DATABASE_URL="postgresql://postgres:YOUR_POSTGRES_PASSWORD@localhost:5432/task_management_db?schema=public"
JWT_SECRET=your-super-secret-jwt-key-change-this-in-production
PORT=5000
NODE_ENV=development
FRONTEND_URL=http://localhost:3000
ADMIN_URL=http://localhost:3001
```

Replace `YOUR_POSTGRES_PASSWORD` with your actual PostgreSQL password!

```powershell
# Run database migrations
npx prisma migrate dev --name init

# Seed the database with sample data
npm run seed

# Start the backend server
npm run dev
```

✅ Backend should now run on http://localhost:5000

**Keep this terminal open!**

### Step 4: Setup Frontend

Open a **NEW** PowerShell/Command Prompt window:

```powershell
# Navigate to frontend folder
cd frontend

# Install dependencies
npm install

# Create .env.local file
copy .env.example .env.local

# Start the frontend
npm run dev
```

✅ Frontend should now run on http://localhost:3000

**Keep this terminal open too!**

### Step 5: Setup Admin Panel

Open **ANOTHER NEW** PowerShell/Command Prompt window:

```powershell
# Navigate to admin-panel folder
cd admin-panel

# Install dependencies
npm install

# Create .env.local file
copy .env.example .env.local

# Start the admin panel
npm run dev
```

✅ Admin panel should now run on http://localhost:3001

---

## 🎯 Test Your Setup

### 1. Test Backend API
Open browser: http://localhost:5000/health
You should see: `{"status":"OK",...}`

### 2. Login to Frontend
- URL: http://localhost:3000
- Email: `demo@example.com`
- Password: `Demo@123456`

### 3. Access Admin Panel
- URL: http://localhost:3001
- Email: `admin@taskmanagement.com`
- Password: `Admin@123456`

---

## 🐛 Common Windows Issues & Solutions

### Issue 1: "npm is not recognized"
**Solution:** 
- Restart your computer after installing Node.js
- Or add Node.js to PATH manually

### Issue 2: "Cannot connect to database"
**Solutions:**
1. Make sure PostgreSQL service is running:
   - Press `Win + R`
   - Type `services.msc`
   - Find "postgresql-x64-14" and make sure it's "Running"

2. Check your DATABASE_URL in `.env`:
   - Use the correct password
   - Port should be 5432
   - Database name: `task_management_db`

### Issue 3: "Port 5000 already in use"
**Solution:**
```powershell
# Find process using port 5000
netstat -ano | findstr :5000

# Kill the process (replace PID with actual number)
taskkill /PID <PID> /F
```

### Issue 4: "Module not found"
**Solution:**
```powershell
# Delete node_modules and reinstall
rmdir /s node_modules
del package-lock.json
npm install
```

### Issue 5: Prisma migration errors
**Solution:**
```powershell
# Reset Prisma
npx prisma migrate reset
npx prisma migrate dev
npm run seed
```

---

## 📁 VS Code Setup (Recommended)

1. Open VS Code
2. File → Open Folder → Select `task-management-system`
3. Install recommended extensions:
   - Prisma
   - ES7+ React/Redux/React-Native snippets
   - Tailwind CSS IntelliSense

4. Open 3 terminals in VS Code:
   - Terminal → New Terminal (3 times)
   - Terminal 1: `cd backend && npm run dev`
   - Terminal 2: `cd frontend && npm run dev`
   - Terminal 3: `cd admin-panel && npm run dev`

---

## 🔧 Quick Commands Reference

### Backend
```powershell
cd backend
npm run dev          # Start development server
npm run seed         # Seed database
npm run studio       # Open Prisma Studio (database GUI)
npx prisma migrate dev  # Run migrations
```

### Frontend
```powershell
cd frontend
npm run dev          # Start development server
npm run build        # Build for production
```

### Admin Panel
```powershell
cd admin-panel
npm run dev          # Start development server
npm run build        # Build for production
```

---

## 🎓 Video Tutorial (If you get stuck)

If you're having trouble, you can:
1. Check the error message carefully
2. Google the specific error
3. Check the main README.md file
4. Look at QUICK_START.md

---

## ✅ Verification Checklist

After setup, make sure:
- [ ] PostgreSQL service is running
- [ ] Database `task_management_db` exists in pgAdmin
- [ ] Backend running on http://localhost:5000
- [ ] Frontend running on http://localhost:3000
- [ ] Admin panel running on http://localhost:3001
- [ ] Can access /health endpoint
- [ ] Can login with demo credentials

---

## 🎉 You're All Set!

Now you have:
- ✅ Backend API running
- ✅ Frontend application running
- ✅ Admin dashboard running
- ✅ Database with sample data

**Next Steps:**
1. Explore the frontend at http://localhost:3000
2. Check admin panel at http://localhost:3001
3. Try creating tasks using the demo account
4. View database in Prisma Studio: `npm run studio` (in backend folder)

**Need Help?** Check the troubleshooting section above or the main documentation!

Happy coding! 🚀
