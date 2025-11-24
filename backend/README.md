# 🐕 TinDog Backend API

Backend API for TinDog - A dating app for dogs built with Node.js, Express, PostgreSQL, MongoDB, and Socket.IO.

## 🚀 Tech Stack

- **Runtime:** Node.js (v18+)
- **Framework:** Express.js
- **Databases:**
  - PostgreSQL (User profiles, dogs, matches)
  - MongoDB (Chat messages)
  - Redis (Caching, sessions)
- **Authentication:** JWT + bcrypt
- **Real-time:** Socket.IO
- **Payment:** Razorpay
- **File Upload:** Multer + Cloudinary
- **Security:** Helmet, CORS, Rate Limiting

## 📁 Project Structure

```
backend/
├── server.js                 # Main entry point
├── package.json             # Dependencies
├── .env                     # Environment variables (create this)
├── .env.example             # Environment template
├── config/
│   ├── database.js          # Database connections
│   ├── cloudinary.js        # Cloudinary config
│   └── razorpay.js          # Payment config
├── models/
│   ├── User.js              # PostgreSQL User model
│   ├── Dog.js               # PostgreSQL Dog model
│   ├── Match.js             # PostgreSQL Match model
│   ├── Subscription.js      # PostgreSQL Subscription model
│   └── Message.js           # MongoDB Message model
├── routes/
│   ├── auth.js              # Authentication routes
│   ├── users.js             # User CRUD routes
│   ├── dogs.js              # Dog profile routes
│   ├── matches.js           # Matching system routes
│   ├── messages.js          # Messaging routes
│   └── subscriptions.js     # Payment/subscription routes
├── controllers/
│   ├── authController.js    # Auth logic
│   ├── userController.js    # User logic
│   ├── dogController.js     # Dog profile logic
│   ├── matchController.js   # Matching algorithm
│   ├── messageController.js # Messaging logic
│   └── subscriptionController.js # Payment logic
├── middleware/
│   ├── auth.js              # JWT verification
│   ├── validate.js          # Input validation
│   ├── upload.js            # File upload handling
│   └── errorHandler.js      # Global error handler
└── utils/
    ├── tokenGenerator.js    # JWT helper
    ├── emailService.js      # Email sending
    └── matchingAlgorithm.js # Match logic
```

## ⚙️ Environment Variables

Create a `.env` file in the backend directory:

```env
# Server
PORT=5000
NODE_ENV=development
FRONTEND_URL=http://localhost:3000

# PostgreSQL Database
DB_HOST=localhost
DB_PORT=5432
DB_NAME=tindog
DB_USER=your_postgres_user
DB_PASSWORD=your_postgres_password

# MongoDB
MONGO_URI=mongodb://localhost:27017/tindog_messages

# Redis
REDIS_HOST=localhost
REDIS_PORT=6379

# JWT
JWT_SECRET=your_super_secret_jwt_key_change_this
JWT_EXPIRE=7d

# Cloudinary (Image Storage)
CLOUDINARY_CLOUD_NAME=your_cloud_name
CLOUDINARY_API_KEY=your_api_key
CLOUDINARY_API_SECRET=your_api_secret

# Razorpay (Payments)
RAZORPAY_KEY_ID=your_razorpay_key_id
RAZORPAY_KEY_SECRET=your_razorpay_secret

# Email (Nodemailer)
EMAIL_HOST=smtp.gmail.com
EMAIL_PORT=587
EMAIL_USER=your_email@gmail.com
EMAIL_PASSWORD=your_app_password
```

## 🛠️ Installation & Setup

### 1. Install Dependencies
```bash
cd backend
npm install
```

### 2. Setup Databases

**PostgreSQL:**
```sql
CREATE DATABASE tindog;

CREATE TABLE users (
  id SERIAL PRIMARY KEY,
  email VARCHAR(255) UNIQUE NOT NULL,
  password_hash VARCHAR(255) NOT NULL,
  name VARCHAR(100),
  phone VARCHAR(20),
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE dogs (
  id SERIAL PRIMARY KEY,
  user_id INTEGER REFERENCES users(id) ON DELETE CASCADE,
  name VARCHAR(100) NOT NULL,
  breed VARCHAR(100),
  age INTEGER,
  gender VARCHAR(10),
  bio TEXT,
  photos TEXT[],
  location GEOMETRY(POINT, 4326),
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE matches (
  id SERIAL PRIMARY KEY,
  dog_id_1 INTEGER REFERENCES dogs(id) ON DELETE CASCADE,
  dog_id_2 INTEGER REFERENCES dogs(id) ON DELETE CASCADE,
  status VARCHAR(20) DEFAULT 'pending',
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE likes (
  id SERIAL PRIMARY KEY,
  liker_dog_id INTEGER REFERENCES dogs(id) ON DELETE CASCADE,
  liked_dog_id INTEGER REFERENCES dogs(id) ON DELETE CASCADE,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE subscriptions (
  id SERIAL PRIMARY KEY,
  user_id INTEGER REFERENCES users(id) ON DELETE CASCADE,
  plan VARCHAR(50) NOT NULL,
  status VARCHAR(20) DEFAULT 'active',
  start_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  end_date TIMESTAMP,
  amount DECIMAL(10, 2)
);
```

**MongoDB:**
```javascript
// Messages collection will be auto-created
// Schema: { matchId, senderId, content, timestamp, read_status }
```

### 3. Run Development Server
```bash
npm run dev
```

Server will start at `http://localhost:5000`

### 4. Test API
```bash
curl http://localhost:5000/api/health
```

Expected response:
```json
{
  "status": "OK",
  "message": "TinDog API is running!"
}
```

## 📡 API Endpoints

### Authentication
```
POST   /api/auth/register      - Register new user
POST   /api/auth/login         - Login user
POST   /api/auth/logout        - Logout user
POST   /api/auth/refresh       - Refresh access token
```

### User Management
```
GET    /api/users/profile      - Get user profile
PUT    /api/users/profile      - Update user profile
DELETE /api/users/profile      - Delete account
```

### Dog Profiles
```
POST   /api/dogs               - Create dog profile
GET    /api/dogs/:id           - Get dog profile
PUT    /api/dogs/:id           - Update dog profile
DELETE /api/dogs/:id           - Delete dog profile
GET    /api/dogs/nearby        - Get nearby dogs (geolocation)
```

### Matching System
```
POST   /api/matches/like/:id   - Like a dog
POST   /api/matches/pass/:id   - Pass on a dog
GET    /api/matches            - Get all matches
GET    /api/matches/suggestions - Get match suggestions
```

### Messaging
```
GET    /api/messages/:matchId  - Get conversation
POST   /api/messages/:matchId  - Send message
DELETE /api/messages/:id       - Delete message
```

### Subscriptions
```
GET    /api/subscriptions/plans    - Get available plans
POST   /api/subscriptions/create   - Create subscription
POST   /api/subscriptions/cancel   - Cancel subscription
GET    /api/subscriptions/status   - Check subscription status
```

## 🔐 Authentication Flow

1. User registers → Returns JWT token
2. Token stored in client (localStorage/cookie)
3. Include token in header: `Authorization: Bearer <token>`
4. Middleware verifies token on protected routes

## 💬 Real-time Chat (Socket.IO)

```javascript
// Client connects
socket.connect('http://localhost:5000');

// Join chat room
socket.emit('join-room', matchId);

// Send message
socket.emit('send-message', { roomId: matchId, message: 'Hello!' });

// Receive message
socket.on('receive-message', (data) => {
  console.log(data);
});
```

## 🎯 Next Steps

### To Complete Implementation:

1. **Create Database Config** (`config/database.js`)
2. **Create Models** (User, Dog, Match, etc.)
3. **Create Controllers** (Business logic)
4. **Create Routes** (API endpoints)
5. **Create Middleware** (Auth, validation)
6. **Add Matching Algorithm** (Location-based)
7. **Integrate Razorpay** (Payment processing)
8. **Set up Cloudinary** (Image uploads)
9. **Add Email Service** (Notifications)
10. **Write Tests** (Jest + Supertest)

## 🚢 Deployment

### Railway / Render
```bash
# Install Railway CLI
npm i -g @railway/cli

# Login and deploy
railway login
railway init
railway up
```

### Environment Variables (Production)
- Set all `.env` variables in deployment platform
- Use production database URLs
- Enable HTTPS
- Configure CORS for production frontend URL

## 📚 Useful Commands

```bash
# Install dependencies
npm install

# Run development
npm run dev

# Run production
npm start

# Run tests
npm test
```

## 🛡️ Security Features

- ✅ JWT authentication
- ✅ Password hashing with bcrypt
- ✅ Rate limiting (100 requests/15min)
- ✅ Helmet.js security headers
- ✅ Input validation
- ✅ SQL injection prevention
- ✅ XSS protection
- ✅ CORS configuration

## 💰 Subscription Plans

| Plan | Price | Features |
|------|-------|----------|
| **Chihuahua** | Free | 5 matches/day, 10 messages/day |
| **Labrador** | ₹99/mo | Unlimited matches & messages |
| **Mastiff** | ₹199/mo | Priority listing + all features |

## 📞 Support

For questions or issues, contact: your_email@example.com

## 📄 License

MIT License - Feel free to use this project!

---

**Built with ❤️ for dogs looking for love! 🐕‍🦺💕**
