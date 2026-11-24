# Episode Kickoff

Start a new podcast episode in one click. Episode Kickoff downloads everyone's raw Riverside tracks into the episode's `RAW` folder, creates the episode's DaVinci Resolve project from your show's template, and imports the tracks into it. It can then hand the episode to the AI agent you use, such as Claude, ChatGPT, Codex or Gemini.

> [!NOTE]
> **This app is vibecoded.** It was built by describing what it should do to Claude, Anthropic's AI model, which wrote the code, the tests and this guide. It's tested, but it comes as is, with no warranty, so keep backups of anything important.

**[Download the latest version](https://github.com/Artemis-AV/episode-kickoff/releases/latest)** · Mac with Apple silicon · DaVinci Resolve Studio · Riverside

## Contents

- [What it does](#what-it-does)
- [What you need](#what-you-need)
- [Install](#install)
- [Set it up](#set-it-up)
- [Each episode](#each-episode)
- [Automations](#automations)
- [Naming fields](#naming-fields)
- [Updates and the About tab](#updates-and-the-about-tab)
- [Where your data lives](#where-your-data-lives)
- [Troubleshooting](#troubleshooting)
- [Command line](#command-line)
- [About this project](#about-this-project)

## What it does

1. **Folders.** Uses the episode folder you pick (or makes one from the episode number), with a `RAW` folder inside.
2. **Riverside tracks.** Downloads each person's raw track into `RAW`, from one recording or several takes. There are three ways to get them:
   - **Riverside API**: fully automatic. Needs a Riverside API key.
   - **Riverside website (automated)**: works on any Riverside plan. The app opens its own browser window and clicks *Download → Raw video* for each track.
   - **Downloads folder**: you click Download in Riverside, and the app moves each finished file into `RAW`.
3. **Resolve project.** Creates the episode's project from your show's template in the right Project Manager folder, opens it, and imports the tracks into its **Raw** bin (one bin per take).
4. **Automations (optional).** When the run finishes, starts the agents or services you choose with the episode's details. For example, Claude builds a rough cut in the new project, or Codex writes show notes. See [Automations](#automations).

Each show gets its own **preset** with its folders, naming, Riverside settings, Resolve template and automations.

## What you need

- A Mac with Apple silicon (M1 or newer).
- DaVinci Resolve Studio.
- A Riverside account. API mode needs a Riverside API key, which Riverside offers on select Business plans. The website and Downloads folder modes work with any plan.
- Optional: for automations, the agent or service you want to use. See [Automations](#automations).

## Install

1. Download **Episode-Kickoff-*version*.dmg** from the [latest release](https://github.com/Artemis-AV/episode-kickoff/releases/latest).
2. Open it and drag **Episode Kickoff** onto **Applications**.
3. Open Episode Kickoff from your Applications folder. The first time, macOS says it can't verify the app, because it isn't notarized through a paid Apple developer account. Click **Done**, go to **System Settings → Privacy & Security**, scroll down and click **Open Anyway** next to Episode Kickoff. You only do this once; updates open normally.
4. macOS may ask whether Episode Kickoff can use files in places like Desktop, Downloads or an external drive, and whether it can use the Riverside key saved in your Keychain. Allow these (choose **Always Allow** for the Keychain).

## Set it up

This takes about 10 minutes, once.

### 1. Let Resolve accept the app

In DaVinci Resolve Studio, go to **Preferences → System → General** and set **External scripting using** to **Local**.

### 2. Make your show's template

Open the project every new episode should start from, with its bins, timelines, render presets and graphics. Use **File → Export Project…** to save it as a `.drp` file.

You can also keep the template as a project inside Resolve and choose **Template project inside Resolve** in the preset; the app copies it for each episode.

> [!TIP]
> Keep the template lean: bins, graphics and settings, but no episode media or old timelines.

### 3. Fill in your show's preset

Click **Edit Presets…**. A new install starts with one blank preset called **My Show**. Rename it and go through the tabs:

| Tab | What to set |
|---|---|
| **Show & Folders** | **Preset name** and **Show name**. **Episode folder**: *Pick an existing folder each run* (the default) or *Create a new folder from a name*. **Show folder**: where the folder picker opens, or where new episode folders are made. Also **RAW folder name**, **Several recordings** (where each take's tracks go) and any **Other subfolders** to create. |
| **Riverside API** | **Default track source**, the **API key** to use, **Recordings from** (a studio or project), what to **Download** (default: *Raw video*) and **Track file names**. |
| **Website & Downloads** | The **Browser** for website mode and the button wording it clicks, plus the folder to **Watch** in Downloads folder mode. |
| **Resolve** | **Start from** (*Template .drp file*, *Template project inside Resolve* or *No template*), the **Project Manager folder**, the **Project name** (for example `{show} EP {episode}`), **If it already exists**, and **Import the RAW tracks into the bin** (default `Raw`). |
| **Resolve Library** | Leave it on the library that's open unless your shows live in different libraries. Blackmagic Cloud settings are here too. |
| **Automations** | Which [automations](#automations) start when a run for this show finishes. **Manage Automations…** adds and edits them. |

The **Preview** at the bottom shows the folder and project name an episode would get. Click **Save** when you're done.

Use **New** or **Duplicate** to add more shows. To give someone your show's settings, click **Export…** and send them the file; they click **Import…**. API keys and automations are never included.

### 4. Connect Riverside

- **Riverside API**: click **API Keys…**, then **Add / Replace…**, and paste your key. **Test** checks that it works. Keys are kept in your Mac's Keychain, not in your presets.
- **Riverside website (automated)**: choose this source and click **Sign in to Riverside…** once. That signs in the app's own browser window, which remembers you. If you see **Install browser support**, click it once; it's a one-time download of about a minute, and isn't needed if Google Chrome is installed. If Google sign-in is blocked in that window, sign in with your email instead.
- **Downloads folder**: nothing to set up.

### 5. Check your setup

Click **Check Setup…** to see what's working: the app, browser support, your API key and the connection to Resolve.

## Each episode

1. Pick the **Show**.
2. Click **Choose…** next to **Folder** and pick the episode's folder. The **Episode #** is read from the folder's name (`EP 159` → 159), and you can type a different one. **Next #** suggests the next number. If your preset makes the folders, the folder is filled in for you, and **Use preset** puts it back after you change it.
3. Optionally add a **Title**, and check the **Date** (it's used in names like `{m}.{d}.{yy}`).
4. Under **Riverside tracks**, pick the source:
   - **Riverside API**: pick the recording from **Recordings**, or paste its ID or link (in Riverside: ⋯ menu → Copy Recording ID). Picking it also fills in the episode number and date. If the episode was recorded in several takes, add each one; **Goes into** shows where each take's tracks will land. **Refresh** reloads the list, and **Remove** takes one off.
   - **Riverside website (automated)**: paste the recording page's link (for several takes, one link per take, separated by spaces), or leave it blank and open the recording in the app's window when it asks.
   - **Downloads folder**: after you click **Start**, click **Download → Raw video** in Riverside for each person. Click **Finish downloads** when they're all in, or set **Stop after** to the number of files.
5. Leave **Create the episode's Resolve project from the show template** ticked, or untick it to only download.
6. If the show has [automations](#automations), they're listed under **Automations**. Untick **When it's done, start…** to skip them this time.
7. Click **Start** (⌘R).

Each file shows its progress, and the steps tick off: **Folders**, **Riverside tracks**, **Resolve project** and **Automations**. When it's done you get a notification, and Resolve opens the new project with the tracks in its **Raw** bin. **Open Episode Folder** shows the folder in Finder, and the button next to **Automations…** opens what the automations started, such as the Claude or ChatGPT conversation, or a command's log.

### Several takes

Riverside makes a separate recording for each take, and Take 1 is the one recorded first. When a run has more than one, each take's tracks go in their own folder inside `RAW` (`RAW/Take 1`, `RAW/Take 2`, …) and their own bin in Resolve (`Raw/Take 1`, …). To rename those folders, or to put everything straight into `RAW`, use **Several recordings** in the preset. The Downloads folder source can't tell takes apart, so it always puts files straight into `RAW`.

### Good to know

- **Safe to run again.** Files already in `RAW` are skipped, an existing project is reused (see **If it already exists**), and clips already in the Raw bin aren't imported twice.
- **Cancel** (⌘.) stops everything. Partial downloads pick up where they left off next time.
- **Your Mac stays awake** during a run. Closing the lid still puts it to sleep.
- **Blackmagic Cloud** project libraries work too. Set the media location and sync options on the **Resolve Library** tab.

## Automations

An automation hands each finished episode to an agent or service. For example, Claude builds a rough cut in the new Resolve project, Codex writes show notes into the episode folder, or a webhook tells your team through Zapier.

### Set one up

1. Click **Automations…** in the main window (or **File → Automations…**), then **New**, and pick a type:

   | Type | What it starts | What you need |
   |---|---|---|
   | **Claude scheduled task** | A [scheduled task](https://claude.ai/code/routines) in Claude that runs in the cloud | A Claude Pro, Max, Team or Enterprise plan. In the task, choose **⋯ → Edit**, click **+** under **Triggers** and choose **API**. Copy the **Fire URL**, then click **Generate token** and copy the token (it's shown once). |
   | **ChatGPT workspace agent** | A ChatGPT workspace agent, in the cloud | A ChatGPT Business, Enterprise or Edu workspace. Add an **API channel** to the agent and copy its trigger ID (`agtch_…`), and make an access token in **ChatGPT Admin → Access tokens** with the **Workspace Agents** scope. |
   | **Claude Code**, **Codex** or **Gemini CLI** | That command-line agent on your Mac, working in the episode folder | The agent installed and signed in. Codex works with a ChatGPT plan. |
   | **Other command** | Any command or script on your Mac | The command |
   | **Shortcut** | A shortcut from the Shortcuts app | The shortcut. It can use the ChatGPT app's **Ask ChatGPT** action, Apple Intelligence, or any other app's actions. |
   | **Webhook** | Zapier, Make, n8n, your own server, or any agent platform that starts from a webhook | The URL, plus a secret header if it needs one |

2. Fill in what it asks for, and write what the agent should do under **Instructions**.
3. Click **Test**. The agent gets a test message saying there's nothing to work on, so it just replies. For commands and Shortcuts, **Open Log** shows what happened.
4. Under **Runs for**, tick the shows that should use it, then click **Save**. You can also change this in **Edit Presets… → Automations**.

> [!IMPORTANT]
> Claude and ChatGPT only let other apps start tasks that run in the cloud, so those tasks can't use apps on your Mac, such as DaVinci Resolve. A Claude scheduled task set to run on your computer has no API trigger to add. For work in Resolve, use a command-line agent on your Mac, such as Claude Code.

> [!TIP]
> A Claude scheduled task keeps its own instructions in Claude, so **Instructions** in Episode Kickoff is optional for it. **Copy Starter Instructions** gives you instructions to paste into the task: they explain the episode details it will receive and how to answer a test.

### What the agent gets

Your instructions, followed by the episode's details as JSON:

- the show, episode number, title and date
- the episode folder and its `RAW` folder
- the takes: each take's folder, its files and the Resolve bin they're in
- the Resolve project: its name, Project Manager folder and library
- whether the run had any problems

How they arrive depends on the type:

- **Claude scheduled task** and **ChatGPT workspace agent**: as the message that starts the run.
- **Commands**: on standard input, and saved as files you can name in the command: `{prompt_file}` (instructions and details) and `{details_file}` (the JSON). `{episode_folder}`, `{raw_folder}`, `{project}`, `{show}`, `{episode}` and `{title}` work too. They're quoted for you, so don't put quotes around them. The same values are in `EK_…` environment variables. Commands run in the episode folder through your login shell.
- **Shortcuts**: a JSON file as the shortcut's input. Start the shortcut with **Get Dictionary from Input**; the ready-made prompt is in `text`.
- **Webhooks**: a POST with the JSON as its body; the ready-made prompt is in `text`.

The command-line templates start from these commands, which you can change:

| Template | Command |
|---|---|
| Claude Code | `claude -p --permission-mode auto --permission-prompts none` |
| Codex | `codex exec --skip-git-repo-check --sandbox workspace-write -` |
| Gemini CLI | `gemini --approval-mode yolo -p "Follow the instructions above."` |

To work in DaVinci Resolve, a command-line agent needs its own connection to Resolve, such as a Resolve MCP server set up in that agent. Without one, it can only work with the files in the episode folder.

> [!WARNING]
> Commands run with your Mac user's permissions, and these templates tell the agent to work without asking before each step. Only add commands you trust.

### When it runs

- When a run for one of its shows finishes.
- By default only after a run with no problems, and only once per episode, so running the same episode again doesn't start it twice. Both are settings on each automation (**Only after a run with no problems**, **Only once per episode**).
- **Run → Run an Automation** starts any automation by hand for the last finished run.

Commands and Shortcuts run in the background, even after you quit Episode Kickoff, and keep your Mac awake while they run. If the app is open when one finishes, you get a notification.

Tokens and other secrets are kept in your Mac's Keychain. Automations stay on your Mac: they're never exported with presets, so a preset someone sends you can't bring a command with it.

## Naming fields

Use these in episode folder names, project names, Project Manager folders and track file names:

| Field | Example | Notes |
|---|---|---|
| `{show}` | My Show | the preset's show name |
| `{episode}` | 159 | `{episode:03d}` gives 159 or 007 |
| `{title}` | Midterm Fallout | if it's blank, the separators around it are tidied away |
| `{date}` | 2026-11-20 | `{date:%b %d}` gives Nov 20 |
| `{yyyy}` `{yy}` | 2026, 26 | |
| `{mm}` `{m}` | 09, 9 | `{m}` has no leading zero |
| `{dd}` `{d}` | 05, 5 | `{d}` has no leading zero |

Track file names (API mode) can also use `{original}` (Riverside's own file name, the default), `{participant}`, `{stem}`, `{ext}`, `{type}`, `{n}`, `{take}` and `{recording}`. Take folder names can use `{take}` (1, 2, 3… in recording order), `{recording}` and the episode fields. The Blackmagic Cloud media location can use `{episode_folder}` and `{raw_folder}`.

For example, the project name `{show} EP {episode} {m}.{d}.{yy}` gives `My Show EP 159 11.20.26`.

## Updates and the About tab

The **About** tab shows which version you have, when it was built, and what's new in it.

Episode Kickoff checks for a new version when it opens. When one is out, an **Update to …** button appears at the top of the window. It takes you to the About tab, where you can read what's new and click **Install and Restart**. The app downloads the update, checks its signature, replaces itself and opens again. Your shows, keys and sign-in stay as they are.

You can check any time with **Check for Updates** on the About tab or **Help → Check for Updates…**, and turn the automatic check off on the About tab.

> [!IMPORTANT]
> Updates only work when Episode Kickoff is in your Applications folder, not when it's opened straight from the installer disk.

## Where your data lives

| What | Where |
|---|---|
| Shows (presets) and app settings | `~/Library/Application Support/Episode Kickoff/` |
| Riverside API keys | your Mac's Keychain |
| Automations | `automations.json` in the folder above; their tokens and secrets are in your Keychain |
| Riverside website sign-in | the app's own browser profile, in the folder above |
| Logs | `~/Library/Logs/Episode Kickoff/`; command and Shortcut automations log to its `automations` folder |

Episode Kickoff has no account and no analytics. It connects to Riverside to get your recordings, to GitHub to check for updates, and to the services your automations use (and, if you click **Install browser support**, downloads a copy of Chromium). The installer contains only the app, never anyone's settings or keys.

## Troubleshooting

- **Help → Check Setup…** shows what's working.
- **"Can't reach DaVinci Resolve"**: open DaVinci Resolve Studio and check that **External scripting using** is set to **Local** ([setup step 1](#1-let-resolve-accept-the-app)).
- **Website mode can't find the buttons**: Riverside may have renamed them. In **Edit Presets… → Website & Downloads**, change the **Click** wording to match the page. Until then, click Download yourself in the app's browser window; anything you download there still lands in `RAW`.
- **macOS won't open the app**: see [Install](#install), step 3. If there's no **Open Anyway** button, run this once in Terminal:

  ```sh
  xattr -dr com.apple.quarantine "/Applications/Episode Kickoff.app"
  ```

- **An automation didn't start**: the log in the main window says why, and **Test** in **File → Automations…** tries it on its own. If a command isn't found, use its full path (in Terminal, `which claude` shows it). Claude and ChatGPT limit how many runs they start in an hour.
- **An update didn't install**: look in `~/Library/Logs/Episode Kickoff/update.log`, and make sure the app is in your Applications folder.
- **Logs**: **File → Open Logs Folder**. **Help → Show detailed log** adds more detail to the log in the window.

## Command line

Optional. The app also runs from Terminal:

```sh
EK="/Applications/Episode Kickoff.app/Contents/MacOS/Episode Kickoff"
"$EK" run -p "My Show" -e 159 --folder ~/Shows/"My Show"/"EP 159" -r <recording ID or link>
"$EK" run -p "My Show" -e 159 --folder ~/Shows/"My Show"/"EP 159" -r <take 1> -r <take 2>
"$EK" run -p "My Show" -e 159 --folder ~/Shows/"My Show"/"EP 159" --source watch --expect 3
"$EK" run -p "My Show" -e 159 --folder ~/Shows/"My Show"/"EP 159" -r <ID> --no-automations
"$EK" recordings -p "My Show"   # recent Riverside recordings (API)
"$EK" check                     # setup check, including your automations
"$EK" update --install          # install a newer version, if there is one
"$EK" presets list|show|export|import
```

## About this project

Episode Kickoff is vibecoded. [Artemis-AV](https://github.com/Artemis-AV) came up with it, decided what it should do and tested it. Claude, Anthropic's AI model, wrote the code, the tests, the installer and update system, and this guide.

**Contributors**

- [Artemis-AV](https://github.com/Artemis-AV): idea, direction and testing
- Claude ([Anthropic](https://www.anthropic.com)): code, tests and documentation

This repository holds the app's installers and updates.
