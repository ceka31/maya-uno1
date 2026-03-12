# 🎨 Customization Guide

Customize the 3D UNO game to match your preferences!

## 🎨 Visual Customization

### Change Card Colors

Edit in `public/index.html` - look for the `colors` object in `create3DCard()`:

```javascript
const colors = {
    red: 0xff3b3b,      // Change red to any hex color
    yellow: 0xffd600,   // Change yellow
    green: 0x00c853,    // Change green
    blue: 0x2979ff,     // Change blue
    wild: 0xa64dff      // Change wild card color
};
```

**Example:** Change red to orange:
```javascript
red: 0xffa500,  // Orange
```

### Modify Table Colors

Edit in `public/index.html` CSS variables:

```css
:root {
    --bg: #0a0008;        /* Main background */
    --bgcard: #120010;    /* Card background */
    --text: #f0e8ff;      /* Text color */
    --muted: #6a5a7a;     /* Muted text */
    --red: #ff3b3b;       /* Red card color */
    --yellow: #ffd600;    /* Yellow card color */
    --green: #00c853;     /* Green card color */
    --blue: #2979ff;      /* Blue card color */
}
```

### Change Background Gradient

In `initScene()`:

```javascript
scene.background = new THREE.Color(0x0a0008);  // Change to any hex color
scene.fog = new THREE.Fog(0x0a0008, 2, 8);    // Match with background
```

## 🔊 Sound Customization

### Create Custom Sounds

Modify sound frequencies in `public/index.html`:

```javascript
function playCardSound() {
    playSoundSequence([
        { freq: 400, dur: 0.1, delay: 0 },     // First beep frequency
        { freq: 500, dur: 0.1, delay: 50 }     // Second beep frequency
    ]);
}
```

**Frequency Guide:**
- 200-300 Hz = Low bass sounds
- 300-500 Hz = Mid-range tones
- 500-1000 Hz = High tones
- 1000+ Hz = Very high pitched

### Create New Sound Sequences

```javascript
function playCustomSound() {
    playSoundSequence([
        { freq: 523, dur: 0.1, delay: 0 },     // Note 1
        { freq: 659, dur: 0.1, delay: 100 },   // Note 2
        { freq: 784, dur: 0.2, delay: 200 },   // Note 3
    ]);
}
```

### Disable Sounds

Comment out sound calls in game logic functions:

```javascript
// playCardSound();  // Disabled
// createExplosion(...);  // Disabled
```

## 🎮 Game Customization

### Change Starting Hand Size

In `server.js`, find `dealGame()`:

```javascript
for (let i = 0; i < 7; i++) {  // Change 7 to your desired number
    room.players[pid].hand.push(room.deck.pop());
}
```

### Modify Card Values

In `server.js`, find `buildDeck()`:

```javascript
const VALUES = ['0', '1', '2', '3', '4', '5', '6', '7', '8', '9', 'skip', 'reverse', 'draw2'];
// Add custom values here
```

### Change Number of Decks

Modify in `buildDeck()`:

```javascript
for (let i = 0; i < 4; i++) deck.push({ ... });  // Change 4 to 1 for single deck
```

### Adjust Point Scoring

In `server.js`, find `calcScore()`:

```javascript
if (c.type === 'wild') pts += 50;           // Change wild card points
else if (['skip', 'reverse', 'draw2'].includes(c.value)) pts += 20;  // Change special card points
else pts += parseInt(c.value) || 0;        // Change number card points
```

## 👥 Player Customization

### Change Avatar Colors

In `server.js`:

```javascript
const AVATAR_COLORS = [
    '#e74c3c', '#e67e22', '#f1c40f', '#2ecc71',
    '#1abc9c', '#3498db', '#9b59b6', '#e91e63'
];
// Add or change colors as needed
```

### Customize Player Names

In `public/index.html`, modify the initialization:

```javascript
gameState.playerName = 'MyCustomName_' + gameState.playerId.slice(-4);
```

## 🎬 Animation Customization

### Speed Up Card Animations

In `animateCardPlay()`:

```javascript
progress += 0.05;  // Change 0.05 to 0.1 for faster, or 0.02 for slower
```

### Change Table Rotation Speed

In `animate()`:

```javascript
table.rotation.y += 0.0005;  // Increase for faster spin, decrease for slower
```

### Adjust Card Bobbing

In `animate()`:

```javascript
card.position.y = card.baseY + Math.sin(Date.now() * 0.001 + i * 0.3) * 0.05;
//                                                              ^              ^^^
//                                                            speed         height
```

## 💫 Particle Effects Customization

### Change Particle Count

In particle creation functions:

```javascript
function createExplosion(position, color = 0xffd600) {
    for (let i = 0; i < 8; i++) {  // Change 8 to more/fewer particles
        // ...
    }
}
```

### Adjust Particle Speed

In `createParticle()`:

```javascript
particle.velocity = velocity.clone();  // Current velocity
// Modify the velocity passed in createExplosion()
```

### Particle Colors

Call with different colors:

```javascript
createExplosion(position, 0xff0000);  // Red
createExplosion(position, 0x00ff00);  // Green
createExplosion(position, 0x0000ff);  // Blue
```

## 🖼️ UI Customization

### Change Button Styles

Edit CSS in `public/index.html`:

```css
.btn-draw {
    background: linear-gradient(135deg, #2979ff, #64b5f6);  /* Change gradient */
    color: #fff;  /* Change text color */
}

.btn-uno {
    background: linear-gradient(135deg, #ff3b3b, #ff6b6b);  /* Change gradient */
}
```

### Modify Card Size in Hand

```css
.uno-card {
    width: 70px;    /* Change card width */
    height: 100px;  /* Change card height */
}
```

### Change Font

In `<head>`:

```html
<link href="https://fonts.googleapis.com/css2?family=YourFont:wght@400;600;700;800;900&display=swap" rel="stylesheet">
```

Update CSS variable:
```css
html, body { font-family: 'YourFont', sans-serif; }
```

## 🌐 Server Customization

### Change Default Port

In `server.js`:

```javascript
const PORT = process.env.PORT || 3000;  // Change 3000 to your port
```

### Modify CORS Settings

```javascript
const io = new Server(server, { cors: { origin: '*' } });
// Change '*' to specific origin for security
```

### Custom Room Names

Add in `server.js`:

```javascript
const GAME_MODES = ['Classic', 'Speed', 'Chaos'];
// Implement room-based game modes
```

## 📱 Responsive Design

### Adjust Mobile Layout

Edit CSS media queries in `public/index.html`:

```css
@media (max-width: 768px) {
    .uno-card {
        width: 60px;   /* Smaller for mobile */
        height: 90px;
    }
    
    #rightSidebar {
        width: 100%;   /* Full width */
    }
}
```

## 🔐 Security Customization

### Enable HTTPS

In `server.js`:

```javascript
const https = require('https');
const fs = require('fs');

const options = {
    key: fs.readFileSync('path/to/key.pem'),
    cert: fs.readFileSync('path/to/cert.pem')
};

const server = https.createServer(options, app);
```

### Add Authentication

Implement user login before game join in `server.js`:

```javascript
socket.on('join', ({ name, roomId, token }) => {
    // Validate token
    // Then proceed with join
});
```

## 📊 Performance Optimization

### Reduce Particle Count for Low-End Devices

```javascript
function createExplosion(position, color) {
    const particleCount = window.devicePixelRatio < 2 ? 4 : 8;
    for (let i = 0; i < particleCount; i++) {
        // ...
    }
}
```

### Lower Rendering Quality

In `initScene()`:

```javascript
renderer.setSize(canvasContainer.clientWidth, canvasContainer.clientHeight);
renderer.setPixelRatio(window.devicePixelRatio * 0.5);  // Lower resolution
```

---

**Need help? Check README.md for more information!**
