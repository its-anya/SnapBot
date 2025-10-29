# SnapBot 🤖📸

Maintaining Snapchat streaks can be fun but also a bit of a hassle, especially if you’re lazy (like me 😅). So I decided to automate it!

Welcome to SnapBot automatically sending snaps and keeping your streaks alive without lifting a finger.

**Automatic Snapchat Streak Keeper**

SnapBot automatically sends daily snaps to your friends to keep your Snapchat streaks alive without lifting a finger! It adds the date to your images and sends them at a scheduled time every day.

## ⚠️ Important Warning

**Automating Snapchat interactions may violate their Terms of Service.** Your account could be:
- Temporarily locked
- Permanently banned
- Required to verify identity

**Use at your own risk!** This tool is for educational purposes only.

## Features

- ✅ Automatic daily snap sending
- ✅ Date overlay on images
- ✅ Configurable recipients list
- ✅ Scheduled sending (e.g., 9:00 AM daily)
- ✅ Session-based authentication (no hardcoded passwords)
- ✅ Easy deployment to PythonAnywhere
- ✅ Comprehensive logging
- ✅ Timezone support

## Project Structure

```
Snapbot/
├── config.json              # Main configuration
├── run.py                   # Main application
├── session_manager.py       # Session handling
├── image_processor.py       # Image processing with date overlay
├── snap_sender.py           # Snap sending logic
├── session_exporter.py      # Tool to create session file
├── setup_folders.py         # Helper to create folder structure
├── requirements.txt         # Python dependencies
├── .gitignore              # Git ignore rules
├── README.md               # This file
├── pythonanywhere_setup.md # Deployment guide
├── images/                 # Your images organized by date
│   └── YYYY/
│       └── MM/
│           └── DD/
│               └── image.jpg
├── logs/                   # Application logs (auto-created)
└── temp/                   # Temporary files (auto-created)
```

## Quick Start Guide

### Step 1: Setup on Windows

1. **Clone or download this project**
   ```powershell
   cd "C:\Users\rehan\Pictures\Saved Pictures\Snapbot"
   ```

2. **Install Python dependencies**
   ```powershell
   pip install -r requirements.txt
   ```

3. **Create folder structure for images**
   ```powershell
   python setup_folders.py
   ```
   
   This creates folders like:
   - `images/2025/10/29/`
   - `images/2025/10/30/`
   - etc.

4. **Add your images**
   
   Place one or more images in each date folder:
   ```
   images/2025/10/29/snap1.jpg
   images/2025/10/30/snap2.jpg
   ```

5. **Configure recipients**
   
   Edit `config.json` and add Snapchat usernames:
   ```json
   {
     "recipients": [
       "friend1_username",
       "friend2_username",
       "friend3_username"
     ]
   }
   ```

### Step 2: Create Session File

Run the session exporter:
```powershell
python session_exporter.py
```

Choose option 1 (Manual Entry) and follow instructions:

1. Open Chrome/Edge/Firefox
2. Go to https://web.snapchat.com and log in
3. Press F12 to open DevTools
4. Go to Application → Cookies → https://web.snapchat.com
5. Find the `sc-a` cookie and copy its value
6. Paste it into the session exporter

This creates `snapchat_session.json` with your authentication data.

**IMPORTANT:** Keep this file secure! Never commit it to Git.

### Step 3: Test Locally

1. **Verify setup**
   ```powershell
   python run.py --verify
   ```

2. **Test sending once**
   ```powershell
   python run.py --once
   ```

3. **Check logs**
   ```powershell
   type logs\snapbot_20251029.log
   ```

### Step 4: Deploy to PythonAnywhere

See detailed instructions in [pythonanywhere_setup.md](pythonanywhere_setup.md)

Quick summary:
1. Create PythonAnywhere account
2. Upload files (via Git or Files tab)
3. Upload `snapchat_session.json` securely
4. Install dependencies
5. Schedule daily task

## Configuration

### config.json

```json
{
  "recipients": [
    "username1",
    "username2"
  ],
  "schedule": {
    "send_time": "09:00",
    "timezone": "Asia/Kolkata"
  },
  "paths": {
    "images_folder": "images",
    "session_file": "snapchat_session.json",
    "logs_folder": "logs"
  },
  "image_settings": {
    "date_format": "%Y-%m-%d",
    "text_position": "bottom_right",
    "text_color": [255, 255, 255],
    "text_size": 40,
    "font_style": "bold"
  },
  "snapchat": {
    "retry_attempts": 3,
    "retry_delay_seconds": 5,
    "request_delay_seconds": 2
  }
}
```

### Configuration Options

| Option | Description | Example |
|--------|-------------|---------|
| `recipients` | List of Snapchat usernames | `["user1", "user2"]` |
| `send_time` | Time to send snaps (24-hour format) | `"09:00"` |
| `timezone` | Timezone for scheduling | `"Asia/Kolkata"` |
| `images_folder` | Base folder for images | `"images"` |
| `session_file` | Path to session file | `"snapchat_session.json"` |
| `date_format` | Date format for overlay | `"%Y-%m-%d"` |
| `text_position` | Position of date text | `"bottom_right"` |
| `text_color` | RGB color for text | `[255, 255, 255]` |
| `text_size` | Font size | `40` |

### Text Positions

- `bottom_right` (default)
- `bottom_left`
- `top_right`
- `top_left`

## Usage

### Run Once (Testing)

```bash
python run.py --once
```

Sends snaps immediately (useful for testing).

### Run on Schedule

```bash
python run.py --schedule
```

Runs continuously and sends snaps at the configured time daily.

### Verify Setup

```bash
python run.py --verify
```

Checks if everything is configured correctly.

### Custom Config File

```bash
python run.py --config custom.json --once
```

## Image Organization

Images must be organized by date:

```
images/
├── 2025/
│   ├── 10/
│   │   ├── 29/
│   │   │   └── image.jpg
│   │   ├── 30/
│   │   │   └── image.jpg
│   │   └── 31/
│   │       └── image.jpg
│   └── 11/
│       └── 01/
│           └── image.jpg
```

**Format:** `images/YYYY/MM/DD/image.jpg`

The bot will:
1. Find the image for today's date
2. Add the date as text overlay
3. Send it to all recipients

## Session Management

### Creating a Session File

The session file stores your Snapchat authentication data so you don't need to log in every time.

**Method 1: Browser DevTools (Recommended)**

1. Log in to Snapchat on web.snapchat.com
2. Open DevTools (F12)
3. Go to Application → Cookies
4. Copy the `sc-a` cookie value
5. Run `python session_exporter.py` and paste it

**Method 2: Programmatic (Not Recommended)**

Not implemented due to Snapchat TOS violations. Use browser method instead.

### Session File Format

```json
{
  "auth_token": "your_auth_token_here",
  "cookies": [
    {
      "name": "sc-a",
      "value": "your_token",
      "domain": ".snapchat.com",
      "path": "/"
    }
  ],
  "created_at": "2025-10-29",
  "notes": "Session created manually"
}
```

### Session Security

- ✅ Never commit session file to Git (it's in .gitignore)
- ✅ Use `chmod 600` on PythonAnywhere
- ✅ Don't share your session file
- ✅ Regenerate if compromised
- ✅ Sessions expire after some time (re-export when needed)

## PythonAnywhere Deployment

### Free Tier Limitations

- One scheduled task only
- Limited CPU seconds per day
- Console may timeout after inactivity

### Scheduled Task Setup

1. Go to **Tasks** tab
2. Add new scheduled task
3. Time: `09:00` (UTC)
4. Command:
   ```bash
   /home/yourusername/snapbot/venv/bin/python /home/yourusername/snapbot/run.py --once
   ```

### Timezone Configuration

PythonAnywhere uses UTC. Set your timezone in `config.json`:

```json
{
  "schedule": {
    "send_time": "09:00",
    "timezone": "Asia/Kolkata"
  }
}
```

The bot will convert to your local time.

## Troubleshooting

### Session Expired

**Symptoms:** Snaps not sending, authentication errors

**Solution:** 
1. Run `python session_exporter.py` on Windows
2. Export fresh session file
3. Upload to PythonAnywhere

### Images Not Found

**Symptoms:** "No image found for date" error

**Solution:**
1. Check folder structure: `images/YYYY/MM/DD/`
2. Ensure images are JPG or PNG
3. Run `python setup_folders.py` to create folders

### Permission Errors (PythonAnywhere)

```bash
chmod 600 snapchat_session.json
chmod 644 *.py config.json
```

### Timezone Issues

Check your timezone setting in `config.json`. Use standard timezone names like:
- `Asia/Kolkata`
- `America/New_York`
- `Europe/London`

Full list: https://en.wikipedia.org/wiki/List_of_tz_database_time_zones

### Logging

Check logs for detailed error messages:

```bash
# Windows
type logs\snapbot_20251029.log

# Linux/PythonAnywhere
cat logs/snapbot_20251029.log
```

## Important Notes

### Snapchat API

This bot uses **unofficial** Snapchat API methods. The actual implementation in `snap_sender.py` contains placeholder code. You need to:

1. Research unofficial Snapchat API libraries (e.g., `snapchat-api`, `pysnap2`)
2. Implement actual sending logic in `snap_sender._send_to_user()`
3. Handle Snapchat's authentication and media upload

**Why placeholders?** 
- Unofficial APIs violate Snapchat TOS
- Libraries frequently break when Snapchat updates
- Account ban risk is high

### Rate Limiting

The bot includes delays between requests to avoid triggering Snapchat's anti-abuse systems:

- `retry_delay_seconds`: Delay between retry attempts
- `request_delay_seconds`: Delay between sending to different users

Adjust these in `config.json` if needed.

### Account Safety

To minimize ban risk:
- Don't send to too many people
- Keep delays reasonable
- Don't run 24/7 from multiple IPs
- Use your real device occasionally
- Don't use on important accounts

## Development

### Project Structure

```
session_manager.py    → Loads/saves session data
image_processor.py    → Handles images and date overlay
snap_sender.py        → Sends snaps (needs implementation)
run.py               → Main app with scheduling
session_exporter.py  → Creates session files
```

### Implementing Snap Sending

Edit `snap_sender.py` and implement `_send_to_user()`:

```python
def _send_to_user(self, image_path: Path, username: str) -> bool:
    # Your implementation using unofficial API
    with open(image_path, 'rb') as f:
        image_data = f.read()
    
    # Upload image
    # Send to user
    # Return True if successful
    
    return success
```

### Testing

```bash
# Verify setup
python run.py --verify

# Test once
python run.py --once

# Check logs
cat logs/snapbot_*.log
```

## FAQ

**Q: Will this get my account banned?**  
A: Possibly. Automating Snapchat violates their Terms of Service.

**Q: Do I need a paid PythonAnywhere account?**  
A: No, free tier works. But you only get one scheduled task.

**Q: Can I send different images to different people?**  
A: Not currently, but you could modify the code to support this.

**Q: How do I update images?**  
A: Just add new images to the date folders. The bot picks them up automatically.

**Q: What happens if there's no image for today?**  
A: The bot logs an error and doesn't send anything.

**Q: Can I run this on Android/iOS?**  
A: Not directly. Deploy to PythonAnywhere instead.

**Q: Session file expired, what now?**  
A: Re-run `session_exporter.py` on Windows and upload the new file.

## License

This project is for educational purposes only. Use at your own risk.

## Disclaimer

This tool is not affiliated with, endorsed by, or connected to Snapchat Inc. in any way. Using this tool may violate Snapchat's Terms of Service and could result in your account being permanently banned. The authors are not responsible for any consequences resulting from the use of this software.

## Contributing

This is a personal project. Fork it and modify as needed!

## Support

Check logs first:
```bash
cat logs/snapbot_$(date +%Y%m%d).log
```

Common issues:
- Session expired → Re-export
- Images not found → Check folder structure
- Timezone wrong → Update config.json

---

**Created with ❤️ for lazy streak keepers everywhere!**
