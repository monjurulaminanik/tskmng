# Task Management & Tracking System

A comprehensive task management system with day-wise serial task display, user authentication, and admin panel.

## Project Structure

```
task-management-system/
├── frontend/           # React frontend application
├── backend/            # Node.js/Express backend API
├── admin-panel/        # Admin dashboard
└── README.md
```

## Features

### Frontend
- Day-wise serial task display
- Responsive design (mobile, tablet, desktop)
- Task filtering and search
- Calendar view
- Real-time updates

### Backend
- JWT authentication
- RESTful API
- PostgreSQL database
- Rate limiting
- Security features (HTTPS, CSRF, XSS protection)

### Admin Panel
- User management
- Task/content management
- Analytics and reports
- System settings
- Activity logs

## Tech Stack

### Frontend
- React.js with Next.js
- Tailwind CSS
- Zustand (state management)
- Axios (HTTP client)

### Backend
- Node.js
- Express.js
- PostgreSQL
- Prisma ORM
- JWT authentication
- bcrypt for password hashing

### Admin Panel
- React.js
- Recharts (analytics)
- Material-UI components

## Getting Started

### Prerequisites
- Node.js 18+ and npm
- PostgreSQL 14+
- Git

### Installation

1. Clone the repository
```bash
git clone <repository-url>
cd task-management-system
```

2. Install dependencies for all components
```bash
# Install frontend dependencies
cd frontend
npm install

# Install backend dependencies
cd ../backend
npm install

# Install admin panel dependencies
cd ../admin-panel
npm install
```

3. Configure environment variables
```bash
# Backend .env
cp backend/.env.example backend/.env
# Edit backend/.env with your database credentials

# Frontend .env
cp frontend/.env.example frontend/.env.local
# Edit with your API URL
```

4. Set up the database
```bash
cd backend
npx prisma migrate dev
npx prisma db seed
```

5. Start development servers
```bash
# Terminal 1 - Backend
cd backend
npm run dev

# Terminal 2 - Frontend
cd frontend
npm run dev

# Terminal 3 - Admin Panel
cd admin-panel
npm run dev
```

## API Endpoints

### Authentication
- `POST /api/auth/register` - User registration
- `POST /api/auth/login` - User login
- `POST /api/auth/logout` - User logout
- `POST /api/auth/forgot-password` - Password reset request
- `POST /api/auth/reset-password` - Reset password

### Tasks
- `GET /api/tasks` - Get all user tasks
- `GET /api/tasks/:id` - Get specific task
- `POST /api/tasks` - Create new task
- `PUT /api/tasks/:id` - Update task
- `DELETE /api/tasks/:id` - Delete task

### Admin
- `GET /api/admin/users` - Get all users
- `GET /api/admin/stats` - Get system statistics
- `PUT /api/admin/users/:id/status` - Update user status

## Development Timeline

- **Planning & Design**: 2 weeks ✓
- **Backend Development**: 4 weeks (In Progress)
- **Frontend Development**: 4 weeks
- **Admin Panel**: 3 weeks
- **Testing & QA**: 2 weeks
- **Deployment**: 1 week

**Total**: 16 weeks

## Security Features

- HTTPS encryption
- JWT token authentication
- Password hashing with bcrypt
- SQL injection prevention (Prisma ORM)
- XSS protection
- CSRF protection
- Rate limiting
- Input validation

## Performance Goals

- Page load time: < 2 seconds
- API response time: < 500ms
- Support: 1000+ concurrent users
- Uptime: 99.9%

## Contributing

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## License

This project is licensed under the MIT License.

## Support

For support, email support@taskmanagement.com or open an issue in the repository.
