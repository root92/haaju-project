# Haaju Project Management Platform

A modern, full-stack project management platform built with Django (backend) and Next.js (frontend). Haaju provides teams with powerful tools for project tracking, team collaboration, and workflow automation.

## 🚀 Features

### Core Features
- **Project Management**: Create, organize, and track projects with intuitive interfaces
- **Task Management**: Assign tasks, set priorities, and track progress
- **Team Collaboration**: Real-time collaboration tools and communication features
- **User Management**: Role-based access control and user profiles
- **Subscription System**: Flexible pricing plans with Stripe integration
- **Analytics & Reporting**: Comprehensive insights into team performance
- **Mobile Responsive**: Optimized for all devices and screen sizes

### Technical Features
- **Modern Stack**: Django 4.x + Next.js 14 + TypeScript
- **Real-time Updates**: WebSocket support for live collaboration
- **GraphQL API**: Flexible data querying alongside REST endpoints
- **Dark/Light Mode**: Beautiful theming system with automatic switching
- **Performance Optimized**: Fast loading times and efficient data handling
- **Security First**: Enterprise-grade security with authentication and authorization

## 🏗️ Architecture

```
haaju_project/
├── haaju_backend/          # Django Backend
│   ├── config/            # Django settings and configuration
│   ├── haaju/             # Main Django app
│   ├── users/             # User management app
│   ├── subscription/      # Subscription handling
│   ├── graphql/           # GraphQL API
│   └── requirements/      # Python dependencies
└── haaju_frontend/        # Next.js Frontend
    ├── src/
    │   ├── app/           # Next.js App Router pages
    │   ├── components/    # Reusable UI components
    │   └── lib/           # Utilities and API config
    └── public/            # Static assets
```

## 🛠️ Tech Stack

### Backend (Django)
- **Framework**: Django 4.x
- **Database**: PostgreSQL
- **API**: Django REST Framework + GraphQL
- **Authentication**: JWT + Django Allauth
- **Payments**: Stripe
- **Task Queue**: Celery + Redis
- **Real-time**: Django Channels + WebSockets

### Frontend (Next.js)
- **Framework**: Next.js 14 (App Router)
- **Language**: TypeScript
- **Styling**: Tailwind CSS
- **UI Components**: Radix UI + Custom Components
- **State Management**: React Context + Hooks
- **Icons**: Lucide React
- **Theme**: next-themes

## 📦 Installation

### Prerequisites
- Python 3.11+
- Node.js 18+
- PostgreSQL 13+
- Redis (for Celery)

### Quick Start

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd haaju_project
   ```

2. **Install dependencies**
   ```bash
   # Install root dependencies
   npm install
   
   # Install frontend dependencies
   cd haaju_frontend && npm install
   
   # Install backend dependencies
   cd ../haaju_backend
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   pip install -r requirements/local.txt
   ```

3. **Set up environment variables**
   ```bash
   # Backend (.env file in haaju_backend/)
   cp haaju_backend/.env.example haaju_backend/.env
   
   # Frontend (.env.local file in haaju_frontend/)
   cp haaju_frontend/.env.example haaju_frontend/.env.local
   ```

4. **Set up the database**
   ```bash
   cd haaju_backend
   python manage.py migrate
   python manage.py createsuperuser
   ```

5. **Start the development servers**
   ```bash
   # From the root directory
   npm run dev
   ```

This will start both:
- Backend: http://localhost:8000
- Frontend: http://localhost:3000

## 🔧 Development

### Backend Development

```bash
cd haaju_backend

# Run Django development server
python manage.py runserver

# Run tests
python manage.py test

# Create migrations
python manage.py makemigrations

# Apply migrations
python manage.py migrate

# Run Celery worker
celery -A config worker -l info

# Run Celery beat (for scheduled tasks)
celery -A config beat -l info
```

### Frontend Development

```bash
cd haaju_frontend

# Run development server
npm run dev

# Build for production
npm run build

# Start production server
npm run start

# Run linting
npm run lint

# Type checking
npm run type-check
```

### Docker Development

```bash
# Start all services
docker-compose -f haaju_backend/docker-compose.local.yml up

# Start specific services
docker-compose -f haaju_backend/docker-compose.local.yml up django postgres redis
```

## 🌐 API Documentation

### REST API
The backend provides a comprehensive REST API at `/api/` with endpoints for:
- Authentication (`/api/auth/`)
- Projects (`/api/projects/`)
- Tasks (`/api/tasks/`)
- Users (`/api/users/`)
- Subscriptions (`/api/subscriptions/`)

### GraphQL API
GraphQL endpoint available at `/graphql/` with:
- Interactive GraphiQL interface
- Schema introspection
- Real-time subscriptions

### API Examples

```bash
# Get projects
curl -H "Authorization: Bearer <token>" http://localhost:8000/api/projects/

# Create a project
curl -X POST -H "Content-Type: application/json" \
  -H "Authorization: Bearer <token>" \
  -d '{"name":"My Project","description":"Project description"}' \
  http://localhost:8000/api/projects/
```

## 🎨 Design System

The frontend uses a custom design system with:

### Colors
- **Primary**: Deep blue (#030213)
- **Secondary**: Light gray with purple tint
- **Accent**: Light gray (#e9ebef)
- **Muted**: Light gray (#ececf0)
- **Destructive**: Red (#d4183d)

### Typography
- **Font**: Inter (Google Fonts)
- **Base Size**: 14px
- **Weights**: 400 (normal), 500 (medium)

### Components
- Button variants (default, outline, ghost, etc.)
- Card components with headers and content
- Form inputs with validation
- Theme toggle for dark/light mode

## 📱 Mobile Support

The application is fully responsive with mobile-specific optimizations:
- Touch-friendly button sizes (44px minimum)
- iOS zoom prevention
- Safe area support for notched devices
- Mobile-specific animations
- Optimized navigation for mobile

## 🔒 Security

- JWT-based authentication
- Role-based access control
- CSRF protection
- XSS prevention
- SQL injection protection
- Rate limiting
- Secure headers

## 🚀 Deployment

### Backend Deployment
```bash
# Production settings
export DJANGO_SETTINGS_MODULE=config.settings.production

# Collect static files
python manage.py collectstatic

# Run migrations
python manage.py migrate

# Start with Gunicorn
gunicorn config.wsgi:application
```

### Frontend Deployment
```bash
# Build for production
npm run build

# Start production server
npm run start
```

### Docker Production
```bash
# Build and run production containers
docker-compose -f haaju_backend/docker-compose.production.yml up -d
```

## 🧪 Testing

### Backend Tests
```bash
cd haaju_backend
python manage.py test
python manage.py test --coverage
```

### Frontend Tests
```bash
cd haaju_frontend
npm run test
npm run test:coverage
```

## 📊 Monitoring

- Django Debug Toolbar (development)
- Sentry integration for error tracking
- Performance monitoring
- Database query optimization

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

### Code Style
- Backend: Follow PEP 8 and Django coding style
- Frontend: ESLint + Prettier configuration
- TypeScript: Strict mode enabled

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🆘 Support

- **Documentation**: [Wiki](https://github.com/haaju/haaju-project/wiki)
- **Issues**: [GitHub Issues](https://github.com/haaju/haaju-project/issues)
- **Discussions**: [GitHub Discussions](https://github.com/haaju/haaju-project/discussions)
- **Email**: support@haaju.com

## 🙏 Acknowledgments

- Django community for the excellent framework
- Next.js team for the amazing React framework
- Tailwind CSS for the utility-first CSS framework
- All contributors and supporters

---

Built with ❤️ by the Haaju Team
