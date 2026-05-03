# Data Visualizer — Klotski Puzzle + Physics State Graph

> 🏆 **[AWARD NAME]** — [HACKATHON NAME] ([YEAR])

A Klotski-style sliding block puzzle game with a twist: every board state is visualized in real time as a 3D physics-based graph, so you can *see* the entire solution space as you play. Includes an AI hint system powered by Google Gemini.

![Gameplay Screenshot](<!-- [ADD SCREENSHOT OR GIF HERE] -->)

---

## ✨ Features

- **Sliding Block Puzzle** — Classic Klotski rules on a 6×6 grid. Slide blocks to guide the Hero piece (ID `1`) to the exit.
- **Live State-Space Graph** — A BFS solver runs on load and builds a full adjacency map of every reachable board state. Each state is rendered as a node in a 3D physics simulation — edges connect states that differ by one move.
- **Real-Time Highlighting** — As you play, your current position in the state graph is highlighted live.
- **AI Hints via Gemini** — Press "Ask Gemini" to send the current board matrix to Gemini Flash. The model returns an optimal move sequence which is parsed, saved, and animated step-by-step on the board.
- **Auto-Solve Mode** — The board can automatically replay the BFS-computed winning path with smooth block slide animations.
- **4 Built-in Levels** — JSON-defined levels of increasing complexity. Easily add your own.
- **Custom Shaders** — GLSL shaders for the board, sliding blocks, and graph connection lines.
- **Level Export** — Export any board layout to JSON at runtime for level design and debugging.

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| Game Engine | [Godot 4.6](https://godotengine.org/) |
| Game Logic | C# (.NET) |
| Scene Scripting | GDScript |
| AI Integration | Google Gemini Flash API |
| Rendering | Forward Plus + Custom `.gdshader` shaders |
| Level Format | JSON |

---

## 🚀 Getting Started

### Prerequisites

- [Godot 4.6+](https://godotengine.org/download) with **.NET / C# support** enabled
- [.NET SDK 8+](https://dotnet.microsoft.com/download)
- A [Google Gemini API key](https://aistudio.google.com/app/apikey) *(only required for the AI hint feature)*

### Installation

```bash
# 1. Clone the repository
git clone [YOUR REPO URL]
cd [REPO FOLDER NAME]

# 2. Open the project in Godot 4.6
#    File → Open Project → select the project.godot file

# 3. Build the C# solution
#    In Godot: Build → Build Solution  (or Ctrl+B)

# 4. (Optional) Enable AI hints
#    Create a file named gemini_api_key.n in the project root
#    and paste your Gemini API key as plain text (no quotes)
echo "YOUR_API_KEY_HERE" > gemini_api_key.n
```

> ⚠️ `gemini_api_key.n` is listed in `.gitignore` — it will never be committed.

### Running

Press **F5** (or the Play button) in the Godot editor to launch the game.

---

## 🎮 How to Play

| Action | Input |
|---|---|
| Select a block | Click on it |
| Move selected block | Arrow keys or WASD |
| Ask Gemini for a hint | Click the **Ask Gemini** button |
| Replay a saved solution | Click a **Solution (1–4)** button |
| Export current board state | Right-click or `Export` keybind |

**Win condition:** Slide the green Hero block (ID `1`) so its right edge reaches the right-center exit of the board.

---

## 📁 Project Structure

```
├── Art/                   # Sprites and sprite sheets for blocks, board, buttons
├── Layouts/               # JSON level files and Gemini response cache
│   ├── level1.json        # Built-in levels (1–4)
│   ├── exported_level.json  # Runtime board export
│   └── GeminiResponse.json  # Cached AI move sequence
├── Board.cs               # Core puzzle logic, BFS solver, state management
├── KlotskiBlock.cs        # Individual block: movement, animation, highlighting
├── BottomBar.cs           # UI controls + Gemini API integration
├── BoardLayout.cs         # Level loading and grid parsing
├── autoloadSignals.gd     # Global signal bus (board ↔ graph communication)
├── multi_nodes.gd         # 3D physics graph: node spawning and layout
├── graph_node.gd          # Individual graph node behaviour
├── node_line.gd           # Edge/connection line rendering
├── cellshader.gdshader    # Board cell shader
├── nodeLine.gdshader      # Graph edge shader
└── project.godot          # Godot project configuration
```

---

## 🤖 Gemini AI Integration

When you press **Ask Gemini**, the app:

1. Serializes the current 6×6 board as a JSON matrix
2. Sends it to `gemini-flash-latest` with a structured prompt describing Klotski rules and the win condition
3. Parses the returned move sequence (format: `Step N: Block 'X' moved DIRECTION to (col, row)`)
4. Saves the sequence to `Layouts/GeminiResponse.json`
5. Animates each step on the board in order

The API key is loaded at runtime from `gemini_api_key.n` (never hardcoded).

---

## 🗺️ Adding Custom Levels

Create a new JSON file in `Layouts/` following this format:

```json
{
  "grid": [
    [0, 0, 0, 0, 0, 0],
    [2, 2, 0, 4, 0, 0],
    [1, 1, 0, 4, 0, 0],
    [3, 3, 0, 0, 0, 0],
    [0, 0, 0, 0, 0, 0],
    [0, 0, 0, 0, 0, 0]
  ]
}
```

- `0` = empty cell
- `1` = Hero block (must be present, horizontal 1×2)
- `2`, `3`, `4` ... = other blocks (same integer = same block, shape inferred from connected cells)

Then point `Board.cs`'s `LevelPath` export variable to your new file in the Godot editor.

---

## 🧩 Known Limitations / Future Work

- [ ] [ADD ANY KNOWN BUGS OR LIMITATIONS]
- [ ] Mobile / touch input support
- [ ] Procedural level generation
- [ ] Move counter and leaderboard
- [ ] [ANYTHING ELSE YOUR TEAM WANTS TO ADD]

---

## 👥 Team

| Name | Role |
|---|---|
| [YOUR NAME] | [YOUR ROLE] |
| [TEAMMATE 2] | [ROLE] |
| [TEAMMATE 3] | [ROLE] |

---

## 📄 License

[CHOOSE A LICENSE — e.g. MIT, Apache 2.0, or leave as "All rights reserved"]

```
[PASTE LICENSE TEXT HERE, OR LINK TO LICENSE FILE]
```

---

## 🙏 Acknowledgements

- [Godot Engine](https://godotengine.org/) — open-source game engine
- [Google Gemini](https://deepmind.google/technologies/gemini/) — AI hint system
- [HACKATHON NAME] organizers and judges
- [ANY OTHER LIBRARIES, ASSETS, OR INSPIRATIONS]
