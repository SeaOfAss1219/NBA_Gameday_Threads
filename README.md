# NBA Game Day Threads Discord Bot

A Discord bot that automatically creates game day threads for NBA games. Runs daily via GitHub Actions—no server required.

## What It Does

Every day at 12:00 PM EST, this bot:
1. Deletes all existing threads in your designated channel
2. Fetches today's NBA games from ESPN
3. Creates a thread for each game (e.g., "Lakers vs Celtics")
4. Posts an opening message in each thread

If there are no games that day, it simply does nothing.

---

## Setup Instructions

### Step 1: Create a Discord Bot

1. Go to the [Discord Developer Portal](https://discord.com/developers/applications)
2. Click **"New Application"** and give it a name (e.g., "NBA Game Threads")
3. Go to the **"Bot"** tab on the left sidebar
4. Click **"Add Bot"** and confirm
5. Under the bot's username, click **"Reset Token"** and copy the token
   - ⚠️ **Save this token somewhere safe—you'll need it later and can only view it once**
6. Scroll down and enable these **Privileged Gateway Intents**:
   - ✅ Server Members Intent
   - ✅ Message Content Intent

### Step 2: Invite the Bot to Your Server

1. In the Developer Portal, go to the **"OAuth2"** tab
2. Click **"URL Generator"**
3. Under **Scopes**, select:
   - ✅ `bot`
4. Under **Bot Permissions**, select:
   - ✅ `Manage Threads`
   - ✅ `Create Public Threads`
   - ✅ `Send Messages`
   - ✅ `Send Messages in Threads`
   - ✅ `Read Message History`
   - ✅ `View Channels`
5. Copy the generated URL at the bottom
6. Open the URL in your browser and select your server to invite the bot

### Step 3: Get Your Channel ID

1. In Discord, go to **User Settings** → **Advanced** → Enable **Developer Mode**
2. Right-click the channel where you want game threads created
3. Click **"Copy Channel ID"**
4. Save this ID—you'll need it in the next step

### Step 4: Set Up the GitHub Repository

1. Create a new repository on GitHub (can be private)
2. Upload these files to the repository:
   ```
   your-repo/
   ├── .github/
   │   └── workflows/
   │       └── nba_threads.yml
   ├── nba_game_threads.py
   ├── requirements.txt
   └── README.md
   ```

### Step 5: Add Repository Secrets

1. In your GitHub repository, go to **Settings** → **Secrets and variables** → **Actions**
2. Click **"New repository secret"** and add:
   - Name: `DISCORD_BOT_TOKEN`
   - Value: Your bot token from Step 1
3. Click **"New repository secret"** again and add:
   - Name: `DISCORD_CHANNEL_ID`
   - Value: Your channel ID from Step 3

### Step 6: Enable GitHub Actions

1. Go to the **"Actions"** tab in your repository
2. If prompted, enable workflows for the repository
3. The bot will now run automatically every day at 12:00 PM EST

---

## Testing

To test the bot without waiting for the scheduled time:

1. Go to the **"Actions"** tab in your repository
2. Click on **"NBA Game Threads"** in the left sidebar
3. Click **"Run workflow"** → **"Run workflow"**
4. Watch the workflow run and check your Discord channel

---

## Customization

### Change the Schedule

Edit `.github/workflows/nba_threads.yml` and modify the cron expression:

```yaml
schedule:
  - cron: '0 17 * * *'  # Currently 12:00 PM EST (17:00 UTC)
```

Cron format: `minute hour day month weekday`

Common examples:
- `'0 15 * * *'` = 10:00 AM EST
- `'0 18 * * *'` = 1:00 PM EST
- `'0 20 * * *'` = 3:00 PM EST

> **Note:** GitHub Actions uses UTC time. EST is UTC-5 (or UTC-4 during daylight saving).

### Change Thread Title Format

Edit `nba_game_threads.py`, find the `create_game_thread` function, and modify:

```python
thread_name = f"{away_team} vs {home_team}"
```

### Change Opening Message

Edit `nba_game_threads.py`, find the `create_game_thread` function, and modify:

```python
opening_message = f"This is the game day chat room for the {away_team} vs {home_team} game!"
```

---

## Troubleshooting

**Bot doesn't create threads:**
- Check that the bot has the correct permissions in the channel
- Make sure the channel is a text channel (not a forum or voice channel)
- Check the Actions tab for error logs

**Threads not being deleted:**
- The bot needs "Manage Threads" permission
- Archived threads older than a certain age may not be accessible

**Workflow not running:**
- Make sure Actions are enabled in your repository
- Check that secrets are named exactly `DISCORD_BOT_TOKEN` and `DISCORD_CHANNEL_ID`
- If the repo is inactive for 60 days, GitHub may pause scheduled workflows

---

## How It Works

- **ESPN API:** Fetches live NBA schedule data (free, no API key needed)
- **discord.py:** Python library for interacting with Discord
- **GitHub Actions:** Runs the script on a schedule without needing a server

The bot logs in, does its job, and immediately logs out. It only runs for a few seconds each day.

---

## License

MIT License - feel free to modify and use as you wish.
