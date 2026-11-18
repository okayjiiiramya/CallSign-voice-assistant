# 🎙️ CallSign

**A voice-controlled AI assistant you name yourself.** Pick an avatar, give your assistant a name, and that name becomes its wake word. Say it, and CallSign searches Google and YouTube, plays songs, opens websites, tells you the time, or answers your questions, all hands-free.

Built with the **MERN stack** and **Llama 3.3 70B** (via the Groq API).

🔗 **Live demo:** https://call-sign-voice-assistant.vercel.app

---

## ✨ Features

- 🗣️ **Wake-word activation:** the assistant responds only when it hears the name you gave it
- 🎧 **Voice in, voice out:** speech recognition for input, speech synthesis for spoken replies
- 🧠 **Natural-language intent detection:** the LLM turns what you say into a structured action
- 🌐 **Browser actions:** Google search, YouTube search, play a song on YouTube, open any website, Instagram, Facebook, calculator, weather
- 🕒 **Date and time:** current time, date, day and month
- 💬 **General Q&A:** conversational answers read aloud
- 🖼️ **Customizable assistant:** choose a preset avatar or upload your own (stored on Cloudinary)
- 🔐 **Authentication:** sign up and sign in with JWT in an httpOnly cookie and bcrypt-hashed passwords
- 🗂️ **Command history:** every command is saved to your account
- 📱 **Responsive UI** built with Tailwind CSS

---

## 🧩 How It Works

```
You speak  →  Web Speech API (browser)
          →  POST /api/user/asktoassistant
          →  Groq (Llama 3.3 70B) returns JSON: { type, userInput, response }
          →  Backend handles time/date types itself
          →  Frontend opens the right link in a new tab and speaks the response
```

Supported intent types: `general`, `google-search`, `youtube-search`, `youtube-play`, `youtube-open`, `website-open`, `calculator-open`, `instagram-open`, `facebook-open`, `weather-show`, `get-time`, `get-date`, `get-day`, `get-month`.

---

## 🗣️ Example Commands

Say your assistant's name first (for example, "CallSign"):

- "CallSign, play Shape of You by Ed Sheeran"
- "CallSign, search for React tutorials on YouTube"
- "CallSign, open amazon"
- "CallSign, google best laptops under 50000"
- "CallSign, what's the time?"
- "CallSign, who invented the telephone?"

---

## 🛠️ Tech Stack

| Layer | Technologies |
|---|---|
| Frontend | React 19, Vite, Tailwind CSS 4, React Router, Axios, React Icons |
| Backend | Node.js, Express 5, Mongoose, JWT, bcryptjs, Multer, Moment |
| Database | MongoDB |
| Storage | Cloudinary (avatar uploads) |
| AI | Groq API, `llama-3.3-70b-versatile` |
| Voice | Web Speech API (SpeechRecognition and SpeechSynthesis) |

---

## 📂 Project Structure

```
.
├── index.js                 # Express server entry
├── grok.js                  # Groq API call and intent prompt
├── config/
│   ├── db.js                # MongoDB connection
│   ├── token.js             # JWT generation
│   └── cloudinary.js        # Image upload helper
├── controllers/
│   ├── auth.controllers.js  # signUp, Login, logOut
│   └── user.controllers.js  # current user, update assistant, ask assistant, YouTube ID
├── middlewares/
│   ├── isAuth.js            # JWT cookie verification
│   └── multer.js            # File upload handling
├── models/
│   └── user.model.js        # User schema
├── routes/
│   ├── auth.routes.js
│   └── user.routes.js
└── frontend/
    └── src/
        ├── pages/           # SignUp, SignIn, Customize, Customize2, Home
        ├── components/      # Card
        ├── context/         # UserContext (global state and API helpers)
        └── assets/
```

---

## 🚀 Getting Started

### Prerequisites

- Node.js 18+
- A MongoDB database (local or Atlas)
- A [Groq API key](https://console.groq.com)
- A [Cloudinary](https://cloudinary.com) account (for avatar uploads)
- Google Chrome or Microsoft Edge (speech recognition support)

### 1. Clone the repository

```bash
git clone https://github.com/okayjiiiramya/AI-virual-assitant.git
cd AI-virual-assitant
```

### 2. Set up the backend

```bash
npm install
mkdir public        # temporary folder used by Multer for uploads
```

Create a `.env` file in the project root:

```env
PORT=8000
MONGODB_URL=your_mongodb_connection_string
JWT_SECRET=your_long_random_secret
GROQ_API_KEY=your_groq_api_key
FRONTEND_URL=http://localhost:5173

CLOUDINARY_CLOUD_NAME=your_cloud_name
CLOUDINARY_API_KEY=your_api_key
CLOUDINARY_API_SECRET=your_api_secret
```

Start the server:

```bash
npm run dev
```

### 3. Set up the frontend

```bash
cd frontend
npm install
```

Create `frontend/.env`:

```env
VITE_BACKEND_URL=http://localhost:8000
```

Start the app:

```bash
npm run dev
```

Open **http://localhost:5173**, sign up, choose an avatar, name your assistant, and allow microphone access when the browser asks.

---

## 🔑 Environment Variables

| Variable | Where | Description |
|---|---|---|
| `PORT` | backend | Server port (default `8000`) |
| `MONGODB_URL` | backend | MongoDB connection string |
| `JWT_SECRET` | backend | Secret used to sign auth tokens |
| `GROQ_API_KEY` | backend | Groq API key for the LLM |
| `FRONTEND_URL` | backend | Allowed CORS origin (default `http://localhost:5173`) |
| `CLOUDINARY_CLOUD_NAME`, `CLOUDINARY_API_KEY`, `CLOUDINARY_API_SECRET` | backend | Cloudinary credentials |
| `NODE_ENV` | backend | Set to `production` when deployed (enables secure cross-site cookies) |
| `VITE_BACKEND_URL` | frontend | URL of the deployed backend |

---

## 🔌 API Reference

| Method | Endpoint | Auth | Description |
|---|---|---|---|
| POST | `/api/auth/signup` | No | Create an account |
| POST | `/api/auth/signin` | No | Log in |
| GET | `/api/auth/logout` | No | Log out |
| GET | `/api/user/current` | Yes | Get the logged-in user |
| POST | `/api/user/update` | Yes | Save assistant name and image |
| POST | `/api/user/asktoassistant` | Yes | Send a voice command, get an action and reply |
| GET | `/api/user/youtube-video-id?query=` | Yes | Get the first YouTube video ID for a query |

---

## ☁️ Deployment

- **Frontend:** deploy the `frontend/` folder to Vercel and set `VITE_BACKEND_URL`.
- **Backend:** deploy to a Node host such as Render, set all backend env variables, set `NODE_ENV=production`, and set `FRONTEND_URL` to your Vercel URL.

---

## ⚠️ Notes

- Voice recognition works best in Chrome and Edge. Firefox does not support the Web Speech Recognition API.
- The microphone is always listening while you are on the home screen, but commands run only when your assistant's name is heard.
- Groq's free tier is rate limited. The backend retries automatically if a limit is hit.

---

## 🔮 Future Improvements

- 🌍 Multi-language voice support
- 🧠 Conversation memory across commands
- 🎵 Music and app integrations
- 📊 Admin dashboard
- 📂 File upload and analysis

---

## 👩‍💻 Author

**Ramya Gupta**: [@okayjiiiramya](https://github.com/okayjiiiramya)

If you like this project, give it a ⭐!
