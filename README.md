# AI-Auto-Clip-Mobile-JS-Edition-
A React Native-based mobile app that uses AI Agent to automatically detect viral moments, trim clips, add subtitles, and fill in the backsound using FFmpeg.
🎬 AI Clip Generator - Automatic Mobile Video Editor


An intelligent mobile video editor application that leverages Google Gemini AI to automatically detect viral moments in videos, generate short clips with intelligent timestamping, and apply professional text overlays and audio effects.
Perfect for: Content creators, TikTok producers, YouTube shorts creators, and social media managers.

📋 Table of Contents

* ✨ Features
* 🏗️ Tech Stack
* 📦 Installation
* ⚙️ Configuration
* 🚀 Quick Start Guide
* 📁 Project Structure
* 🔧 API Setup
* 💾 Database Setup
* 📖 Usage Guide
* 🐛 Troubleshooting
* 🎨 Customization
* 🚀 Deployment
* 📚 Documentation
* 🤝 Contributing
* 📄 License


✨ Features
🤖 AI-Powered Video Analysis

* Automatic Viral Moment Detection - Uses Gemini 1.5 Flash to identify engaging segments
* Smart Timestamping - Generates 15-60 second clips based on content
* Mood Classification - Categorizes clips by emotion (Energetic, Sad, Happy, Funny, Inspiring)
* Reasoning Explanations - Provides detailed reasoning for each detected moment
* Real-time Processing - Analyzes video while you wait with progress tracking

🎬 Advanced Video Processing

* Smart Clip Cutting - Automatically trims videos at optimal points
* Custom Text Overlays - Add styled text with customizable fonts
* Audio Mixing - Combines original audio with background music
* Multiple Quality Presets - Export in 480p to 1080p HD
* Professional Effects - Watermark, audio volume control, transitions

🎨 Customization & Branding

* Font Library - Import and manage custom .ttf/.otf fonts
* Music Management - Upload your own background music
* Watermark System - Add logo or text watermarks
* Audio Control - Fine-tune audio levels (0-200%)
* Quality Settings - Adjust bitrate, resolution, and encoding

📤 Export & Sharing

* Batch Export - Process multiple clips simultaneously
* Social Media Ready - Optimized presets for TikTok, Instagram, YouTube
* Share Directly - Export to messaging apps and social platforms
* Size Estimation - Predict file size before export
* Time Estimation - Know rendering time in advance

💾 Cloud Integration

* Supabase Backend - Secure cloud storage for metadata
* User Authentication - Email/password login system
* Project History - Track all your videos and clips
* Cloud Sync - Access projects across devices
* Backup Support - Automatic metadata backup

📊 Professional Features

* Real-time Progress - Visual feedback during processing
* Video Preview - Built-in player with playback controls
* Clip Management - Delete, rename, and organize clips
* Performance Optimization - Efficient memory and battery usage
* Error Recovery - Handles processing interruptions gracefully


🏗️ Tech Stack
Frontend Framework
TechnologyPurposeVersionReact NativeCross-platform mobile framework0.73.0ExpoDevelopment & managed deployment50.0.0AxiosHTTP client for API requests^1.6.0AsyncStorageLocal device storage^1.21.0
AI & Video Processing
TechnologyPurposeVersionGoogle Gemini 1.5 FlashVideo analysis & AI insightsAPI v1betaFFmpeg KitVideo encoding & transformation^6.0.0expo-avNative video playback~14.0.0expo-image-pickerMedia library access~14.7.0
Backend & Database
TechnologyPurposeVersionSupabaseBackend as a Service (PostgreSQL)^2.38.0PostgreSQLRelational databaseCloud-hostedJWTSecure authenticationBuilt-in Supabase
Additional Libraries
LibraryPurposeVersionexpo-document-pickerFile import dialog~11.5.0expo-file-systemFile I/O operations~16.0.0react-native-shareSocial media sharing^10.0.0lottie-react-nativeAnimation support6.4.0

📦 Installation
System Requirements

* Node.js ≥ 16.0.0
* npm or yarn package manager
* Expo CLI (npm install -g expo-cli)
* Mobile Device with Expo Go app OR Android/iOS Simulator

Step 1: Clone Repository
bashDownloadCopy codegit clone https://github.com/yourusername/ai-clip-generator.git
cd ai-clip-generator
Step 2: Install Dependencies
bashDownloadCopy codenpm install
Or with yarn:
bashDownloadCopy codeyarn install
Step 3: Create Environment File
bashDownloadCopy codecp .env.example .env
Step 4: Configure Credentials
Edit .env with your API keys:
envDownloadCopy code# Google Gemini API
REACT_APP_GEMINI_API_KEY=AIzaSy...xxxxx
REACT_APP_GEMINI_MODEL=gemini-1.5-flash
REACT_APP_API_BASE_URL=https://generativelanguage.googleapis.com/v1beta/

# Supabase Database
REACT_APP_SUPABASE_URL=https://xxxxx.supabase.co
REACT_APP_SUPABASE_KEY=eyJhbGc...xxxxx

# App Settings
REACT_APP_APP_NAME=AI Clip Generator
REACT_APP_VERSION=1.0.0
REACT_APP_DEBUG=false
Step 5: Launch Application
bashDownloadCopy code# Start development server
expo start

# Options:
# Press 'i' → iOS Simulator
# Press 'a' → Android Emulator
# Press 'w' → Web Browser
# Scan QR Code → Expo Go App (iOS/Android)

⚙️ Configuration
Environment Variables Reference
File Location: .env (root directory)
envDownloadCopy code# ========================================
# GOOGLE GEMINI AI CONFIGURATION
# ========================================
# Get from: https://aistudio.google.com/app/apikey
REACT_APP_GEMINI_API_KEY=your_api_key_here

# Model selection (recommended: gemini-1.5-flash)
REACT_APP_GEMINI_MODEL=gemini-1.5-flash

# API endpoint (do not modify)
REACT_APP_API_BASE_URL=https://generativelanguage.googleapis.com/v1beta/

# ========================================
# SUPABASE DATABASE CONFIGURATION
# ========================================
# Get from: https://app.supabase.com/project/[project-id]/settings/api
REACT_APP_SUPABASE_URL=https://your-project.supabase.co
REACT_APP_SUPABASE_KEY=your_supabase_anon_key

# ========================================
# APPLICATION SETTINGS
# ========================================
# Application name
REACT_APP_APP_NAME=AI Clip Generator

# Current version
REACT_APP_VERSION=1.0.0

# Debug mode (set to 'true' for verbose logging)
REACT_APP_DEBUG=false

# Maximum video duration (seconds)
REACT_APP_MAX_VIDEO_DURATION=600

# Default export quality preset
REACT_APP_DEFAULT_QUALITY=FULL_HD
FFmpeg Configuration
Default presets are optimized for mobile devices:
javascriptDownloadCopy code{
  ULTRA_FAST: {
    codec: 'libx264',
    preset: 'ultrafast',
    bitrate: '800k',
    resolution: '640x360'
  },
  MOBILE: {
    codec: 'libx264',
    preset: 'fast',
    bitrate: '1500k',
    resolution: '854x480'
  },
  FULL_HD: {
    codec: 'libx264',
    preset: 'medium',
    bitrate: '3000k',
    resolution: '1280x720'
  },
  HD: {
    codec: 'libx264',
    preset: 'medium',
    bitrate: '5000k',
    resolution: '1920x1080'
  }
}
AI Analysis Prompt
The system prompt sent to Gemini API:
Analyze this video and identify 3 viral-worthy segments.

Requirements:
1. Identify attention-grabbing hooks in the first 3 seconds
2. Provide segments between 15-60 seconds
3. Output MUST be a clean JSON ARRAY
4. Include fields: start (MM:SS), end (MM:SS), title, mood, reasoning

Mood categories: Energetic, Sad, Happy, Funny, Inspiring

Return ONLY valid JSON array, no additional text.


🚀 Quick Start Guide
Typical Workflow (5 Minutes)
┌─────────────────────────────────┐
│  1. Open App & Select Video     │
└──────────────┬──────────────────┘
               ↓
┌─────────────────────────────────┐
│  2. Configure Custom Assets     │
│     (Fonts & Music - Optional)  │
└──────────────┬──────────────────┘
               ↓
┌─────────────────────────────────┐
│  3. Run AI Analysis             │
│     ⏱️ Wait 30-60 seconds       │
└──────────────┬──────────────────┘
               ↓
┌─────────────────────────────────┐
│  4. Review AI Recommendations   │
│     (3 Viral Clips Generated)   │
└──────────────┬──────────────────┘
               ↓
┌─────────────────────────────────┐
│  5. Render Selected Clips       │
│     ⏱️ Wait for processing      │
└──────────────┬──────────────────┘
               ↓
┌─────────────────────────────────┐
│  6. Configure Export Settings   │
│     (Quality, Watermark, etc.)  │
└──────────────┬──────────────────┘
               ↓
┌─────────────────────────────────┐
│  7. Export Final Videos         │
│     ⏱️ Processing may take time │
└──────────────┬──────────────────┘
               ↓
┌─────────────────────────────────┐
│  8. Preview, Share, Download    │
│     🎉 Ready for Social Media!  │
└─────────────────────────────────┘

Step-by-Step Instructions
1. Select Video from Gallery
Home Screen
  ↓
Tap "📹 Select Video" Button
  ↓
Grant Gallery Permissions
  ↓
Choose Video File
  ↓
Display: Video name, Size, Duration

2. Setup Custom Fonts (Optional)
Asset Picker Section
  ↓
Tap "Select Font" Button
  ↓
Choose from Saved Fonts or "+ Add New"
  ↓
Browse Device Files
  ↓
Select .ttf/.otf Font File
  ↓
Auto-saved to App Assets Folder

3. Choose Background Music (Optional)
Asset Picker Section
  ↓
Tap "Select Music" Button
  ↓
Choose from Saved Music or "+ Add New"
  ↓
Browse Device Files
  ↓
Select .mp3/.wav/.m4a Audio File
  ↓
Auto-saved to App Assets Folder

4. Analyze Video with AI
Tap "🤖 Analyze Video" Button
  ↓
Modal Appears with Progress
  ↓
Stages:
  - Converting video...       (0-20%)
  - Sending to AI...          (20-40%)
  - Processing response...    (40-80%)
  - Analysis complete!        (100%)
  ↓
Results: 3 AI-generated Clips

5. Review & Preview Clips
Clip List Section
  ↓
Scroll through 3 Recommendations
  ↓
Each Clip Shows:
  - Title
  - Mood (badge with color)
  - Time Range (MM:SS - MM:SS)
  - AI Reasoning
  ↓
Tap "👁️ Preview" to Watch
  ↓
Tap "📤 Share" to Export
  ↓
Tap "🗑️" to Delete

6. Render Clips with FFmpeg
Tap "🎨 Render All Clips" Button
  ↓
Processing Overlay Appears
  ↓
For Each Clip:
  - Cutting video segment
  - Applying text overlay
  - Mixing audio
  - Encoding to MP4
  ↓
Progress: 0-100% per clip
  ↓
All Rendered Clips Ready

7. Configure Export Settings
Export Settings Panel
  ↓
Quality Preset:
  ├─ 480p Mobile (lowest quality, fastest)
  ├─ 720p Full HD (balanced)
  ├─ 1080p HD (high quality)
  └─ Custom (advanced)
  ↓
Watermark:
  ├─ Enable/Disable
  └─ Enter Custom Text
  ↓
Audio Volume:
  ├─ Slider 0-200%
  └─ Preview estimated time/size

8. Export Final Videos
Tap "📤 Export" Button
  ↓
Processing Overlay
  ↓
Batch Process Each Clip:
  - Re-encode if needed
  - Apply watermark
  - Adjust audio levels
  - Save to storage
  ↓
Progress: Per-clip tracking
  ↓
Export Complete ✅

9. Share or Download
In Clip Card:
  ↓
Tap "📤 Share":
  └─ Share to WhatsApp, Instagram, etc.
  ↓
Tap "👁️ Preview":
  └─ Watch full video in modal
  ↓
Tap "🗑️ Delete":
  └─ Remove from app
  ↓
Manual Download:
  └─ File stored in DocumentDirectory


📁 Project Structure
ai-clip-generator/
│
├── 📄 App.js                          # Application entry point
├── 📄 app.json                        # Expo configuration
├── 📄 package.json                    # Dependencies & scripts
├── 📄 .env                            # Environment variables (DO NOT COMMIT)
├── 📄 .env.example                    # Template for .env
├── 📄 README.md                       # This file
├── 📄 .gitignore                      # Git ignore rules
│
├── 📁 src/
│   │
│   ├── 📁 services/
│   │   ├── VideoEngine.js             # Core video processing engine
│   │   │   ├── analyzeVideoWithGemini()
│   │   │   ├── renderClip()
│   │   │   ├── processVideoWithClips()
│   │   │   └── Helper methods
│   │   │
│   │   ├── DatabaseService.js         # Supabase integration
│   │   │   ├── saveSourceVideo()
│   │   │   ├── saveClips()
│   │   │   ├── getUserVideos()
│   │   │   └── Auth methods
│   │   │
│   │   ├── AssetManager.js            # Font & music management
│   │   │   ├── pickFont()
│   │   │   ├── pickMusic()
│   │   │   ├── getSavedFonts()
│   │   │   └── File I/O helpers
│   │   │
│   │   └── ExportService.js           # Export & quality presets
│   │       ├── exportClip()
│   │       ├── batchExportClips()
│   │       ├── createCompilation()
│   │       └── Utility methods
│   │
│   ├── 📁 components/
│   │   ├── VideoPicker.js             # Video selection component
│   │   ├── AnalysisState.js           # AI analysis progress modal
│   │   ├── ClipList.js                # Scrollable clips list
│   │   ├── ClipCard.js                # Individual clip card UI
│   │   ├── RenderingOverlay.js        # Rendering progress overlay
│   │   ├── AssetPicker.js             # Font & music selection
│   │   └── ExportSettings.js          # Export configuration panel
│   │
│   └── 📁 screens/
│       └── HomeScreen.js              # Main application screen
│           ├── State management
│           ├── Component integration
│           ├── Event handlers
│           └── UI layout
│
├── 📁 assets/
│   ├── 📁 fonts/
│   │   └── Montserrat-Bold.ttf        # Default font
│   ├── 📁 music/
│   │   └── bg-music.mp3               # Default background music
│   └── 📁 images/
│       ├── app-icon.png
│       ├── splash-screen.png
│       └── logo.png
│
├── 📁 docs/
│   ├── API_SETUP.md                   # Detailed Gemini API setup
│   ├── DATABASE_SETUP.md              # Supabase configuration
│   ├── ARCHITECTURE.md                # Technical architecture
│   ├── TROUBLESHOOTING.md             # Common issues & fixes
│   └── PERFORMANCE.md                 # Optimization tips
│
└── 📁 config/
    ├── ffmpeg-presets.js              # FFmpeg command templates
    └── export-presets.js              # Export quality presets


🔧 API Setup
Create Google Cloud Project

1. 
Navigate to Google Cloud Console

URL: https://console.cloud.google.com
Sign in with Google account


2. 
Create New Project
Click "Select a Project" → "New Project"
Project Name: "AI Clip Generator"
Organization: (leave default)
Click "CREATE"


3. 
Enable Required APIs
Left Menu → "APIs & Services" → "Library"

Search & Enable:
✓ Google Generative AI API
✓ Cloud Logging API (optional)


4. 
Create API Key
Left Menu → "APIs & Services" → "Credentials"
Click "+ CREATE CREDENTIALS" → "API Key"
Copy the generated key


5. 
Configure .env
bashDownloadCopy codeREACT_APP_GEMINI_API_KEY=AIzaSy...your_key_here

6. 
Test Connection
javascriptDownloadCopy code// Quick test in terminal
curl -X POST "https://generativelanguage.googleapis.com/v1beta/models/gemini-1.5-flash:generateContent?key=YOUR_KEY" \
-H "Content-Type: application/json" \
-d '{
  "contents": [{
    "parts": [{
      "text": "Hello Gemini"
    }]
  }]
}'


API Rate Limits
TierRequests/MinCostFree60$0Standard1,000Pay-as-you-goPremium10,000Custom pricing
Gemini API Request Format
javascriptDownloadCopy codeconst request = {
  contents: [{
    parts: [
      {
        text: "Analyze this video..."
      },
      {
        inline_data: {
          mime_type: "video/mp4",
          data: "base64_encoded_video_data"
        }
      }
    ]
  }]
};

const response = await axios.post(
  `${GEMINI_BASE_URL}models/gemini-1.5-flash:generateContent?key=${GEMINI_API_KEY}`,
  request
);
Expected Response Format
jsonDownloadCopy code{
  "candidates": [{
    "content": {
      "parts": [{
        "text": "[{\"start\": \"00:15\", \"end\": \"00:45\", ...}]"
      }]
    }
  }]
}

💾 Database Setup
Create Supabase Project

1. 
Sign Up / Login

URL: https://supabase.com
Click "Sign Up"
Verify email


2. 
Create Organization & Project
New Project → Organization: Your Name
Project Name: "AI Clip Generator"
Database Password: (save securely)
Region: (select nearest region)
Pricing: Free tier for development


3. 
Wait for Initialization

Usually takes 2-3 minutes
You'll receive confirmation email


4. 
Copy Credentials
Project Settings → API

Copy to .env:
REACT_APP_SUPABASE_URL=https://xxxxx.supabase.co
REACT_APP_SUPABASE_KEY=eyJhbGc...



Create Database Tables
Navigate to: Supabase Dashboard → SQL Editor
Run SQL Script:
sqlDownloadCopy code-- ==========================================
-- TABLE: source_videos
-- Description: Stores video metadata
-- ==========================================
CREATE TABLE source_videos (
  id BIGINT PRIMARY KEY GENERATED ALWAYS AS IDENTITY,
  user_id UUID REFERENCES auth.users(id) ON DELETE CASCADE NOT NULL,
  original_path TEXT NOT NULL,
  filename TEXT NOT NULL,
  duration INTEGER,
  file_size INTEGER,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT TIMEZONE('utc', NOW()),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT TIMEZONE('utc', NOW())
);

-- Indexes for performance
CREATE INDEX idx_source_videos_user_id ON source_videos(user_id);
CREATE INDEX idx_source_videos_created_at ON source_videos(created_at DESC);

-- ==========================================
-- TABLE: ai_clips
-- Description: Stores AI-generated clips
-- ==========================================
CREATE TABLE ai_clips (
  id BIGINT PRIMARY KEY GENERATED ALWAYS AS IDENTITY,
  user_id UUID REFERENCES auth.users(id) ON DELETE CASCADE NOT NULL,
  source_video_id BIGINT REFERENCES source_videos(id) ON DELETE CASCADE NOT NULL,
  title TEXT NOT NULL,
  mood TEXT CHECK (mood IN ('Energetic', 'Sad', 'Happy', 'Funny', 'Inspiring')),
  reasoning TEXT,
  start_time TEXT NOT NULL,
  end_time TEXT NOT NULL,
  output_path TEXT,
  file_size INTEGER,
  clip_index INTEGER,
  status TEXT DEFAULT 'pending' CHECK (status IN ('pending', 'rendering', 'rendered', 'exported', 'error')),
  created_at TIMESTAMP WITH TIME ZONE DEFAULT TIMEZONE('utc', NOW()),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT TIMEZONE('utc', NOW())
);

-- Indexes
CREATE INDEX idx_ai_clips_user_id ON ai_clips(user_id);
CREATE INDEX idx_ai_clips_source_video_id ON ai_clips(source_video_id);
CREATE INDEX idx_ai_clips_status ON ai_clips(status);
Enable Row Level Security (RLS)
sqlDownloadCopy code-- Enable RLS for source_videos
ALTER TABLE source_videos ENABLE ROW LEVEL SECURITY;

-- Policy: Users can only see their own videos
CREATE POLICY "users_can_view_own_videos"
  ON source_videos FOR SELECT
  USING (auth.uid() = user_id);

-- Policy: Users can insert their own videos
CREATE POLICY "users_can_insert_own_videos"
  ON source_videos FOR INSERT
  WITH CHECK (auth.uid() = user_id);

-- Policy: Users can delete their own videos
CREATE POLICY "users_can_delete_own_videos"
  ON source_videos FOR DELETE
  USING (auth.uid() = user_id);

-- Enable RLS for ai_clips
ALTER TABLE ai_clips ENABLE ROW LEVEL SECURITY;

-- Similar policies for ai_clips
CREATE POLICY "users_can_view_own_clips"
  ON ai_clips FOR SELECT
  USING (auth.uid() = user_id);

CREATE POLICY "users_can_insert_own_clips"
  ON ai_clips FOR INSERT
  WITH CHECK (auth.uid() = user_id);

CREATE POLICY "users_can_delete_own_clips"
  ON ai_clips FOR DELETE
  USING (auth.uid() = user_id);

CREATE POLICY "users_can_update_own_clips"
  ON ai_clips FOR UPDATE
  USING (auth.uid() = user_id);
Test Database Connection
javascriptDownloadCopy code// Add to your app initialization
import DatabaseService from './services/DatabaseService';

const initializeApp = async () => {
  try {
    const user = await DatabaseService.getCurrentUser();
    if (user) {
      console.log('✅ Database connected for:', user.email);
    } else {
      console.log('ℹ️ No authenticated user');
    }
  } catch (error) {
    console.error('❌ Database connection failed:', error.message);
  }
};

// Call on app start
useEffect(() => {
  initializeApp();
}, []);

📖 Usage Guide
Service Documentation
VideoEngine.js - Core Processing Engine
javascriptDownloadCopy codeimport VideoEngine from './services/VideoEngine';

// 1. Analyze video with Gemini AI
const clips = await VideoEngine.analyzeVideoWithGemini(
  videoPath,
  (progress) => {
    console.log(`Analysis: ${progress.percent}% - ${progress.stage}`);
  }
);
// Returns: Array of clip objects with metadata

// 2. Render individual clip
const result = await VideoEngine.renderClip(
  inputVideoPath,
  clipData, // { start: "00:15", end: "00:45", title: "...", ... }
  outputFilePath,
  (progress) => {
    console.log(`Rendering: ${progress.percent}%`);
  }
);
// Returns: { success: true, path: outputPath }

// 3. Complete pipeline (analyze + render)
const pipelineResult = await VideoEngine.processVideoWithClips(
  inputVideoPath,
  outputDirectory,
  (progress) => 
