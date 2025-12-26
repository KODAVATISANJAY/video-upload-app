# Video Upload App

A full-stack video upload application built with modern technologies for uploading, managing, and streaming videos with real-time progress tracking and basic authentication.

## Features

✨ **Core Features:**
- **Video Upload**: Upload videos with progress tracking
- **Authentication**: JWT-based user authentication
- **Sensitivity Flags**: Mark videos as low, medium, or high sensitivity
- **Video Streaming**: Stream videos with range request support
- **Video Management**: View, delete, and manage uploaded videos
- **Real-time Progress**: Track upload progress in real-time
- **Responsive UI**: Clean and user-friendly interface

## Tech Stack

### Frontend
- **React 18**: UI library
- **Vite**: Build tool and dev server
- **TypeScript**: Type safety
- **Axios**: HTTP client
- **CSS**: Styling

### Backend
- **Node.js**: Runtime environment
- **Express.js**: Web framework
- **MongoDB**: Database
- **Mongoose**: ODM for MongoDB
- **JWT**: Authentication
- **Multer**: File upload handling
- **bcryptjs**: Password hashing
- **CORS**: Cross-origin resource sharing

## Project Structure

```
video-upload-app/
├── backend/
│   ├── src/
│   │   ├── models/
│   │   │   └── Video.js
│   │   ├── middleware/
│   │   ├── routes/
│   │   ├── controllers/
│   │   └── server.js
│   ├── .env
│   ├── package.json
│   └── .gitignore
├── frontend/
│   ├── src/
│   │   ├── components/
│   │   ├── types/
│   │   ├── App.tsx
│   │   └── main.tsx
│   ├── index.html
│   ├── vite.config.ts
│   ├── tsconfig.json
│   └── package.json
└── README.md
```

## Getting Started

### Prerequisites
- Node.js (v14 or higher)
- MongoDB (local or cloud)
- npm or yarn

### Backend Setup

1. Navigate to the backend folder:
```bash
cd backend
```

2. Install dependencies:
```bash
npm install
```

3. Create `.env` file with your configuration:
```
MONGODB_URI=mongodb://localhost:27017/video-upload-app
JWT_SECRET=your_jwt_secret_key
PORT=5000
NODE_ENV=development
```

4. Create uploads folder:
```bash
mkdir uploads
```

5. Start the backend server:
```bash
npm run dev
```

The backend will run on `http://localhost:5000`

### Frontend Setup

1. Navigate to the frontend folder:
```bash
cd frontend
```

2. Install dependencies:
```bash
npm install
```

3. Create `.env` file:
```
VITE_API_URL=http://localhost:5000/api
```

4. Start the development server:
```bash
npm run dev
```

The frontend will run on `http://localhost:5173`

## API Endpoints

### Authentication
- `POST /api/auth/register` - Register a new user
- `POST /api/auth/login` - Login user

### Videos
- `POST /api/videos` - Upload a video (requires auth)
- `GET /api/videos` - Get all videos for authenticated user
- `DELETE /api/videos/:id` - Delete a video (requires auth)
- `GET /api/videos/stream/:id` - Stream a video

## Usage

1. **Register/Login**: Create an account or login with existing credentials
2. **Upload Video**: Click upload button, select a video file, add title and description
3. **Set Sensitivity**: Choose sensitivity level (low/medium/high)
4. **Track Progress**: Monitor upload progress in real-time
5. **Manage Videos**: View all your uploaded videos, stream them, or delete

## Environment Variables

### Backend (.env)
```
MONGODB_URI=mongodb://localhost:27017/video-upload-app
JWT_SECRET=your_secret_key_change_this_in_production
PORT=5000
NODE_ENV=development
```

### Frontend (.env)
```
VITE_API_URL=http://localhost:5000/api
```

## Deployment

### Backend Deployment (Render/Vercel)
1. Push to GitHub
2. Connect repository to Render/Vercel
3. Set environment variables
4. Deploy

### Frontend Deployment (Vercel/Netlify)
1. Run `npm run build`
2. Deploy `dist` folder to Vercel/Netlify

## Contributing

Contributions are welcome! Please fork the repository and submit pull requests.

## License

MIT License - feel free to use this project for learning and development.

## Author

KODAVATISANIJAY

## Support

For issues and questions, please create an issue in the GitHub repository.
