# Console RPG — Text + Tactical Web Battle

This repo haveteo games:
- **Text adventure with AI** (Flask app at app.py) — a room/DM flow powered by Gemini.
- **Tactical web battle** (Flask app at webui.py) — a hex-grid combat UI. Combat should be triggered from the text game;

Maintenance notice: development is paused for the near future; expect limited updates.

## Requirements
- Python 3.12+
- Google Gemini API key with a model that has quota (set via environment)

## Environment
Create a .env in the project root:
```
GEMINI_API_KEY=your_key_here
```

## Structure

   ```
   Console_RPG/
   ├── app.py                  # Text adventure server (Flask, port 8000)
   ├── webui.py                # Tactical battle UI server (Flask, port 5000)
   ├── DEF.py                  # Main D&D game logic and DM AI integration
   ├── gemini.py               # Gemini API wrapper with key rotation and retry logic
   ├── gemini_schema.py        # Pydantic models and JSON schemas for AI responses
   ├── room_manager.py         # Room state management for multiplayer sessions
   ├── config.py               # Game configuration (battlefield, player, enemies, rules)
   ├── battlefield_configs.py  # Predefined battle arenas and terrain configs
   ├── character_config.py     # Race/class stats, bonuses, and ability calculations
   ├── dnd_spells.py           # Spell definitions (level 1, 2, basic attacks)
   ├── prompts.py              # System prompts for the AI DM in multiple languages
   ├── translations.py         # Translation loading utilities
   ├── .env                    # Environment variables (GEMINI_API_KEY, etc.)
   ├── .env.example            # Example environment file
   ├── requirements.txt        # Python dependencies
   ├── pyproject.toml          # Project metadata and uv config
   ├── static/
   │   ├── css/
   │   │   ├── battle.css      # Tactical battle UI styles
   │   │   ├── character.css   # Character creation styles
   │   │   ├── dice.css        # Dice roll animations
   │   │   ├── game.css        # Text adventure UI styles
   │   │   ├── index.css       # Landing page styles
   │   │   ├── style.css       # Common styles
   │   │   └── styles.css      # Additional global styles
   │   └── js/
   │       ├── character.js    # Character creation logic
   │       ├── combat.js       # Combat system frontend
   │       ├── game.js         # Text adventure frontend
   │       ├── hexgrid.js      # Hex grid pathfinding and rendering
   │       ├── index.js        # Landing page logic
   │       ├── notifications.js # Toast notifications system
   │       ├── settings.js     # Settings panel
   │       ├── translations.js # Frontend translation utilities
   │       └── ui.js           # UI helper functions
   ├── templates/
   │   ├── base.html           # Base template with common layout
   │   ├── battle.html         # Tactical hex-grid battle view
   │   ├── character.html      # Character sheet view
   │   ├── create.html         # Character creation form
   │   ├── game.html           # Text adventure chat interface
   │   └── index.html          # Landing/room selection page
   ├── translations/
   │   ├── en.json             # English translations
   │   └── ru.json             # Russian translations
   ├── logs/                   # Application logs (auto-created)
   ├── saves/                  # Saved game files (auto-created)
   └── README.md               # This file
   ```

## Run servers
Text adventure (rooms, DM, combat handoff signal):
```bash
python app.py  # runs on http://localhost:8000
```

Tactical battle UI:
```bash
python webui.py  # runs on http://localhost:5000
```

## How the handoff works
- During text play, when combat starts, the server responds with a redirect flag and enemy info. The frontend should navigate the player to the battle UI and seed the enemy (name, HP).
- After the tactical fight, the battle UI should POST the result back to the text server so the narrative can continue (enemy dead → story continues; player dead → offer character creation).

## Playing the text adventure
- Choose language, create/join a room, and interact with the DM via text.
- Save/load via provided endpoints (see app UI).

## Playing the tactical battle
- Starts when redirected from the text game or by visiting /battle in the battle app.
- Supports movement, attacks, spells, effects, and HP tracking for player/enemy.

## Known limitations
- Development is currently paused; features and fixes may be delayed and not surely will be continued.
- Gemini responses depend on your model/quota; ensure your key has access to the configured model.