# VR Reading Pod – AR Book Showcase
### Interactive Web AR | MindAR + Three.js

---

## Project Overview

When a user scans the printed **robot image** placed at the VR Reading Pod stall, their phone camera detects the image target and instantly renders **5 animated 3D book cards** floating in a circular orbit around the robot — each tappable to reveal title, author, and summary.

---

## File Structure

```
vr-reading-pod/
├── index.html          ← Complete single-page AR app
├── assets/
│   ├── targets.mind    ← ⚠️  YOU MUST GENERATE THIS (instructions below)
│   └── robot.jpg       ← Your robot target image
└── README.md           ← This file
```

---

## STEP 1 — Generate the `.mind` Target File

The `.mind` file is MindAR's compiled image descriptor. You generate it **once** from your robot image.

### Option A — MindAR Online Compiler (Easiest ✅)

1. Visit: **https://hiukim.github.io/mind-ar-js-doc/tools/compile**
2. Click **"Upload Images"** → select your `robot.jpg`
3. Wait for compilation (30–60 seconds)
4. Click **"Export"** → saves `targets.mind`
5. Place `targets.mind` inside the `assets/` folder

### Option B — Node.js CLI

```bash
npm install -g mindar-compiler

mindar-compiler compile \
  --inputFiles robot.jpg \
  --output assets/targets.mind
```

### Tips for best tracking
- Use an image with **high contrast** and **rich texture**
- Avoid solid color backgrounds or very symmetrical images
- Recommended size: **min 800×600px**, printed at A5 or larger
- Print on **matte paper** (avoid glossy reflections)

---

## STEP 2 — Test Locally (HTTPS required for camera)

WebAR requires HTTPS. Use one of these local servers:

### Option A — VS Code Live Server
Install the **Live Server** extension → Right-click `index.html` → "Open with Live Server"

### Option B — npx serve (recommended)
```bash
npx serve . --ssl-cert localhost.pem --ssl-key localhost-key.pem
```

Or for quick testing with HTTP (Chrome only with flag):
```bash
npx http-server . -p 8080
# Then open: http://localhost:8080
```

### Option C — Python
```bash
python3 -m http.server 8080
# Then open: http://localhost:8080
```

> ⚠️ **Camera access requires HTTPS** on Android Chrome. Use ngrok or GitHub Pages for real device testing.

### ngrok tunnel (real device testing)
```bash
npm install -g ngrok
ngrok http 8080
# Copy the https:// URL and open it on your phone
```

---

## STEP 3 — Deploy to GitHub Pages

1. **Create a GitHub repository** (e.g. `vr-reading-pod`)

2. **Push your files:**
```bash
git init
git add .
git commit -m "Initial AR app"
git remote add origin https://github.com/YOUR_USERNAME/vr-reading-pod.git
git push -u origin main
```

3. **Enable GitHub Pages:**
   - Go to repo → **Settings** → **Pages**
   - Source: **Deploy from branch** → `main` → `/ (root)`
   - Click **Save**

4. **Your app is live at:**
   ```
   https://YOUR_USERNAME.github.io/vr-reading-pod/
   ```

5. **Generate a QR code** pointing to that URL:
   - Use https://qr.io or https://qrcode-monkey.com
   - Print and place it near the robot at the stall

---

## STEP 4 — Print the Robot Image

- Print your robot image at **A4 or A5 size** on matte paper
- Laminate if possible (protects from reflections & wear)
- Place flat on the table/robot at the stall
- Ensure good lighting — avoid harsh shadows over the image

---

## How It Works (Technical)

| Layer | Technology | Role |
|-------|-----------|------|
| Image Tracking | MindAR 1.2.5 | Detects robot image, provides 6DOF pose |
| 3D Rendering | Three.js r150 | Renders book cards in AR space |
| Card Graphics | HTML Canvas API | Generates per-book textures with title/author |
| Interaction | Raycaster | Detects tap on floating book cards |
| UI | Pure CSS | Loading screen, info panel, HUD |

### AR World Scale Reference
- `RADIUS = 0.28` → book orbit radius (~28cm from center in real space)
- `CARD_W = 0.22` → book card width (~22cm)
- Adjust these values in `index.html` to scale for your printed image size

---

## Customization

### Add/Remove Books
Edit the `BOOKS` array in `index.html`:
```js
const BOOKS = [
  {
    title: "Your Book Title",
    author: "Author Name",
    summary: "One-line description.",
    color: "#00ffe7"   // Accent color for this card
  },
  // ...
];
```

### Change Orbit Radius
```js
const RADIUS = 0.28;  // Increase for wider spread
```

### Change Rotation Speed
```js
ringGroup.rotation.y = t * 0.22;  // Increase for faster spin
```

### Change Card Size
```js
const CARD_W = 0.22;
const CARD_H = 0.11;
```

---

## Browser Compatibility

| Browser | Support |
|---------|---------|
| Android Chrome | ✅ Full support |
| Android Samsung Internet | ✅ Supported |
| iOS Safari (15+) | ✅ Supported |
| Desktop Chrome | ✅ (for development) |
| Firefox Android | ⚠️ Partial |

> **Best experience:** Android Chrome or iOS Safari with good lighting

---

## Troubleshooting

| Issue | Fix |
|-------|-----|
| Black screen / no camera | Ensure HTTPS, grant camera permission |
| Target not detecting | Better lighting, print larger, use more textured image |
| Books not appearing | Check `targets.mind` is in `assets/` folder |
| 404 on targets.mind | Ensure exact filename match in HTML (`assets/targets.mind`) |
| Slow performance | Close other tabs, ensure good device specs (2019+ Android) |

---

## Performance Tips

- Keep `targets.mind` under 2MB (use a single, sharp image)
- Serve files with gzip compression (GitHub Pages does this automatically)
- Use Wi-Fi for first load; after caching it works offline

---

*Built with MindAR Image Tracking + Three.js | No backend required*
