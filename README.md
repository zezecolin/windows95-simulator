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
| `Calculator` | Standard / scientific calculator with memory and keyboard support |
| `DesktopIcon` | Desktop icon with single/double-click detection |

---

## 🔄 Agent Operational Flow Framework

> This section describes the full runtime flow of the Windows 95 Simulator — from page load to user interaction.

### 1. Bootstrap / Initialization Sequence

When the HTML file is opened in the browser the script block at the bottom of `<body>` runs synchronously, wiring everything together in a fixed order:

```
Page Load
    │
    ├─► new WindowManager()        // creates wins Map, sets zBase=100
    │
    ├─► new Notepad(wm)            // stores ref to wm, reads localStorage key
    ├─► new MyComputer(wm)
    ├─► new RecycleBin(wm)
    ├─► new CmdPrompt(wm)          // initialises cwd = 'C:\WINDOWS', hist[]
    ├─► new Calculator(wm)         // initialises display state & sci mode
    │
    ├─► new DesktopIcon × 6        // appends .dicon divs to #desktop
    │     (My Computer, Recycle Bin, Notepad, MS-DOS, Calculator, About)
    │
    ├─► tick()  +  setInterval(tick, 1000)   // live clock in tray
    │
    ├─► initStartMenu()            // wires Start button & menu items
    │
    ├─► initDesktopCtx()           // wires desktop right-click
    │
    └─► setTimeout(showWelcome, 400ms)       // delayed welcome dialog
```

---

### 2. Window Lifecycle

Every visible window passes through the following states, all managed by `WindowManager`:

```
         ┌──────────────────────────────────────────────┐
         │            wm.create(cfg)                    │
         │  • Build DOM (.window div + titlebar/grip)   │
         │  • Add to wins Map                           │
         │  • Wire drag / resize / control events       │
         │  • Create taskbar button                     │
         │  • Call cfg.build(contentEl, id)             │
         │  • wm.activate(id)                           │
         └─────────────────┬────────────────────────────┘
                           │
                    ┌──────▼──────┐
                    │   ACTIVE    │◄──────────────────────────────┐
                    │  (visible,  │                               │
                    │  z-top,     │  click window / taskbar btn   │
                    │  blue title)│  (when minimized or inactive) │
                    └──┬────┬─────┘                               │
         minimize()    │    │ toggleMax()                         │
              ┌────────┘    └────────┐                            │
              ▼                      ▼                            │
       ┌────────────┐        ┌──────────────┐                     │
       │ MINIMIZED  │        │  MAXIMIZED   │                     │
       │ display:   │        │  fills whole │                     │
       │ none, in   │        │  viewport    │                     │
       │ taskbar    │        │  (prevRect   │                     │
       └──────┬─────┘        │  saved)      │                     │
              │              └──────┬───────┘                     │
              │restore()            │toggleMax() again             │
              └─────────────────────┴─────────────────────────────┘

         Any state ──► wm.close(id) ──► remove DOM + taskbar btn
                                         + delete from wins Map
                                         + call cfg.onClose()
```

---

### 3. Event Handling Flow

The simulator uses three layers of event delegation:

```
Browser Event
     │
     ├─ mousedown on .window
     │        └─► wm.activate(id)   (capture phase, true)
     │
     ├─ mousedown on .wtitlebar  (drag)
     │        ├─► record ox/oy, ol/ot
     │        ├─► attach document mousemove  → reposition window (clamped)
     │        └─► attach document mouseup   → remove mousemove listener
     │
     ├─ mousedown on .wgrip  (resize)
     │        ├─► record sx/sy, sw/sh
     │        ├─► attach document mousemove  → resize window (min 200×100)
     │        └─► attach document mouseup   → remove mousemove listener
     │
     ├─ click on .dicon  (desktop icon)
     │        ├─► click #1 within 400 ms  → select highlight only
     │        └─► click #2 within 400 ms  → onOpen() (open the app)
     │
     ├─ contextmenu on #desktop / .dicon
     │        └─► showCtx(items, cx, cy)
     │                ├─► build #ctx div with .cmi items
     │                ├─► clamp position inside viewport
     │                └─► document mousedown (once) → closeCtx()
     │                     (item actions use mousedown + stopPropagation
     │                      so they fire before the outside-click handler)
     │
     ├─ click on #start-btn
     │        └─► toggle #start-menu display block/none
     │
     └─ click on .sm-item[data-app]
              └─► <App>.open()   (see App Flow below)
```

---

### 4. Application Open / Singleton Flow

Each application uses a **singleton guard** (`this.wid`) to prevent duplicate windows:

```
<App>.open()
     │
     ├─[wid exists in wins Map]──► wm.activate(wid)   // just focus
     │
     └─[no open window]
            │
            ├─► wm.create({ title, icon, width, height,
            │               menubar, statusbar, build, onClose })
            │
            └─► build(contentEl, id)
                     ├─ inject HTML into content area
                     ├─ wire app-specific event listeners
                     └─ (Notepad)     restore from localStorage
                        (CmdPrompt)   focus input, print banner
                        (Calculator)  render button grid, attach keydown
                        (MyComputer)  render drive icons
```

---

### 5. Data Flow & Persistence

```
┌─────────────────────────────────────────────────────┐
│                   In-Memory State                   │
│                                                     │
│  WindowManager.wins  Map<id, {el, minimized, …}>    │
│  CmdPrompt.hist      string[]   (command history)   │
│  CmdPrompt.cwd       string     (current directory) │
│  Calculator.display  string     (displayed number)  │
│  Calculator.prev     string     (left operand)      │
│  Calculator.op       string     (pending operator)  │
│  Calculator.mem      number     (memory register)   │
└──────────────────────────┬──────────────────────────┘
                           │
                  persisted across
                  page reloads via
                           │
                    ┌──────▼───────┐
                    │ localStorage │
                    │  key:        │
                    │  "win95_np"  │
                    │  (Notepad    │
                    │   content)   │
                    └──────────────┘
```

---

### 6. Component Interaction Map

```
                 ┌──────────────────────┐
                 │     WindowManager    │
                 │  wins, zBase, active │
                 └──┬──┬──┬──┬──┬──────┘
    create/activate │  │  │  │  │ close/minimize
          ┌─────────┘  │  │  │  └─────────────┐
          ▼            │  │  │                 ▼
       Notepad         │  │  │           Calculator
      (wm ref)         │  │  │            (wm ref)
          │            │  │  │                 │
       File I/O     MyComputer  CmdPrompt    Keyboard
      localStorage  (wm ref)   (wm ref)     events
                       │           │
                 Drive viewer  DOS parser
                 sub-windows   hist array

     DesktopIcon ──► double-click ──► <App>.open()
     showCtx ──► menu items ──► <App>.open() / wm.close()
     initStartMenu ──► .sm-item click ──► <App>.open()
     tick() ──► setInterval ──► #clock textContent
```

---

### 7. Full Startup-to-Interaction Timeline

```
t=0ms    HTML parsed, <script> executes
          └─ wm, notepad, mycomputer, recyclebin, cmd, calculator created
          └─ 6 DesktopIcons added to #desktop
          └─ tick(), initStartMenu(), initDesktopCtx() called

t=400ms   Welcome dialog appears via wm.create()

t=1000ms  First clock tick (and every 1 s thereafter)

User double-clicks "Notepad" icon
          └─ DesktopIcon click handler fires twice within 400 ms
          └─ notepad.open() called
          └─ wm.create() builds window DOM, appends to #desktop
          └─ build() restores textarea from localStorage
          └─ wm.activate() sets z-index, marks .active on taskbar btn

User types in Notepad textarea
          └─ 'input' event → this.mod=true, _upTitle(), _upStatus()

User presses Ctrl+S
          └─ 'keydown' event → notepad.save()
          └─ localStorage.setItem('win95_np', textarea.value)

User clicks taskbar button (active window)
          └─ wm.minimize(id) → el.style.display='none'

User clicks taskbar button again (minimized)
          └─ wm.restore(id) → el.style.display='', wm.activate(id)

User clicks ✕ (close)
          └─ wm.close(id) → el.remove(), taskbar btn removed,
             wins.delete(id), onClose() sets notepad.wid=null
```

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
