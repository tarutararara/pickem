[README.md](https://github.com/user-attachments/files/28451745/README.md)
# CS2 Major Pick'Em Simulator

Monte Carlo simulation for IEM Cologne Major 2026 Stage 1 predictions. Uses real HLTV data (player Rating 3.0, 3-month win rates, world rankings) to compute each team's probability of advancing through the Swiss stage.

## Quick Start

**Option A -- Just open the file:**
Double-click `pickem.html`. Runs entirely in your browser. No server, no install.

**Option B -- Host online (free):**
Upload to GitHub Pages or Netlify (see below).

## Features

- **16 teams** with real HLTV data: player ratings, win rates, world rankings
- **5 Swiss rounds** simulated: BO1 early rounds, BO3 for elimination/advancement
- **Real round 1 matchups** from HLTV schedule
- **Adjustable parameters:** rating scale, BO1 upset factor, BO3 weight, rank weight
- **Team rating overrides:** click any team to manually set its rating
- **Team Data tab:** view all underlying HLTV stats per team
- **Export CSV** of results
- **Runs in your browser** -- no server, no Python, no installation

## How to Host on GitHub Pages (free)

### Step 1: Create a GitHub account

Go to [github.com](https://github.com) and click **Sign up**. Enter a username, email, and password. Verify your email.

### Step 2: Create a repository

After logging in, click the green **New** button (top-left, or the + icon):

- Repository name: `pickem` (or anything you want)
- Leave it **Public**
- Check **"Add a README file"** (optional)
- Click **Create repository**

### Step 3: Upload the file

On your new repository page, click **"uploading an existing file"** (the blue link in the middle of the page). Drag `pickem.html` from your computer onto the page. At the bottom, click the green **"Commit changes"** button.

### Step 4: Enable GitHub Pages

This is the step that makes your file go live on the internet:

1. Click the **Settings** tab (it's in the row of tabs near the top: Code, Issues, Pull requests, Actions, ..., **Settings**)
2. In the left sidebar, click **Pages** (under "Code and automation")
3. Under **"Branch"**, change the dropdown from "None" to **"main"** (or "master")
4. Next to it, make sure the folder is set to **"/(root)"**
5. Click the **Save** button

GitHub will now build your site. Wait about **30-60 seconds**, then refresh the page. You'll see a blue banner at the top saying:

> "Your site is live at https://yourusername.github.io/pickem/pickem.html"

That's your URL. Anyone can open it.

### Step 5: Share it

Post the URL on Reddit, Discord, Twitter, wherever. The page loads as a complete web app -- sliders, simulation, results, everything works.

## How to Host on Netlify (even easier)

1. Go to [app.netlify.com/drop](https://app.netlify.com/drop)
2. Drag `pickem.html` onto the page
3. Done. Copy the URL it gives you.

## Adjusting Parameters

| Slider | What it does |
|--------|-------------|
| **Rating Scale** | How much rating gaps matter. Lower = bigger favorites, higher = more upsets. At 4.0, a 0.1 rating gap = ~60% win chance. |
| **BO1 Upset Factor** | Extra randomness in Best-of-1 matches. BO1s are inherently volatile -- a hot pistol round can decide the map. 0 = no extra variance, 0.10 = very chaotic. |
| **BO3 Series Weight** | How much a Best-of-3 amplifies the better team's edge. 1.0 = BO3 is just 3 separate BO1s. 2.0 = favorites win more decisively in a series. |
| **Rank Weight** | How much world ranking matters vs player stats & win rate. 1.0 = pure ranking-based, ignoring player form. 0.0 = only player ratings & recent win rate matter, ignoring ranking entirely. |

## Data Sources

All data scraped from HLTV.org team pages (June 1, 2026):

- **World ranking:** Current HLTV world ranking
- **Player Rating 3.0:** 5 current starters' individual ratings
- **Win rate:** Last 3 months win percentage

## Model

The composite rating is computed as:

```
base = 1.40 * e^(-k * (rank - 1))     // exponential decay from ranking
adj  = (avg_player - 1.05) * 0.04      // player rating bonus
      + (winrate - 0.50) * 0.03        // win rate bonus
final = clamp(base + adj, -0.04, +0.04)  // rank dominates
```

Match win probability uses a logistic function:

```
P(A wins) = 1 / (1 + e^(-scale * (rating_A - rating_B)))
```

Round 1 uses real HLTV scheduled matchups. Subsequent rounds pair teams with equal records (Buchholz Swiss system).

## Files

- `pickem.html` -- standalone web app (open in any browser)
- `app.py` -- Flask server version (run locally with `python app.py`)
- `templates/index.html` -- Flask template
- `composite_sim.py` -- command-line simulation script
- `config.json` -- tunable parameters

## License

MIT
