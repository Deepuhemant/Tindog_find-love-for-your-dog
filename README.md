<div align="center">

# 🐶 TinDog - Find Love for Your Dog

### *The Ultimate Dating App for Dogs*

[![GitHub Stars](https://img.shields.io/github/stars/Deepuhemant/Tindog_find-love-for-your-dog?style=social)](https://github.com/Deepuhemant/Tindog_find-love-for-your-dog)
[![GitHub Forks](https://img.shields.io/github/forks/Deepuhemant/Tindog_find-love-for-your-dog?style=social)](https://github.com/Deepuhemant/Tindog_find-love-for-your-dog/fork)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)

**Find the perfect match for your furry friend!**

[Demo](#demo) • [Features](#features) • [Tech Stack](#tech-stack) • [Installation](#installation) • [Contributing](#contributing)

</div>

---

## 📸 Demo

> *A beautiful, responsive dating platform designed specifically for dogs and their owners*

### ✨ Key Highlights
- 💕 **Swipe-based matching** - Like Tinder, but for dogs!
- 💬 **Real-time messaging** - Chat instantly when you match
- 📍 **Location-based** - Find dogs nearby
- 💳 **Subscription plans** - Free, Labrador (₹99/mo), Mastiff (₹199/mo)
- 📱 **Fully responsive** - Works on all devices

---

## 🌟 Features

### For Dog Owners
- ✅ Create detailed dog profiles with photos
- ✅ Browse nearby dogs with location filtering
- ✅ Like/Pass swipe interface
- ✅ Get instant match notifications
- ✅ Chat with other dog owners in real-time
- ✅ Subscription management for premium features

### Premium Features
| Feature | Free | Labrador (₹99/mo) | Mastiff (₹199/mo) |
|---------|------|----------------|---------------|
| Matches per day | 5 | Unlimited | Unlimited |
| Messages per day | 10 | Unlimited | Unlimited |
| App usage | Unlimited | Unlimited | Unlimited |
| Support | Email | Help center | Priority listing |

---

## 🚀 Tech Stack

### Frontend
<div>

![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=for-the-badge&logo=html5&logoColor=white)
![CSS3](https://img.shields.io/badge/CSS3-1572B6?style=for-the-badge&logo=css3&logoColor=white)
![Bootstrap](https://img.shields.io/badge/Bootstrap-7952B3?style=for-the-badge&logo=bootstrap&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)

</div>

- **HTML5** - Semantic markup
- **CSS3** - Custom styles with animations
- **Bootstrap 5** - Responsive grid and components
- **Font Awesome** - Beautiful icons
- **Google Fonts** - Montserrat & Ubuntu typography

### Backend
<div>

![Node.js](https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=node.js&logoColor=white)
![Express](https://img.shields.io/badge/Express-000000?style=for-the-badge&logo=express&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-336791?style=for-the-badge&logo=postgresql&logoColor=white)
![MongoDB](https://img.shields.io/badge/MongoDB-47A248?style=for-the-badge&logo=mongodb&logoColor=white)
![Redis](https://img.shields.io/badge/Redis-DC382D?style=for-the-badge&logo=redis&logoColor=white)
![Socket.IO](https://img.shields.io/badge/Socket.IO-010101?style=for-the-badge&logo=socket.io&logoColor=white)

</div>

- **Node.js** - JavaScript runtime
- **Express.js** - Web framework
- **PostgreSQL** - User & dog profiles database
- **MongoDB** - Chat messages storage
- **Redis** - Caching & session management
- **Socket.IO** - Real-time communication
- **JWT** - Authentication
- **Razorpay** - Payment processing
- **Cloudinary** - Image storage & optimization

---

## 📁 Project Structure

```
Tindog_find-love-for-your-dog/
├── 🎪 Frontend
│   ├── index.html              # Main landing page
│   ├── css/
│   │   └── styles.css         # Custom styles
│   └── images/                # Dog photos & assets
│
├── 🛠️ Backend
│   ├── server.js              # Express server & Socket.IO
│   ├── package.json           # Dependencies
│   ├── .env.example           # Environment template
│   ├── config/                # Database configs
│   ├── models/                # Data models
│   ├── routes/                # API endpoints
│   ├── controllers/           # Business logic
│   ├── middleware/            # Auth & validation
│   └── utils/                 # Helper functions
│
└── README.md               # You are here!
```

---

## 🛠️ Installation & Setup

### Prerequisites
- Node.js (v18 or higher)
- PostgreSQL (v14 or higher)
- MongoDB (v6 or higher)
- Redis (v7 or higher)

### Frontend Setup

1. **Clone the repository**
```bash
git clone https://github.com/Deepuhemant/Tindog_find-love-for-your-dog.git
cd Tindog_find-love-for-your-dog
```

2. **Open in browser**
```bash
# Simply open index.html in your browser
open index.html  # macOS
start index.html # Windows
xdg-open index.html # Linux
```

Or use a local server:
```bash
# Using Python
python -m http.server 8000

# Using Node.js
npx serve
```

Visit: `http://localhost:8000`

### Backend Setup

1. **Navigate to backend directory**
```bash
cd backend
```

2. **Install dependencies**
```bash
npm install
```

3. **Set up environment variables**
```bash
cp .env.example .env
# Edit .env with your actual credentials
```

4. **Create PostgreSQL database**
```bash
psql -U postgres
CREATE DATABASE tindog;
```

Run the SQL schema from `backend/README.md`

5. **Start the server**
```bash
# Development mode with auto-reload
npm run dev

# Production mode
npm start
```

Server runs at: `http://localhost:5000`

6. **Test the API**
```bash
curl http://localhost:5000/api/health
```

---

## 📡 API Documentation

### Base URL
```
http://localhost:5000/api
```

### Authentication Endpoints
```http
POST   /auth/register      # Register new user
POST   /auth/login         # Login user  
POST   /auth/logout        # Logout user
POST   /auth/refresh       # Refresh token
```

### Dog Profile Endpoints
```http
POST   /dogs               # Create dog profile
GET    /dogs/:id           # Get dog details
PUT    /dogs/:id           # Update dog profile
DELETE /dogs/:id           # Delete dog profile  
GET    /dogs/nearby        # Get nearby dogs (geolocation)
```

### Matching Endpoints
```http
POST   /matches/like/:id   # Like a dog
POST   /matches/pass/:id   # Pass on a dog
GET    /matches            # Get all matches
GET    /matches/suggestions # Get match suggestions
```

### Messaging Endpoints (Socket.IO)
```javascript
// Connect
socket.connect('http://localhost:5000');

// Join room
socket.emit('join-room', matchId);

// Send message
socket.emit('send-message', { roomId, message });

// Receive message
socket.on('receive-message', (data) => { });
```

### Subscription Endpoints
```http
GET    /subscriptions/plans    # Get available plans
POST   /subscriptions/create   # Create subscription
POST   /subscriptions/cancel   # Cancel subscription
```

---

## 📦 Database Schema

### PostgreSQL Tables

**users**
```sql
id, email, password_hash, name, phone, created_at
```

**dogs**
```sql
id, user_id, name, breed, age, gender, bio, photos[], location (POINT), created_at
```

**matches**
```sql
id, dog_id_1, dog_id_2, status, created_at
```

**likes**
```sql
id, liker_dog_id, liked_dog_id, created_at
```

**subscriptions**
```sql
id, user_id, plan, status, start_date, end_date, amount
```

### MongoDB Collections

**messages**
```json
{
  "matchId": "...",
  "senderId": "...",
  "content": "...",
  "timestamp": "...",
  "read_status": true
}
```

---

## 🔐 Security Features

- ✅ JWT-based authentication
- ✅ Password hashing with bcrypt
- ✅ Rate limiting (100 req/15min)
- ✅ Helmet.js security headers
- ✅ Input validation & sanitization
- ✅ SQL injection prevention
- ✅ XSS protection
- ✅ CORS configuration
- ✅ Environment variable protection

---

## 🚀 Deployment

### Frontend Deployment (Netlify/Vercel)

```bash
# Build command (if using build tools)
npm run build

# Deploy
netlify deploy --prod
# or
vercel deploy --prod
```

### Backend Deployment (Railway/Render)

```bash
# Install Railway CLI
npm i -g @railway/cli

# Deploy
railway login
railway init
railway up
```

**Environment Variables**: Set all `.env` variables in your hosting platform.

---

## 🧑‍💻 Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

---

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 📞 Contact & Support

- **Developer**: Deepuhemant
- **GitHub**: [@Deepuhemant](https://github.com/Deepuhemant)
- **Email**: your_email@example.com

---

## 🚀 Roadmap

- [ ] Add video chat feature
- [ ] Implement AI-based matching algorithm
- [ ] Add dog breed filters
- [ ] Create mobile app (React Native)
- [ ] Add social media sharing
- [ ] Implement dog walking meetup scheduling
- [ ] Add vet recommendations
- [ ] Create admin dashboard

---

## 🙏 Acknowledgments

- Bootstrap team for the amazing framework
- Font Awesome for beautiful icons
- All the dog lovers who inspired this project
- Community contributors

---

<div align="center">

### Made with ❤️ for dogs looking for love!

🐶 **Happy Matching!** 🐶

⭐ **Star this repo if you found it helpful!** ⭐

</div>
