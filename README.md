[Run](https://marcoleonardi97.github.io/screensaver)

Retro Notes Screensaver

A retro-style HTML screensaver for your Fire Stick TV that displays a large clock and cycling notes. Notes are stored in a JSON file, synced with GitHub, and can be managed directly from the terminal.

Features

Retro-style screensaver with a large digital clock.

Notes displayed at the bottom, auto-refreshing from notes.json.

Local notes stored in JSON and synced to GitHub automatically.

Terminal commands for adding and deleting notes:

newnote "Your note here" — adds a new note, commits, and pushes to GitHub.

delnotei INDEX — deletes a note by index.

delnotei all — clears all notes.

HTML Screensaver

The main file is retro_screensaver.html. It uses JavaScript to display the clock and notes. Notes are fetched dynamically from notes.json hosted on GitHub Pages. To use on Fire Stick, the repository must be published to GitHub Pages. Open the published URL in Silk Browser or Downloader app on Fire Stick. You can optionally set up auto-refresh of notes every few minutes.

Installation

Clone the repository:
git clone https://github.com/YOUR_USERNAME/screensaver.git
cd screensaver

Make sure notes.json exists and is a valid array:
[]

Add your notes using:
newnote "Your first note"

Publish the repository to GitHub Pages by going to your repo on GitHub → Settings → Pages, select the main branch (or docs folder) as the source, save, and wait for deployment. Open the GitHub Pages URL on your Fire Stick via Silk Browser or Downloader.

Terminal Commands

Add a note: newnote "Remember to call mom"

Delete a note by index: delnotei 0 (deletes first note), delnotei -1 (deletes last note)

Clear all notes: delnotei all

All commands automatically commit and push changes to GitHub.

Notes

Keep newnote and delnotei functions in your ~/.zshrc (or ~/.bashrc) for easy access. Make sure jq is installed (brew install jq) for JSON manipulation. Use quotes around multi-word notes to prevent shell issues. The screensaver only works if the repository is published to GitHub Pages.
