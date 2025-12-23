# Chief of Staff - Frontend

Web interface for Chief of Staff AI assistant.

## Features

- Google Sign-In authentication
- Dashboard with real-time stats
- Briefs list (morning & meeting)
- Today's schedule view
- User settings panel
- Responsive design
- Clean, modern UI

## Setup

### 1. Configure Google Client ID

Edit `index.html` (line ~42):

```html
data-client_id="YOUR_GOOGLE_CLIENT_ID"
```

Replace with your actual Google OAuth Client ID from Google Cloud Console.

### 2. Configure Backend URL (Optional)

If backend is not at `http://localhost:8000`, edit `index.html` (line ~11 in JavaScript):

```javascript
const API_URL = 'http://localhost:8000';  // Change this
```

### 3. Run Local Server

**Option A: Python**
```bash
python -m http.server 3000
```

**Option B: Node.js**
```bash
npx http-server -p 3000
```

**Option C: VS Code Live Server**
- Install "Live Server" extension
- Right-click `index.html`
- Select "Open with Live Server"

### 4. Open Browser

```
http://localhost:3000
```

## File Structure

```
frontend/
├── index.html          # Complete single-page application
├── README.md           # This file
├── .gitignore          # Git ignore rules
└── package.json        # NPM config (optional)
```

## How It Works

### Single HTML File Architecture

All code is in `index.html`:

1. **HTML** - Page structure (lines 1-400)
2. **CSS** - Styling in `<style>` tags (lines 10-350)
3. **JavaScript** - Logic in `<script>` tags (lines 400-650)

### Components

**Login Page:**
- Google Sign-In button
- Feature showcase
- Responsive design

**Dashboard:**
- Header with user info
- Stats cards (meetings, briefs, next meeting)
- Tab navigation (Briefs, Schedule, Settings)
- Real-time data updates

**Briefs Tab:**
- List of all briefs
- Filter by type (all, morning, meeting)
- Unread indicators
- Click to view full brief

**Schedule Tab:**
- Today's meetings from Google Calendar
- Time and attendee info
- Refresh button

**Settings Tab:**
- Morning brief time selector
- Notification toggle
- Save button

## Customization

### Change Colors

Edit the `:root` section in `<style>`:

```css
:root {
    --primary: #2563eb;        /* Main brand color */
    --primary-dark: #1e40af;   /* Darker shade */
    --success: #10b981;        /* Success color */
    --danger: #ef4444;         /* Error color */
    --warning: #f59e0b;        /* Warning color */
}
```

### Change Layout

Find the relevant HTML section and modify:
- `.stats-grid` - Stats cards
- `.tabs` - Tab navigation
- `.briefs-list` - Briefs display

### Add Features

Add new tabs:
1. Add tab button in `.tabs` section
2. Create new `.tab-panel`
3. Add JavaScript handler in `setupEventListeners()`

## API Integration

Frontend calls these backend endpoints:

```javascript
// Authentication
POST /auth/google

// User data
GET /user/me?user_email={email}
PATCH /user/settings?user_email={email}

// Briefs
GET /briefs?user_email={email}
GET /briefs/{id}?user_email={email}
PATCH /briefs/{id}/read?user_email={email}

// Schedule
GET /schedule/today?user_email={email}
```

## State Management

User state stored in `localStorage`:

```javascript
// Save user
localStorage.setItem('user', JSON.stringify(userData));

// Get user
const user = JSON.parse(localStorage.getItem('user'));

// Clear user (logout)
localStorage.removeItem('user');
```

## Deployment

### Option 1: Netlify

1. Drag and drop `frontend` folder to Netlify
2. Update `API_URL` to your backend URL
3. Update Google OAuth redirect URIs

### Option 2: Vercel

```bash
vercel deploy
```

### Option 3: GitHub Pages

1. Push to GitHub
2. Enable GitHub Pages in settings
3. Select branch and `/` root

### Option 4: Static Hosting

Upload `index.html` to any static host:
- AWS S3
- Google Cloud Storage
- Cloudflare Pages
- Firebase Hosting

## Configuration Checklist

Before deployment:

- [ ] Update Google Client ID in HTML
- [ ] Update `API_URL` to production backend
- [ ] Update Google OAuth authorized origins
- [ ] Update Google OAuth redirect URIs
- [ ] Test login flow
- [ ] Test API calls

## Browser Compatibility

Works in all modern browsers:
- Chrome 90+
- Firefox 88+
- Safari 14+
- Edge 90+

## Development Tips

### View in Browser DevTools

Press F12 to open:
- **Console** - See JavaScript logs
- **Network** - Monitor API calls
- **Application** - View localStorage
- **Elements** - Inspect HTML/CSS

### Test Without Backend

Comment out API calls and use mock data:

```javascript
// Mock data for testing
const mockBriefs = [
    {
        id: 1,
        brief_type: 'morning',
        title: 'Morning Brief - Dec 22',
        content: 'Test content...',
        read: false
    }
];
```

### Debug Authentication

Check console for:
- Google Sign-In load errors
- Credential response
- API call responses

## Security Notes

**Never commit:**
- Google Client Secret (backend only)
- API keys
- User data

**Client ID is safe to expose** (it's meant to be public in frontend).

## Performance

Page load:
- HTML: ~20KB
- CSS: ~5KB (embedded)
- JS: ~8KB (embedded)
- Google Sign-In: ~100KB
- **Total**: ~133KB

Fast load times, no external dependencies!

## Troubleshooting

**Google Sign-In not loading:**
- Check Client ID is correct
- Check internet connection
- Check browser console for errors

**Can't connect to backend:**
- Verify backend is running
- Check `API_URL` is correct
- Check CORS settings in backend

**Data not updating:**
- Check browser console for errors
- Verify user is logged in
- Check API responses in Network tab

**Login fails:**
- Verify Google OAuth is configured correctly
- Check authorized origins include your URL
- Check Client ID matches backend

## License

MIT
