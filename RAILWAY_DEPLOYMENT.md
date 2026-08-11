# Railway Deployment Guide

This guide will help you deploy the YouTube Automation Agent to Railway.

## Prerequisites

- Railway account: https://railway.app
- GitHub account (linked to Railway)
- Required API keys:
  - OpenAI API key (or alternative AI provider: Gemini, OpenRouter, etc.)
  - YouTube OAuth credentials (JSON file)
  - Optional: ElevenLabs API key for better TTS

## Deployment Steps

### 1. Create a New Railway Project

1. Go to https://railway.app/dashboard
2. Click "New Project"
3. Select "Deploy from GitHub repo"
4. Connect your GitHub account and select `Youtube-automation-agent-render-ready`

### 2. Configure Environment Variables

In the Railway dashboard, add these environment variables in the "Variables" tab:

**Required:**
```
NODE_ENV=production
PORT=3456
OPENAI_API_KEY=your-api-key-here
(or use GEMINI_API_KEY, OPENROUTER_API_KEY, etc.)
```

**Channel Configuration:**
```
CHANNEL_NAME=Your Channel Name
DEFAULT_AUTHOR=Your Name
TARGET_AUDIENCE=Your target audience description
YOUTUBE_REGION=US
DEFAULT_PRIVACY_STATUS=private
```

**Optional but Recommended:**
```
API_KEY=your-secure-api-key-for-endpoints
ELEVENLABS_API_KEY=your-elevenlabs-key (for better TTS)
ELEVENLABS_VOICE_ID=your-voice-id
LOG_LEVEL=info
```

**YouTube OAuth (if uploading to YouTube):**
- Upload your Google OAuth credentials JSON file
- Set: `GOOGLE_APPLICATION_CREDENTIALS=/app/credentials.json`

### 3. Persistent Storage

Railway automatically provides persistent storage for:
- `/app/data/` - Database files
- `/app/uploads/` - Generated content

These are configured in `railway.toml` and preserved across deployments.

### 4. Deploy

Railway will automatically:
1. Detect the Dockerfile
2. Build the Docker image
3. Install dependencies
4. Start the application with `npm start`
5. Health check every 30 seconds

### 5. Access Your Deployment

Once deployed, Railway provides a public URL:
- **Main Dashboard:** `https://your-railway-url.railway.app/`
- **Health Check:** `https://your-railway-url.railway.app/health`
- **Analytics:** `https://your-railway-url.railway.app/analytics`
- **Schedule:** `https://your-railway-url.railway.app/schedule`

### 6. Monitoring

View logs in Railway dashboard:
1. Select your project
2. Click "Deployments"
3. View real-time logs

## API Endpoints

### Health Check
```bash
curl https://your-railway-url.railway.app/health
```

### Generate Content
```bash
curl -X POST https://your-railway-url.railway.app/generate \
  -H "Content-Type: application/json" \
  -H "x-api-key: your-api-key" \
  -d '{
    "topic": "AI Trends 2025",
    "style": "educational",
    "length": "medium"
  }'
```

### Get Schedule
```bash
curl https://your-railway-url.railway.app/schedule
```

### Get Analytics
```bash
curl https://your-railway-url.railway.app/analytics
```

## Troubleshooting

### Build Fails
- Check Node.js version (requires Node 18+)
- Review build logs in Railway dashboard
- Ensure all dependencies are in `package.json`

### Application Won't Start
1. Check environment variables are set correctly
2. Review startup logs for credential validation errors
3. Ensure all required API keys are provided

### API Requests Return 401
- Verify `API_KEY` environment variable is set
- Include `x-api-key` header in requests (if API_KEY is configured)

### Out of Memory
- Railway provides up to 8GB for hobby tier
- Optimize image generation settings
- Reduce video resolution in production

## Scaling

To scale your deployment:
1. Go to Railway dashboard → Project → Settings
2. Adjust RAM allocation
3. Enable auto-scaling if needed

## Cost Optimization

- Use Railway's free tier for testing
- Pay-as-you-go for production
- Monitor resource usage in dashboard
- Consider using cheaper AI providers (Gemini free tier, OpenRouter)

## Updating Your Deployment

1. Push changes to GitHub
2. Railway automatically rebuilds and redeploys
3. No downtime with persistent storage

## Support

- Railway docs: https://docs.railway.app
- Join Railway community: https://railway.app/community

---

**Happy automating! 🎉**
