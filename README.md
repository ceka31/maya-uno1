# 🎮 3D UNO - Interactive Card Game

A beautiful, fully-featured 3D UNO card game built with Node.js, Express, Socket.IO, and Three.js!

## ✨ Features

### 🎨 Visual Effects
- **3D Table with Model** - 63MB GLB model loading with progress bar
- **3D Card Rendering** - Beautifully rendered cards on the table with shadows
- **Particle Explosions** - Dynamic particle effects for card plays
- **Smooth Animations** - Card animations, rotations, and smooth transitions
- **Glowing Effects** - Interactive glow animations on card hover

### 🔊 Sound Effects
- **Card Play Sounds** - Satisfying beeps when playing cards
- **UNO Call Sound** - Distinctive sound for calling UNO
- **Win Fanfare** - Victory music sequence
- **Draw Sound** - Audio feedback when drawing cards
- **Web Audio API** - Pure JavaScript audio generation (no external files needed)

### 🎯 Game Features
- **Multiplayer Support** - Real-time multiplayer via WebSockets
- **Full UNO Rules** - Complete implementation of UNO game logic
- **Player Management** - Join rooms, view player list and scores
- **Hand Display** - Easy-to-use card hand UI with drag-and-select
- **Wild Card System** - Choose colors when playing wild cards
- **Turn System** - Automatic turn management with skip and reverse
- **Draw Stack** - Handles draw2 and draw4 cumulative effects
- **Chat System** - In-game chat for communication

### 🎲 Game Mechanics
- Proper turn order management
- Color matching and value matching
- Special cards: Skip, Reverse, Draw2, Wild, Draw4
- UNO calling system
- Score tracking across rounds
- Game reset and new round system

## 🚀 Getting Started

### Installation

```bash
# Install dependencies
npm install

# Start the server
npm start
```

The game will be available at `http://localhost:3000`

### Development

For development with auto-restart:

```bash
npm run dev
```

## 📋 Requirements

- Node.js 14+
- Modern web browser with WebGL support
- Internet connection (for model loading)

## 🎮 How to Play

1. **Join a Game**
   - Enter your name and room ID
   - Wait for other players to join

2. **Start Game**
   - Click "Start Game" when 2+ players are ready
   - Each player gets 7 cards

3. **Play Cards**
   - Click a card in your hand to play it
   - Match color or number with the top card
   - For wild cards, choose a color

4. **Draw Cards**
   - Click "DRAW" button if you can't play
   - Draw additional cards as needed

5. **Call UNO**
   - Click "UNO!" button when you have 1 card left
   - Alert other players you're close to winning!

## 🛠️ Technology Stack

### Backend
- **Express.js** - Web server framework
- **Socket.IO** - Real-time multiplayer communication
- **Node.js** - JavaScript runtime

### Frontend
- **Three.js** - 3D graphics and rendering
- **HTML5 Canvas** - Card texture generation
- **Web Audio API** - Sound effects
- **Vanilla JavaScript** - Game logic and UI

## 📁 Project Structure

```
.
├── server.js              # Main server file with game logic
├── package.json          # Dependencies and scripts
└── public/
    └── index.html        # Main game client (HTML + CSS + JS)
```

## 🎓 Features Explained

### 3D Rendering
- **Three.js** handles all 3D rendering
- Cards are created with custom textures
- Model loads with progress tracking
- Shadows and lighting for depth

### Sound System
- Web Audio API oscillators create pure tones
- No external sound files needed
- Multiple sound sequences for different actions
- Smooth audio transitions

### Particle System
- Position-based particle generation
- Physics simulation with gravity
- Life-time based particle removal
- Color customization per action

### Game State
- Server maintains authoritative game state
- Clients emit actions via WebSocket
- Real-time updates broadcast to all players
- Proper turn and card validation

## ⚙️ Configuration

### Modify Game Settings

Edit `server.js` to customize:
- `COLORS` - Available card colors
- `VALUES` - Card values and special cards
- `AVATAR_COLORS` - Player avatar color palette
- `PORT` - Server port (default: 3000)

## 🐛 Troubleshooting

### Model Not Loading
- Check internet connection
- Verify CDN is accessible
- Check browser console for errors

### Sound Not Working
- Enable audio in browser settings
- Check system volume
- Ensure AudioContext is allowed

### Multiplayer Issues
- Verify both players are in same room
- Check browser console for connection errors
- Ensure WebSocket connection is stable

## 📊 Game Rules

### Basic Rules
- Match card color or number with previous card
- Play special cards for effects
- Draw when no valid cards available

### Special Cards
- **Skip** - Skip next player's turn
- **Reverse** - Reverse play direction
- **Draw2** - Next player draws 2 cards
- **Wild** - Choose any color to continue
- **Draw4** - Wild card that makes next player draw 4

### Winning
- First player to play all cards wins
- Other players score points from remaining cards
- New round can be started for multiple games

## 📝 License

MIT

## 🤝 Contributing

Feel free to fork, modify, and improve!

---

**Enjoy the game! 🎉**
