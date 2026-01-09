# Quick Setup Guide

## 1. Install Dependencies (Already Done)
The project was created with `create-next-app` and dependencies are installed.

## 2. Run Development Server
```bash
npm run dev
```

## 3. Open in Browser
Navigate to: http://localhost:3000

The app will redirect to `/gallery` automatically.

## 4. Testing the App

### Desktop Testing
1. Open Chrome DevTools (F12)
2. Toggle Device Toolbar (Ctrl+Shift+M / Cmd+Shift+M)
3. Select a mobile device (e.g., iPhone 12 Pro)
4. Click the FAB (+) button to start recording

### Mobile Testing (Recommended)
Since this is a camera-based PWA, testing on a real device is important.

**Option A: Local Network**
1. Find your computer's local IP: `ipconfig` (Windows) or `ifconfig` (Mac/Linux)
2. Run `npm run dev`
3. On your mobile device, navigate to: `http://YOUR_IP:3000`
4. Grant camera permissions when prompted

**Option B: HTTPS Tunnel (Required for some features)**
```bash
# Install ngrok
npm install -g ngrok

# In terminal 1
npm run dev

# In terminal 2
ngrok http 3000
```
Use the HTTPS URL provided by ngrok on your mobile device.

## 5. Key Features to Test

### ✅ Video Recording
- [ ] Camera permission request works
- [ ] Front/back camera toggle works
- [ ] Recording timer shows
- [ ] Pause/resume during recording
- [ ] Video saves after stopping

### ✅ Storage Persistence
- [ ] Close tab during recording
- [ ] Reopen - video appears in gallery with "Pending" status
- [ ] Storage meter shows usage

### ✅ Upload Queue
- [ ] Videos automatically enter upload queue
- [ ] Status changes: Pending → Uploading → Completed (or Failed)
- [ ] Failed uploads show "Retry" button
- [ ] "Retry All Failed" batch action works

### ✅ Gallery Management
- [ ] Videos display in grid
- [ ] Click thumbnail to play video
- [ ] Download button saves to device
- [ ] Delete button removes video
- [ ] Status badges update in real-time

## 6. Browser DevTools Inspection

### Storage
1. Open DevTools → Application tab
2. Check "Storage" section:
   - **IndexedDB**: `video-recorder-db` and `upload-queue-db`
   - **OPFS**: Check "File System" (Chrome 102+)

### Console
Watch for logs:
- "OPFS initialized successfully" or fallback to IndexedDB
- "Persistent storage: granted"
- Chunk saves during recording
- Upload attempts and results

## 7. Common Issues

### Camera Not Working
- **HTTPS Required**: Use ngrok or deploy to Vercel/Netlify
- **Permissions**: Check browser settings
- **DevTools**: Some browsers restrict camera in device emulation

### OPFS Not Available
- Falls back to IndexedDB automatically
- Check browser compatibility (Chrome 86+, Safari 15.2+)

### Videos Not Uploading
- Mock upload has 80% success rate (intentional for testing)
- Check browser console for upload logs
- Failed videos can be manually retried

## 8. Production Deployment

### Vercel (Recommended)
```bash
npm install -g vercel
vercel
```

### Manual Build
```bash
npm run build
npm start
```

## 9. Next Steps

1. **Replace Mock Upload**: Edit `lib/upload-queue.ts` → `mockUpload()` function
2. **Add Real API**: Integrate with your backend (AWS S3, Azure, etc.)
3. **Generate Thumbnails**: Use Canvas API to extract video frames
4. **Add Service Worker**: Use `next-pwa` for offline support
5. **Authentication**: Add user accounts and cloud sync

## 10. File Structure Overview

```
recorder/
├── app/
│   ├── record/page.tsx       # Camera recording interface
│   ├── gallery/page.tsx      # Video gallery with CRUD
│   ├── page.tsx              # Root → redirects to gallery
│   └── layout.tsx            # PWA metadata
├── lib/
│   ├── types.ts              # TypeScript interfaces
│   ├── video-storage.ts      # OPFS/IndexedDB wrapper
│   └── upload-queue.ts       # Retry queue manager
├── components/
│   ├── video-card.tsx        # Gallery item
│   └── storage-meter.tsx     # Quota display
├── public/
│   ├── manifest.json         # PWA manifest
│   ├── icon-192.svg          # App icon
│   ├── icon-512.svg          # App icon
│   └── storage-worker.js     # OPFS worker (optional)
└── README.md                 # Full documentation
```

## Support

- **Next.js Docs**: https://nextjs.org/docs
- **OPFS Guide**: https://web.dev/file-system-access/
- **MediaRecorder API**: https://developer.mozilla.org/en-US/docs/Web/API/MediaRecorder
