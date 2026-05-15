Pakida Downstairs Display · v2 (Portrait)
Vertical-mounted display. Two HTML files on GitHub Pages, Firebase as the data layer, your phone as the remote.
Files
File
Where
What
pakida-display.html
On the TV (full-screen browser)
Auto-rotates 7 scenes
pakida-display-admin.html
On your phone
PIN 7364 · edit content, toggle scenes
Both files talk to pakida-f94f3 Firebase Realtime DB at the /display path. Changes go live in 2-3 seconds.
The 7 scenes (in rotation order)
Live Status — huge member count + tables grid
Tonight's Event — kicker + headline + time + tag
Eligibility Terminal — roast hook pointing to the kiosk
Hours & Pricing — Open Daily · 12 PM – 12 AM · ₹100/hr · ...
Member Spotlight — avatar + name + quote
QR / Apply — big QR + headline + subline
Reel — 9:16 video (optional, only shows if URL is set)
Disable any scene via toggle in the admin → it's skipped, no gap.
Portrait mounting — 3 ways to handle TV rotation
The Thomson 32PATH0111BL outputs at 1920×1080 landscape over HDMI. When mounted vertically, you need the content rendered rotated. Pick whichever works:
Path A — Android TV native rotation (best if it works)
TV Settings → Display → Screen Rotation → 90° (menu name varies)
Open the display URL in the browser → it now renders natively at 1080×1920
Done
Some Thomson Android TVs hide this in Settings → Device Preferences → Display. If you can't find it, skip to Path B.
Path B — Fully Kiosk Browser (recommended fallback)
Install Fully Kiosk Browser from Play Store. In its settings:
Web Content Settings → Rotate Browser Display → 90°
Set start URL to your display URL
Enable: Keep screen on, Auto-restart on reboot, Hide system bars
This rotates just the browser content, even if Android TV won't.
Path C — URL flag (last resort, works on any browser)
Append ?rotate=90 to the display URL:
Code
The page rotates itself via CSS. Works on every browser, including the TV's built-in one. Slight downside: text crispness can vary on cheap TVs because the GPU is doing the rotation instead of the panel.
Test order: Try A first. If no rotation option, try B. If you can't install Fully Kiosk, use C.
First setup (10 minutes)
Drop both files into your GitHub Pages repo.
Firebase rules — make sure /display allows public read/write:
Json
On the TV:
Connect to WiFi
Open browser → go to your display URL
Set fullscreen + rotation per the section above
Plug into the sign board's power so it powers on with the rest
On your phone:
Open the admin URL
Enter PIN 7364
Fill in: tonight's event, member spotlight, current member count
Hit save on each
Verify: edit anything in admin → TV updates within 3 seconds.
Daily ops (under 60 seconds)
When
What
Morning
Update Tonight's Event headline + time
Throughout
Update Live Status members + tables occupied
Weekly
Rotate Member Spotlight
As needed
Toggle Reel on/off when you have a new one to push
Auto-updating live status from ESP32 (when ready)
Your ESP32 already pushes café status to Firebase. To wire it into the display, just PATCH /display/status:
Code
Display picks it up within 3 seconds. Admin will reflect the new values next time you open it.
Member Spotlight photo
Easiest path: host the photo on Imgur or Cloudinary, paste the direct URL. The display falls back to the initial letter if no URL — looks intentional, no broken image.
Reel videos
Drop the URL of any 9:16 MP4 hosted somewhere with CORS allowed. Options:
GitHub raw URL (https://raw.githubusercontent.com/.../reel.mp4)
Cloudinary free tier
Firebase Storage (you already have a project)
Aspect ratio matters: 9:16 vertical fills the screen perfectly. 16:9 horizontal will letterbox.
Keyboard / remote shortcuts (on TV)
Right arrow → next scene immediately
Left arrow → previous scene
Click / OK → enter fullscreen mode (once per browser session)
Troubleshooting
Symptom
Fix
Red dot top-right on TV
Firebase rules denying read, or WiFi off
Admin says "Failed · Retry"
Firebase write rules; check /display allows write
TV stuck in landscape
Use rotation Path B or C above
Page goes dark after a while
TV sleep mode — disable in TV settings or use Fully Kiosk's "keep screen on"
QR code missing
api.qrserver.com blocked on TV's network. Tell me, I'll swap to a local-generated QR
Members count doesn't update
Currently manual via admin. ESP32 wiring is one PATCH call (above)
What's NOT in this v2 (intentional, ask if you want any)
Malayalam scenes (removed)
ESP32-direct hosting (parked — needs your laptop + board)
Photo upload (use external host URLs for now)
Write-token gate on Firebase (currently anyone with the URL can PATCH — fine if you don't post the URL publicly)
Multiple device sync indicator (single source of truth via Firebase, doesn't need it)
Tell me if any of these should come back.
