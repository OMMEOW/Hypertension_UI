# Hypertension Tracker

A comprehensive blood pressure tracking application built with Next.js, TypeScript, and Prisma. Track your hypertension journey with secure authentication, interactive visualizations, and medical report management.

## 🚀 Features

- **Secure Authentication**: Email/SMS OTP verification
- **Blood Pressure Tracking**: Add, view, and analyze measurements
- **Interactive Analytics**: Charts with risk zones and trends
- **Medical Reports**: Upload and manage health documents
- **Mobile-First Design**: Responsive UI optimized for all devices
- **HIPAA Compliant**: Enterprise-grade security and privacy
- **Export Capabilities**: Download data and reports

## 🛠 Tech Stack

### Frontend
- **Next.js 14** (App Router)
- **TypeScript** - Type safety and better DX
- **Tailwind CSS** - Utility-first styling
- **shadcn/ui** - Beautiful, accessible components
- **Lucide React** - Icon library
- **Recharts** - Interactive data visualization

### Backend
- **Next.js API Routes** - Serverless API endpoints
- **Prisma** - Type-safe database ORM
- **PostgreSQL** - Reliable relational database
- **JWT** - Secure authentication tokens

### External Services
- **Twilio** - SMS OTP delivery
- **SendGrid** - Email OTP delivery
- **AWS S3** - File storage for medical reports
- **Supabase** - Alternative auth and database option

### Development
- **ESLint** - Code linting
- **Prettier** - Code formatting
- **Husky** - Git hooks
- **Vitest** - Unit testing
- **Cypress** - E2E testing

## 📋 Prerequisites

- Node.js 18+ 
- PostgreSQL database
- Twilio account (for SMS)
- SendGrid account (for email)
- AWS S3 bucket (for file uploads)

## 🚀 Quick Start

### 1. Clone the Repository

```bash
git clone <repository-url>
cd hypertension-tracker
```

### 2. Install Dependencies

```bash
npm install
# or
yarn install
```

### 3. Environment Setup

Copy the environment example file and configure your variables:

```bash
cp env.example .env.local
```

Update `.env.local` with your configuration:

```env
# Database
DATABASE_URL="postgresql://username:password@localhost:5432/hypertension_tracker"

# Authentication
JWT_SECRET="your-super-secret-jwt-key-here"
JWT_REFRESH_SECRET="your-super-secret-refresh-key-here"

# Twilio (SMS)
TWILIO_ACCOUNT_SID="your-twilio-account-sid"
TWILIO_AUTH_TOKEN="your-twilio-auth-token"
TWILIO_PHONE_NUMBER="+1234567890"

# SendGrid (Email)
SENDGRID_API_KEY="your-sendgrid-api-key"
SENDGRID_FROM_EMAIL="noreply@yourdomain.com"

# AWS S3
AWS_ACCESS_KEY_ID="your-aws-access-key"
AWS_SECRET_ACCESS_KEY="your-aws-secret-key"
AWS_REGION="us-east-1"
AWS_S3_BUCKET="your-s3-bucket-name"
```

### 4. Database Setup

```bash
# Generate Prisma client
npm run db:generate

# Run database migrations
npm run db:migrate

# Seed the database with sample data
npm run db:seed
```

### 5. Start Development Server

```bash
npm run dev
# or
yarn dev
```

Open [http://localhost:3000](http://localhost:3000) to view the application.

## 🧪 Testing

### Unit Tests
```bash
npm run test
```

### E2E Tests
```bash
npm run test:e2e
```

### Test Coverage
```bash
npm run test:coverage
```

## 📊 Sample Data

The seed script creates 3 sample users with realistic blood pressure data:

### Test Credentials
- **Email**: `john.doe@example.com`
- **Phone**: `+1234567890`
- **Test OTP**: `123456` (for john.doe@example.com)

### Sample Users
1. **John Doe** - Normal to elevated BP (30 readings)
2. **Jane Smith** - Elevated to stage 1 (25 readings)
3. **Mike Wilson** - Stage 1 to stage 2 (35 readings)

## 🏗 Architecture

```
├── app/                    # Next.js App Router
│   ├── api/               # API routes
│   ├── dashboard/         # Dashboard page
│   ├── login/            # Authentication
│   ├── measurements/     # BP tracking
│   └── globals.css       # Global styles
├── components/           # Reusable UI components
│   └── ui/              # shadcn/ui components
├── lib/                 # Utility functions
├── prisma/              # Database schema & migrations
└── hooks/               # Custom React hooks
```

## 🔐 Authentication Flow

1. User enters email or phone number
2. System generates 6-digit OTP
3. OTP sent via email (SendGrid) or SMS (Twilio)
4. User verifies OTP
5. JWT token created and stored in HTTP-only cookie
6. User redirected to dashboard

## 📱 Pages & Routes

### Public
- `/` - Landing page with hero and trust badges

### Authentication
- `/login` - Email/phone input and OTP verification
- `/verify` - OTP input and verification
- `/register` - Profile completion for new users

### App (Authenticated)
- `/dashboard` - Overview with quick cards and mini graph
- `/measurements` - List view with search and sort
- `/measurements/new` - Add new BP reading
- `/reports` - Upload and manage medical reports
- `/analytics` - Interactive charts and insights
- `/profile` - User profile and settings
- `/help` - FAQ and support

## 🗄 Database Schema

```sql
-- Users table
users (
  id, email, phone, name, dob, gender, 
  weight, height, consent, created_at, updated_at
)

-- OTP table
otps (
  id, user_id, contact, code_hash, purpose, 
  expires_at, used, created_at
)

-- Measurements table
measurements (
  id, user_id, recorded_at, systolic, diastolic, 
  heart_rate, notes, created_at
)

-- Reports table
reports (
  id, user_id, filename, s3_key, mime, size, 
  uploaded_at, extracted_text
)

-- Sessions table
sessions (
  id, user_id, token, expires_at, created_at
)
```

## 🔧 API Endpoints

### Authentication
- `POST /api/auth/send-otp` - Send verification code
- `POST /api/auth/verify` - Verify OTP and create session

### Measurements
- `GET /api/measurements` - Get user's measurements
- `POST /api/measurements` - Create new measurement
- `PUT /api/measurements/:id` - Update measurement
- `DELETE /api/measurements/:id` - Delete measurement

### User Profile
- `GET /api/user/profile` - Get user profile
- `PUT /api/user/profile` - Update user profile

### Reports
- `GET /api/reports/upload-url` - Get presigned S3 URL
- `POST /api/reports` - Create report entry
- `GET /api/reports` - Get user's reports

## 🎨 Design System

### Colors
- **Primary**: `#0EA5A3` (Teal-500)
- **Background**: `#F8FAFC` (Slate-50)
- **Cards**: White with subtle shadows

### Typography
- **Font**: Inter (Google Fonts)
- **Weights**: 300, 400, 500, 600, 700

### Components
- **Buttons**: Primary (teal), Ghost (white border)
- **Cards**: White background, rounded corners, subtle shadow
- **Inputs**: Rounded borders, focus states with teal ring

## 🔒 Security & Privacy

- **HTTPS Only**: All communications encrypted
- **HTTP-Only Cookies**: JWT tokens stored securely
- **Rate Limiting**: OTP requests and verification attempts
- **Data Encryption**: Sensitive fields encrypted at rest
- **Audit Logging**: Track authentication and data access
- **GDPR/HIPAA Compliance**: Data export and deletion endpoints

## 🚀 Deployment

### Vercel (Recommended)

1. Connect your GitHub repository to Vercel
2. Configure environment variables in Vercel dashboard
3. Deploy automatically on push to main branch

### Manual Deployment

```bash
# Build the application
npm run build

# Start production server
npm start
```

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🆘 Support

- **Documentation**: [Wiki](link-to-wiki)
- **Issues**: [GitHub Issues](link-to-issues)
- **Discussions**: [GitHub Discussions](link-to-discussions)

## 🙏 Acknowledgments

- [shadcn/ui](https://ui.shadcn.com/) for beautiful components
- [Tailwind CSS](https://tailwindcss.com/) for utility-first styling
- [Prisma](https://www.prisma.io/) for type-safe database access
- [Next.js](https://nextjs.org/) for the amazing React framework

---

Built with ❤️ for better health tracking #   H y p e r t e n s i o n _ U I  
 #   H y p e r t e n s i o n _ U I  
 