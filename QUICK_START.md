# 🚀 QUICK START GUIDE
## Task Management & Tracking System

This guide will help you get the project up and running in 10 minutes!

## 📋 Prerequisites Checklist

Before starting, make sure you have:
- [ ] Node.js 18+ installed (`node --version`)
- [ ] npm installed (`npm --version`)
- [ ] PostgreSQL 14+ installed and running
- [ ] Git installed (optional, for version control)

## ⚡ Quick Setup (3 Steps)

### Step 1: Backend Setup (5 minutes)

```bash
# Navigate to backend
cd backend

# Install dependencies
npm install

# Create environment file
cp .env.example .env
```

**Edit the `.env` file** with your database credentials:
```env
DATABASE_URL="postgresql://YOUR_USERNAME:YOUR_PASSWORD@localhost:5432/task_management_db?schema=public"
JWT_SECRET=your-super-secret-jwt-key-change-in-production
```

```bash
# Create database and run migrations
npx prisma migrate dev --name init

# Seed the database with sample data
npm run seed

# Start the backend server
npm run dev
```

✅ Backend should now be running on `http://localhost:5000`

### Step 2: Frontend Setup (3 minutes)

Open a **new terminal window**:

```bash
# Navigate to frontend
cd frontend

# Install dependencies
npm install

# Create environment file
cp .env.example .env.local
```

**Edit the `.env.local` file**:
```env
NEXT_PUBLIC_API_URL=http://localhost:5000
```

```bash
# Start the frontend development server
npm run dev
```

✅ Frontend should now be running on `http://localhost:3000`

### Step 3: Admin Panel Setup (2 minutes)

Open **another new terminal window**:

```bash
# Navigate to admin panel
cd admin-panel

# Install dependencies
npm install

# Create environment file
cp .env.example .env.local
```

**Edit the `.env.local` file**:
```env
NEXT_PUBLIC_API_URL=http://localhost:5000
```

```bash
# Start the admin panel development server
npm run dev
```

✅ Admin panel should now be running on `http://localhost:3001`

## 🎯 Test Your Setup

### 1. Test the Backend API
Open your browser or Postman:
```
GET http://localhost:5000/health
```
You should see: `{ "status": "OK" }`

### 2. Login to Frontend
```
URL: http://localhost:3000
Email: demo@example.com
Password: Demo@123456
```

### 3. Access Admin Panel
```
URL: http://localhost:3001
Email: admin@taskmanagement.com
Password: Admin@123456
```

## 📱 What's Included?

### Backend (/backend)
- ✅ Complete REST API
- ✅ JWT Authentication
- ✅ PostgreSQL Database
- ✅ Prisma ORM
- ✅ Sample data seeded
- ✅ Admin endpoints
- ✅ Rate limiting
- ✅ Security middleware

### Frontend (/frontend)
- ✅ Next.js 14 setup
- ✅ Tailwind CSS configured
- ✅ Day-wise task display
- ✅ Authentication ready
- ✅ State management (Zustand)
- ✅ Responsive design

### Admin Panel (/admin-panel)
- ✅ Next.js 14 setup
- ✅ User management interface
- ✅ Analytics dashboard
- ✅ Task overview
- ✅ Activity logs

## 🎨 Default Accounts

After running `npm run seed` in the backend:

**Admin Account:**
- Email: `admin@taskmanagement.com`
- Password: `Admin@123456`
- Role: Super Admin

**Demo User Account:**
- Email: `demo@example.com`
- Password: `Demo@123456`
- Role: Regular User

⚠️ **Important:** Change these passwords in production!

## 🗂️ Project Structure

```
task-management-system/
├── backend/                 # Node.js/Express API
│   ├── src/
│   │   ├── config/         # Database config
│   │   ├── controllers/    # Route controllers
│   │   ├── middlewares/    # Auth, error handling
│   │   ├── routes/         # API routes
│   │   └── server.js       # Main server file
│   ├── prisma/
│   │   ├── schema.prisma   # Database schema
│   │   └── seed.js         # Sample data
│   └── package.json
│
├── frontend/               # React/Next.js app
│   ├── src/
│   │   ├── components/    # Reusable components
│   │   ├── pages/         # Next.js pages
│   │   ├── services/      # API calls
│   │   ├── contexts/      # React contexts
│   │   └── hooks/         # Custom hooks
│   └── package.json
│
└── admin-panel/           # Admin dashboard
    ├── src/
    │   ├── components/
    │   ├── pages/
    │   └── services/
    └── package.json
```

## 🔧 Common Commands

### Backend
```bash
npm run dev      # Start development server
npm start        # Start production server
npm run migrate  # Run database migrations
npm run seed     # Seed database
npm run studio   # Open Prisma Studio (DB GUI)
```

### Frontend & Admin Panel
```bash
npm run dev      # Start development server
npm run build    # Build for production
npm start        # Start production server
```

## 📚 API Documentation

### Authentication Endpoints
```
POST /api/auth/register     # Register new user
POST /api/auth/login        # Login
GET  /api/auth/me          # Get current user
POST /api/auth/logout      # Logout
```

### Task Endpoints (Protected)
```
GET    /api/tasks           # Get all tasks
GET    /api/tasks/by-date   # Get tasks grouped by date
GET    /api/tasks/:id       # Get single task
POST   /api/tasks           # Create task
PUT    /api/tasks/:id       # Update task
DELETE /api/tasks/:id       # Delete task
```

### Admin Endpoints (Admin Only)
```
GET    /api/admin/stats         # Dashboard statistics
GET    /api/admin/users         # Get all users
GET    /api/admin/users/:id     # Get user details
PUT    /api/admin/users/:id/status  # Activate/deactivate user
DELETE /api/admin/users/:id     # Delete user (Super Admin)
GET    /api/admin/tasks         # Get all tasks
GET    /api/admin/logs          # Get activity logs
```

## 🐛 Troubleshooting

### Database Connection Error
```bash
# Check if PostgreSQL is running
# Windows: Services → PostgreSQL
# Mac: brew services list
# Linux: sudo systemctl status postgresql

# Verify DATABASE_URL in .env file
# Make sure database exists: CREATE DATABASE task_management_db;
```

### Port Already in Use
```bash
# Backend (Port 5000)
lsof -ti:5000 | xargs kill -9

# Frontend (Port 3000)
lsof -ti:3000 | xargs kill -9

# Admin (Port 3001)
lsof -ti:3001 | xargs kill -9
```

### Module Not Found
```bash
# Delete node_modules and reinstall
rm -rf node_modules package-lock.json
npm install
```

## 🚀 Next Steps

1. **Customize the UI**: Edit components in `frontend/src/components`
2. **Add Features**: Extend controllers in `backend/src/controllers`
3. **Configure Email**: Set up email service in backend `.env`
4. **Deploy**: Follow deployment guide in main README.md

## 📖 Additional Resources

- [Full Documentation](./README.md)
- [Backend API Guide](./backend/README.md)
- [Prisma Documentation](https://www.prisma.io/docs)
- [Next.js Documentation](https://nextjs.org/docs)

## 💡 Tips

- Use `npm run studio` in backend to visually manage your database
- Check `backend/src/routes/` to see all available endpoints
- Sample tasks are created for demo@example.com after seeding
- All passwords are hashed with bcrypt (10 salt rounds)

## ✅ Verification Checklist

After setup, verify:
- [ ] Backend running on port 5000
- [ ] Frontend running on port 3000
- [ ] Admin panel running on port 3001
- [ ] Can access http://localhost:5000/health
- [ ] Can login to frontend with demo account
- [ ] Can access admin panel with admin account
- [ ] Sample tasks visible in frontend

---

**Need Help?** Check the troubleshooting section or open an issue!

Happy coding! 🎉
