# 🖥️ Windows 95 Simulator

A highly faithful Windows 95 desktop experience built entirely with pure HTML/CSS/JavaScript — no frameworks, no dependencies. Just open the file in your browser and enjoy the nostalgia.

**[▶️ Live Demo](https://zezecolin.github.io/windows95-simulator/win95.html)**

![Windows 95 Simulator Screenshot](https://img.shields.io/badge/Windows-95-008080?style=for-the-badge&logo=windows95&logoColor=white)
![HTML](https://img.shields.io/badge/HTML-5-E34F26?style=for-the-badge&logo=html5&logoColor=white)
![CSS](https://img.shields.io/badge/CSS-3-1572B6?style=for-the-badge&logo=css3&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-ES6-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)

---

## ✨ Features

### 🖼️ Desktop Environment
- Classic Win95 UI with authentic **#C0C0C0** gray theme
- 3D beveled borders using pure CSS `box-shadow`
- Pixel-perfect MS Sans Serif typography
- Animated taskbar with **Start button**, window buttons, and **live clock**
- Desktop icons with **double-click** detection (400ms threshold)

### 🪟 Window System
- **Drag** windows by their title bar
- **Resize** windows from the bottom-right grip
- **Minimize** to taskbar, **Maximize** to full screen, **Close**
- Window **z-index layering** (click to bring to front)
- Taskbar buttons — click to minimize/restore/activate
- Inactive windows display grayed title bars

### 📝 Notepad
- Full text editor with syntax-free editing
- **Ctrl+S** to save to `localStorage`
- File menu: New, Open (local file), Save, Save As (download)
- Edit menu: Cut, Copy, Paste, Select All
- Format menu: Word Wrap toggle
- Status bar showing **Line / Column** position

### 💻 My Computer
- Static drive/folder icons (Floppy, Local Disk, CD-ROM, etc.)
- Double-click to open sub-folder windows
- Status bar showing selection info

### ⌨️ MS-DOS Prompt
- Authentic black terminal with monospace font
- **Command history** with ↑/↓ arrow keys
- Built-in commands:
  ```
  HELP  CLS   DIR   VER   ECHO  DATE  TIME
  CD    MD    SET   PING  WHOAMI  TYPE  EXIT
  ```

### 🗑️ Recycle Bin
- Opens as a standalone window

### 🖱️ Right-Click Context Menus
- Desktop context menu (Arrange, Refresh, New, Properties)
- Icon context menus (Open, Properties)
- Display Properties dialog

---

## 🚀 Getting Started

### Option 1 — Open Directly
```bash
# Clone the repo
git clone https://github.com/zezecolin/windows95-simulator.git

# Open in browser (macOS)
open win95.html

# Open in browser (Linux)
xdg-open win95.html

# Open in browser (Windows)
start win95.html
```

### Option 2 — No Install Needed
Simply download [`win95.html`](win95.html) and open it in any modern browser.

---

## 🏗️ Architecture

The project follows an **Object-Oriented** design with clearly separated classes:

```
┌─────────────────────────────────────────┐
│              WindowManager              │  ← Core: manages all windows
├──────────┬──────────┬──────────┬────────┤
│ Notepad  │MyComputer│CmdPrompt │Recycle │  ← Applications
├──────────┴──────────┴──────────┴────────┤
│           DesktopIcon                   │  ← Desktop icon component
├─────────────────────────────────────────┤
│        showCtx / closeCtx               │  ← Context menu utilities
└─────────────────────────────────────────┘
```

| Class | Responsibility |
|-------|---------------|
| `WindowManager` | Create, activate, minimize, maximize, close, and layer windows |
| `Notepad` | Text editor with localStorage persistence and file I/O |
| `MyComputer` | Drive/folder viewer with nested window support |
| `CmdPrompt` | MS-DOS terminal with command parser and history |
| `RecycleBin` | Empty recycle bin window |
| `DesktopIcon` | Desktop icon with single/double-click detection |

---

## 🎨 Design Tokens

| Token | Value | Usage |
|-------|-------|-------|
| `--gray` | `#c0c0c0` | Primary surface color |
| `--dark` | `#808080` | Borders, inactive elements |
| `--blue` | `#000080` | Active title bar, selections |
| `--desktop` | `#008080` | Desktop background (teal) |
| `--raised` | 4-layer `box-shadow` | Raised/pressed button look |
| `--sunken` | 4-layer `box-shadow` | Inset/focused field look |

---

## 💾 Data Persistence

Notepad content is automatically saved to `localStorage` under the key `win95_np` and restored on next open.

---

## 🛠️ Tech Stack

- **Pure HTML5 / CSS3 / Vanilla JavaScript (ES6+)**
- Zero external libraries or frameworks
- Zero build tools required
- Single file — everything in `win95.html`

---

## 📋 Requirements

| Browser | Version |
|---------|---------|
| Chrome  | 90+     |
| Firefox | 88+     |
| Safari  | 14+     |
| Edge    | 90+     |

---

## 🗺️ Roadmap

- [ ] More applications (Calculator, Paint, Minesweeper)
- [ ] Draggable desktop icons
- [ ] Window snap / tiling
- [ ] Sound effects (startup chime, error beep)
- [ ] More DOS commands
- [ ] Internet Explorer window (iframe)

---

## 🤝 Contributing

Contributions are welcome! Please feel free to open issues or submit pull requests.

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/add-calculator`)
3. Commit your changes (`git commit -m 'feat: add Calculator app'`)
4. Push to the branch (`git push origin feature/add-calculator`)
5. Open a Pull Request

---

## 📄 License

This project is licensed under the **MIT License** — see the [LICENSE](LICENSE) file for details.

---

## 🙏 Acknowledgements

Inspired by the real Windows 95 UI. Built for nostalgia and educational purposes.

> *"It's now safe to turn off your computer."*
