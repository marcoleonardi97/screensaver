[Run](https://marcoleonardi97.github.io/screensaver)

# 📺 Retro TV Screensaver

A simple vintage CRT television screensaver that displays the current time and rotating notes/messages. Perfect for Fire TV Sticks, Raspberry Pis, old tablets, or any browser-enabled device you want to repurpose as a retro display.

## ✨ Features

- **Beautiful retro aesthetic** - Authentic CRT television with bezel, scanlines, color bars, and subtle screen effects
- **Live clock display** - Large, easy-to-read time with chromatic aberration glow effect
- **Dynamic notes** - Displays rotating messages from a JSON file
- **Fully responsive** - Works on any screen size from phone to TV
- **Zero dependencies** - Pure HTML, CSS, and vanilla JavaScript
- **GitHub Pages ready** - Host it for free and update notes via terminal commands
- **Accessibility friendly** - Respects `prefers-reduced-motion` settings

## 🚀 Quick Start

### Option 1: Use GitHub Pages (Recommended)

1. **Fork this repository** or create a new repo with these files
2. Enable GitHub Pages in your repository settings (Settings → Pages → Source: main branch)
3. Open the GitHub Pages URL on your device (e.g., `https://yourusername.github.io/retro-screensaver/`)
4. Bookmark it or set it as your browser homepage on Fire TV/streaming devices

### Option 2: Local Setup

1. Clone this repository:
   ```bash
   git clone https://github.com/yourusername/retro-screensaver.git
   cd retro-screensaver
   ```

2. Open `index.html` in your browser, or serve it locally:
   ```bash
   python -m http.server 8000
   ```

3. Navigate to `http://localhost:8000`

## 📝 Managing Notes

The screensaver displays rotating messages from `notes.json`. Update them easily via terminal commands:

### Add a New Note
```bash
newnote "Remember to take out the trash"
```

### Delete a Note
```bash
delnote 2  # Deletes the note at index 2
```

### Clear All Notes
```bash
clearnotes
```

### Set Up Terminal Commands (Optional)

Add these functions to your `~/.bashrc` or `~/.zshrc`:

```bash
# Navigate to your repo directory first
cd /path/to/retro-screensaver

# Add a new note (supports multiwords, no need for "")

newnote() {
  CWD=$(pwd)
  cd
  if [ $# -eq 0 ]; then
    echo "Usage: newnote \"Your note here\""
    return 1
  fi

  REPO="screensaver"   # <-- change this
  FILE="$REPO/notes.json"

  # Join all arguments into a single string
  NOTE="$*"

  # Ensure the JSON file exists and is valid
  if [ ! -f "$FILE" ]; then
    echo "[]" > "$FILE"
  fi

  # Append the note safely using jq
  if command -v jq >/dev/null 2>&1; then
    tmp=$(mktemp)
    jq --arg note "$NOTE" '. + [$note]' "$FILE" > "$tmp" && mv "$tmp" "$FILE"
  else
    echo "Error: jq is required for safe JSON handling. Install it with 'brew install jq'."
    return 1
  fi

  # Git add, commit, push
  cd "$REPO" || return 1
  git add notes.json
  git commit -m "New note: $NOTE"
  git push origin main

  echo "Note added and pushed: $NOTE"
  cd "$CWD"
}

# Delete notes (index or all)

delnote() {
  CWD=$(pwd)
  cd
  if [ $# -eq 0 ]; then
    echo "Usage: delnotei INDEX | all"
    return 1
  fi

  REPO="$HOME/screensaver"
  FILE="$REPO/notes.json"
  ARG=$1

  if ! command -v jq >/dev/null 2>&1; then
    echo "Error: jq is required. Install it with 'brew install jq'."
    return 1
  fi

  cd "$REPO" || return 1

  if [ "$ARG" = "all" ]; then
    # Clear the JSON array
    echo "[]" > "$FILE"
    git add notes.json
    git commit -m "Cleared all notes"
    git push origin main
    echo "All notes cleared."
  else
    INDEX=$ARG
    # Delete note at specified index
    tmp=$(mktemp)
    jq "del(.[$INDEX])" "$FILE" > "$tmp" && mv "$tmp" "$FILE"
    git add notes.json
    git commit -m "Deleted note at index $INDEX"
    git push origin main
    echo "Deleted note at index $INDEX."
  fi
  cd "$CWD"
}
```

**Note:** These commands require `jq` (JSON processor). Install it with:
- **macOS:** `brew install jq`
- **Linux:** `sudo apt-get install jq` or `sudo yum install jq`
- **Windows:** Download from [stedolan.github.io/jq](https://stedolan.github.io/jq/)

## 🔥 Fire TV Stick Setup

1. Install **Firefox** or **Amazon Silk Browser** from the Amazon App Store
2. Navigate to your GitHub Pages URL
3. Add it to bookmarks or set as homepage
4. Optional: Use a screensaver app that opens a URL on idle

### Tips for Fire TV:
- Use the Fire TV remote to navigate - the interface is remote-friendly
- Notes rotate every 10 seconds
- The clock updates every second in real-time
- No internet after initial load? It'll show the last fetched notes

## 📁 File Structure

```
retro-screensaver/
├── index.html          # Main screensaver page
├── notes.json          # Your rotating messages
├── README.md           # This file
└── screenshot.png      # Optional preview image
```

## 🎨 Customization

### Change Note Rotation Speed
Edit line in `index.html`:
```javascript
}, 10000);  // Change 10000 to desired milliseconds
```

### Modify Visual Style
All styling is in the `<style>` section. Easily adjust:
- Colors (search for color values like `#ff3b30`)
- Animation speeds (look for `animation` properties)
- Screen effects (adjust `opacity` values for scanlines, noise, etc.)

### Default Notes
Edit `notes.json`:
```json
[
  "Welcome to your retro display",
  "Time to make coffee ☕",
  "Don't forget your meeting at 3pm"
]
```

## 🛠️ Technical Details

- **No external dependencies** - Everything runs locally after initial load
- **Responsive design** - Automatically adapts to any screen size
- **Performance optimized** - Smooth animations even on low-power devices
- **Accessibility** - Honors reduced motion preferences
- **Cross-browser compatible** - Works on all modern browsers


## 🤝 Contributing

Contributions are welcome! Feel free to:
- Report bugs
- Suggest new features
- Submit pull requests
- Share your customizations

## 📄 License

MIT License - feel free to use this project however you'd like!

## 💡 Ideas for Use

- Kitchen display for family messages
- Office lobby display
- Smart mirror integration
- Digital picture frame alternative
- Retro gaming room decoration
- Meeting room status display

## 🙏 Credits

Created with nostalgia for those chunky CRT televisions we all grew up with. Inspired by vintage test patterns and that distinctive scan-line aesthetic.

---

**Enjoy your retro screensaver!** If you create something cool with this, I'd love to see it. ⭐ Star this repo if you find it useful!
