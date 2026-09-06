# Post-It

A living-room trivia game with a wagering twist. Two to six teams, five rounds of five questions, and every round each team gets one chip each of **2, 4, 6, 8 and 10**. On every question you wager exactly one unused chip. Right answer banks the chip, wrong answer banks nothing. Chips reset each round. Highest total after 25 questions wins.

Everything is one static file, `index.html`. No build step, no server, no accounts.

**Play it:** https://superplatypus54.github.io/postit/

## The three roles

Open the same URL and pick a role on the lobby screen.

1. **Host control** (laptop). Reads the questions, runs the manual 60-second timer, logs each team's wager and Right/Wrong verdict, scores, undoes, advances rounds.
2. **TV display** (second window of the same browser, fullscreen on the big screen). Shows the question, the answer on reveal, the timer, scores, and each team's remaining chips. Host and TV sync through `BroadcastChannel` and `localStorage`, so both windows must be in the **same browser on the same computer**.
3. **Team phone** (optional). In Phone mode, one person per team scans the QR code on the TV or opens the URL and types the four-character game code, claims a seat, then sends an answer plus a wager for each question. The host sees the typed answer with the wager pre-selected and can override either.

## Running a game night

1. Open the URL on the laptop and pick **Host control**.
2. Open the same URL in a second window, pick **TV display**, drag it to the TV and fullscreen it.
3. On the host, set the team count (2 to 6) and choose **Post-it notes** or **Phones**.
   - Post-it mode: type the team names in the panel on the right.
   - Phone mode: a code and QR appear on the host and TV. One phone per team scans it, names the team and takes a seat.
4. **Start the game**, then **Start round 1**.
5. For each question: start the timer (`Space`), **Reveal** (`R`), pick each team's wager and Right/Wrong, **Score it** (`Enter`), **Next** (`N`). **Undo last** is in the top bar.
6. After round 5 the TV shows the final table. **Reset game** on the host clears everything.

## How the phones connect

- Phones talk directly to the host browser over WebRTC using [PeerJS](https://peerjs.com). The public PeerJS signalling server is used only to find each other; game messages go peer to peer.
- The host is the single source of truth. Seat claims are first come first served and resolved on the host. A phone that reloads keeps its seat.
- Works best with everyone on the same Wi-Fi. Some networks block WebRTC. If a phone can't connect it says so in one line, and the host simply carries on with post-its. Scoring never depends on the network.
- Switching Phone mode off mid-game puts you straight back on post-its with nothing lost.

## Local development

Serve the folder over HTTP (some browser features are off on `file://`):

```bash
python3 -m http.server 8000
```

Then open http://localhost:8000/ for the host and TV.

## Deploying

GitHub Pages serves the `main` branch root. Edit `index.html`, commit, push. Done.
