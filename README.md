# Post-It

A living-room trivia game with a wagering twist. Two to six teams, five rounds of five questions, and every round each team gets one chip each of **2, 4, 6, 8 and 10**. On every question you wager exactly one unused chip. Right answer banks the chip, wrong answer banks nothing. Chips reset each round. Highest total after 25 questions wins.

Everything is one static file, `index.html`. No build step, no server, no accounts.

**Play it:** https://superplatypus54.github.io/postit/

## The three roles

Open the same URL and pick a role on the lobby screen.

1. **Host control** (laptop). Reads the questions, runs the manual 60-second timer, logs each team's wager and Right/Wrong verdict, scores, undoes, advances rounds.
2. **TV display** (second window of the same browser, fullscreen on the big screen). Shows the question, the answer on reveal, the timer, scores, and each team's remaining chips. Host and TV sync through `BroadcastChannel` and `localStorage`, so both windows must be in the **same browser on the same computer**.
3. **Team phone** (optional). In Phone mode, one person per team scans the QR code on the TV or opens the URL and types the six-character game code, claims a seat, then sends an answer plus a wager for each question. The host sees the typed answer with the wager pre-selected and can override either.

## Running a game night

1. Open the URL on the laptop and pick **Host control**.
2. Open the same URL in a second window, pick **TV display**, drag it to the TV and fullscreen it.
3. On the host, set the team count (2 to 6) and choose **Post-it notes** or **Phones**.
   - Post-it mode: type the team names in the panel on the right.
   - Phone mode: a six-character code and a QR appear on the host and TV. One phone per team scans it, names the team and takes a seat. Each claimed seat on the host has a **Kick** button that frees the seat and tells that phone to pick again; **Clear seats** does the same for everyone.
4. **Start the game**, then **Start round 1**.
5. For each question: start the timer (`Space`), **Reveal** (`R`), pick each team's wager and Right/Wrong, **Score it** (`Enter`), **Next** (`N`). **Undo last** is in the top bar.
6. After round 5 the TV shows the final table. **Reset game** on the host clears everything.

## How the phones connect

- Phones talk directly to the host browser over WebRTC using [PeerJS](https://peerjs.com). The public PeerJS signalling server is used only to find each other; game messages go peer to peer.
- The host is the single source of truth. Seat claims are first come first served and resolved on the host. A phone that reloads keeps its seat until the host kicks it.
- The relay id is a hash of the game code, not the code itself, so nobody can find a game by guessing ids. The code on screen stays short and readable.
- Works best with everyone on the same Wi-Fi. Some networks block WebRTC. If a phone can't connect it says so in one line, and the host simply carries on with post-its. Scoring never depends on the network.
- Switching Phone mode off mid-game puts you straight back on post-its with nothing lost.

## Question packs

Questions live in JSON packs, not in the page. A pack is one file:

```json
{
  "name": "General Knowledge Vol. 1",
  "rounds": [
    { "title": "History", "questions": [ { "q": "...", "a": "...", "d": 1 } ] }
  ]
}
```

Every round has exactly 5 questions (one per chip), a pack has 1 to 10 rounds, and `d` is a difficulty from 1 to 3 that only the host sees. Bad packs are refused on import with a message naming the round that is wrong.

**Writing a pack.** Paste [PACK_PROMPT.md](PACK_PROMPT.md) into Claude, save the JSON it returns.

**Importing a pack for tonight.** On the host setup screen, find the **Question pack** row, press **Import pack**, paste the JSON, press **Add pack**. It appears in the dropdown with an *imported* tag and is kept in this browser until you delete it. Pick it before you press Start; the pack is fixed once a game begins.

**Adding a pack to the repo permanently.** Drop the file in `packs/`, add a line for it to `packs/index.json`:

```json
[
  { "file": "general-knowledge-1.json", "name": "General Knowledge Vol. 1" },
  { "file": "your-pack.json", "name": "Your Pack Name" }
]
```

then commit and push. It shows up in the dropdown for everyone. When the page is opened straight from disk (`file://`) it cannot fetch `packs/`, so it falls back to the built-in copy of General Knowledge Vol. 1 embedded in the page.

## Local development

Serve the folder over HTTP (some browser features are off on `file://`):

```bash
python3 -m http.server 8000
```

Then open http://localhost:8000/ for the host and TV.

## Deploying

GitHub Pages serves the `main` branch root. Edit `index.html`, commit, push. Done.
