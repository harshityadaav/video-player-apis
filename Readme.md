# Video Player APIs

A full-featured YouTube-like backend REST API built with Node.js, Express, MongoDB, and Cloudinary. Supports video uploads, user authentication, tweets, comments, likes, playlists, subscriptions, and channel dashboards.

## Tech Stack

- **Runtime:** Node.js (ESM)
- **Framework:** Express.js
- **Database:** MongoDB via Mongoose
- **Media Storage:** Cloudinary
- **Auth:** JWT (access + refresh tokens)
- **File Uploads:** Multer
- **Password Hashing:** bcrypt

## Project Structure

```
src/
├── controllers/       # Route handlers (business logic)
├── models/            # Mongoose schemas
├── routes/            # Express routers
├── middlewares/       # Auth (JWT) & file upload (Multer)
├── utils/             # ApiError, ApiResponse, asyncHandler, Cloudinary
├── db/                # MongoDB connection
├── app.js             # Express app setup
└── index.js           # Entry point
```

## Getting Started

### Prerequisites

- Node.js v18+
- MongoDB instance (local or Atlas)
- Cloudinary account

### Installation

```bash
git clone https://github.com/harshityadaav/video-player-apis.git
cd video-player-apis
npm install
```

### Environment Variables

Create a `.env` file in the root directory:

```env
PORT=8000
MONGODB_URI=mongodb://localhost:27017
DB_NAME=videoplayer

CORS_ORIGIN=*

ACCESS_TOKEN_SECRET=your_access_token_secret
ACCESS_TOKEN_EXPIRY=1d
REFRESH_TOKEN_SECRET=your_refresh_token_secret
REFRESH_TOKEN_EXPIRY=10d

CLOUDINARY_CLOUD_NAME=your_cloud_name
CLOUDINARY_API_KEY=your_api_key
CLOUDINARY_API_SECRET=your_api_secret
```

### Run

```bash
npm run dev
```

Server starts at `http://localhost:8000`.

---

## API Reference

All routes are prefixed with `/api/v1`.

### Healthcheck

| Method | Endpoint | Auth | Description |
|--------|----------|------|-------------|
| GET | `/healthcheck` | No | Server health status |

---

### Users — `/users`

| Method | Endpoint | Auth | Description |
|--------|----------|------|-------------|
| POST | `/register` | No | Register with avatar & cover image |
| POST | `/login` | No | Login, returns access & refresh tokens |
| POST | `/logout` | Yes | Logout current user |
| POST | `/refresh-token` | No | Get new access token via refresh token |
| POST | `/change-password` | Yes | Change current password |
| GET | `/current-user` | Yes | Get authenticated user's profile |
| PATCH | `/update-account` | Yes | Update fullName and email |
| PATCH | `/avatar` | Yes | Update avatar image |
| PATCH | `/cover-image` | Yes | Update cover image |
| GET | `/c/:username` | Yes | Get public channel profile |
| GET | `/history` | Yes | Get watch history |

---

### Videos — `/videos`

| Method | Endpoint | Auth | Description |
|--------|----------|------|-------------|
| GET | `/` | Yes | Get all videos (paginated) |
| POST | `/` | Yes | Upload a video with thumbnail |
| GET | `/:videoId` | Yes | Get video by ID |
| PATCH | `/:videoId` | Yes | Update video title/description/thumbnail |
| DELETE | `/:videoId` | Yes | Delete a video |
| PATCH | `/toggle/publish/:videoId` | Yes | Toggle video publish status |

---

### Tweets — `/tweets`

| Method | Endpoint | Auth | Description |
|--------|----------|------|-------------|
| POST | `/` | Yes | Create a tweet |
| GET | `/user/:userId` | Yes | Get all tweets by a user |
| PATCH | `/:tweetId` | Yes | Update a tweet |
| DELETE | `/:tweetId` | Yes | Delete a tweet |

---

### Comments — `/comments`

| Method | Endpoint | Auth | Description |
|--------|----------|------|-------------|
| GET | `/:videoId` | Yes | Get all comments for a video |
| POST | `/:videoId` | Yes | Add a comment to a video |
| PATCH | `/c/:commentId` | Yes | Update a comment |
| DELETE | `/c/:commentId` | Yes | Delete a comment |

---

### Likes — `/likes`

| Method | Endpoint | Auth | Description |
|--------|----------|------|-------------|
| POST | `/toggle/v/:videoId` | Yes | Toggle like on a video |
| POST | `/toggle/c/:commentId` | Yes | Toggle like on a comment |
| POST | `/toggle/t/:tweetId` | Yes | Toggle like on a tweet |
| GET | `/videos` | Yes | Get all liked videos |

---

### Playlists — `/playlist`

| Method | Endpoint | Auth | Description |
|--------|----------|------|-------------|
| POST | `/` | Yes | Create a new playlist |
| GET | `/:playlistId` | Yes | Get playlist by ID |
| PATCH | `/:playlistId` | Yes | Update playlist name/description |
| DELETE | `/:playlistId` | Yes | Delete a playlist |
| PATCH | `/add/:videoId/:playlistId` | Yes | Add video to playlist |
| PATCH | `/remove/:videoId/:playlistId` | Yes | Remove video from playlist |
| GET | `/user/:userId` | Yes | Get all playlists of a user |

---

### Subscriptions — `/subscriptions`

| Method | Endpoint | Auth | Description |
|--------|----------|------|-------------|
| POST | `/c/:channelId` | Yes | Toggle subscription to a channel |
| GET | `/c/:channelId` | Yes | Get channels subscribed to |
| GET | `/u/:subscriberId` | Yes | Get subscribers of a channel |

---

### Dashboard — `/dashboard`

| Method | Endpoint | Auth | Description |
|--------|----------|------|-------------|
| GET | `/stats` | Yes | Get channel stats (views, subscribers, videos, likes) |
| GET | `/videos` | Yes | Get all videos for the authenticated channel |

---

## Authentication

Protected routes require a JWT access token sent in one of:

- `Authorization: Bearer <token>` header
- `accessToken` cookie

Use `POST /api/v1/users/refresh-token` with a valid `refreshToken` cookie to obtain a new access token without re-login.

## Models

| Model | Key Fields |
|-------|-----------|
| User | username, email, fullName, avatar, coverImage, watchHistory, password, refreshToken |
| Video | videoFile, thumbnail, title, description, duration, views, isPublished, owner |
| Comment | content, video, owner |
| Tweet | content, owner |
| Like | video / comment / tweet, likedBy |
| Playlist | name, description, videos[], owner |
| Subscription | subscriber, channel |

## License

ISC — Harshit Yadav
