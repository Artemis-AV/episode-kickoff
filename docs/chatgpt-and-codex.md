# ChatGPT and Codex

Part of the [Episode Kickoff user guide](../README.md).

Episode Kickoff can hand each finished episode to ChatGPT or to Codex, OpenAI's agent that runs on your Mac. There are three ways to connect them. Pick the one that fits the job:

| | [Codex on your Mac](#codex-on-your-mac) | [ChatGPT workspace agent](#chatgpt-workspace-agent) | [ChatGPT app, through a shortcut](#chatgpt-app-through-a-shortcut) |
|---|---|---|---|
| Runs | On your Mac, in the episode folder | In ChatGPT, in the cloud | On your Mac, in the ChatGPT app |
| Can open the episode's files | Yes | No | No |
| Can work in DaVinci Resolve | Yes, with a Resolve MCP server | No | No |
| You need | ChatGPT Plus, Pro, Business, Edu or Enterprise, or an OpenAI API key | A ChatGPT Business, Enterprise or Edu workspace | The ChatGPT app for Mac, on any plan |
| Good for | Work on the episode itself: notes, checks, files, Resolve | Work in the apps connected to your workspace, such as trackers, docs and channels | Short text, such as a message to your team |

Whichever you choose, the agent gets your instructions, followed by the episode's details: the show, episode number, title and date, the episode folder and its `RAW` folder, each take's files and Resolve bin, and the Resolve project. See [What the agent gets](../README.md#what-the-agent-gets).

## Codex on your Mac

Codex works in the episode folder on your Mac and uses the Codex usage included with your ChatGPT plan.

### 1. Install Codex and sign in

1. Open Terminal and install Codex with one of these commands:

   ```sh
   curl -fsSL https://chatgpt.com/codex/install.sh | sh   # OpenAI's installer
   brew install --cask codex                              # or Homebrew
   npm install -g @openai/codex                           # or npm
   ```

2. Run `codex`, choose **Sign in with ChatGPT**, and finish signing in in your browser. Then type `/exit` to leave Codex.
3. Check that Codex runs on its own, the way Episode Kickoff runs it:

   ```sh
   echo "Reply with the word ready." | codex exec --skip-git-repo-check -
   ```

   After a few seconds, it should print `ready`.

### 2. Optional: let Codex work in DaVinci Resolve

Codex works in Resolve through an MCP server for Resolve: a small program that gives Codex tools to read and change Resolve projects. DaVinci Resolve Studio 21.1 and later comes with one: in Resolve, choose **File → Setup AI Assistants** and set up Codex instead of step 2. For older versions, [davinci-resolve-mcp](https://github.com/samuelgursky/davinci-resolve-mcp) is a popular open-source one, and its installer can add it to Codex for you. Other Resolve MCP servers work too.

1. In DaVinci Resolve Studio, open **Preferences → System → General** and set **External scripting using** to **Local**.
2. Install the server by following its instructions. If they don't add it to Codex, add it yourself, using the command from its instructions that starts the server:

   ```sh
   codex mcp add resolve -- <the command that starts the server>
   ```

3. Run `codex mcp list`. The server should be listed as enabled. Note its name: you need it in the next part.

Resolve has to be open while Codex works in it. Episode Kickoff opens the new project before it starts the automations, so it usually is.

### 3. Add the automation

1. In Episode Kickoff, open **File → Automations…**, click **New** and choose **Codex on this Mac**.
2. Keep the command it fills in:

   ```sh
   codex exec --skip-git-repo-check --sandbox workspace-write -
   ```

   | Part | What it does |
   |---|---|
   | `codex exec` | Runs Codex without its interactive screen. It quits when the job is done. |
   | `--skip-git-repo-check` | Lets Codex work in an episode folder, which isn't a Git repository |
   | `--sandbox workspace-write` | Codex's own commands can only change files in the episode folder (and temporary folders), and can't use the internet. MCP servers, such as Resolve's, aren't limited by this. |
   | `-` | Codex reads the instructions and episode details that Episode Kickoff sends it |

   If Codex should work in Resolve, use this command instead, with your server's name from `codex mcp list` in place of `resolve`:

   ```sh
   codex exec --skip-git-repo-check --sandbox workspace-write -c 'mcp_servers.resolve.default_tools_approval_mode="approve"' -
   ```

   The extra part lets Codex use the server's tools without asking first. Nobody is there to say yes during an automation, so Codex would otherwise cancel any tool call that needs your OK. It applies only to this automation: when you use Codex yourself, it still asks.

   If the server's name has a space in it, such as `DaVinci Resolve`, the extra part doesn't work. Use the first command, then open `~/.codex/config.toml` in a plain-text editor, find the server's section, and add the line `default_tools_approval_mode = "approve"` to it. That way Codex doesn't ask first in your own sessions either.

3. Under **Instructions**, write what Codex should do. See the [examples](#example-instructions).
4. Under **Runs for**, tick your shows. Leave **Only after a run with no problems** and **Only once per episode** on.
5. Click **Test**. Codex gets a test message saying there's nothing to work on, so it just replies, and its reply shows next to **Test**. **Show Activity** shows each step it took. Then click **Save**.

### After each episode

- When a run for one of those shows finishes, Episode Kickoff starts Codex in the episode folder with your instructions and the episode's details.
- Codex works in the background, even if you quit Episode Kickoff, and your Mac doesn't go to sleep on its own until Codex is done. If Episode Kickoff is still open when Codex finishes, you get a notification with the first line of its answer.
- In the main window, **Show Activity** next to **Automations…** shows what Codex is doing while it works: each command it runs, each file it changes, each Resolve tool it uses, and its answer at the end (if more than one automation ran, the button is **Open Results…**). **File → Automation Activity…** shows it too, and **Stop** ends a run that's going the wrong way.
- To see what Codex did and ask for more, open its session in Terminal. Click **Open Episode Folder**, type `cd ` (with a space) in Terminal, drag the episode folder from Finder into the Terminal window, and press Return. Then run:

  ```sh
  codex resume --last --include-non-interactive
  ```

### Options

- **Use an OpenAI API key instead of your ChatGPT plan**: next to **Secret variable**, type `CODEX_API_KEY` as the **Name** and paste the key as the **Value**. Episode Kickoff keeps it in your Mac's Keychain and gives it to Codex only when this automation runs.
- **Let Codex's commands use the internet**: add `-c sandbox_workspace_write.network_access=true` before the final `-`.
- **Choose the model**: add `-m <model>` before the final `-`.

### If Codex doesn't run

- **`command not found: codex`** (exit code 127): Episode Kickoff couldn't find Codex. In Terminal, run `which codex`, and put the full path it shows at the start of the command.
- **`Not inside a trusted directory and --skip-git-repo-check was not specified`**: the command is missing `--skip-git-repo-check`.
- **Codex says you're not signed in**: run `codex login status` in Terminal. If you're signed out, run `codex` and sign in again, or use an API key (see [Options](#options)).
- **`invalid transport` in `mcp_servers.…`**: the server name in the command doesn't match the one in `codex mcp list`.
- **Nothing happened in Resolve**: check that Resolve is open, that **External scripting using** is **Local**, and that `codex mcp list` shows the server. If the log says a tool call was cancelled, use the Resolve command from [Add the automation](#3-add-the-automation).
- **Codex couldn't save a file**: it can only change files in the episode folder. Ask it to save there.

## ChatGPT workspace agent

For ChatGPT Business, Enterprise and Edu workspaces. A workspace agent runs in ChatGPT's cloud and works with the apps connected to it, such as Google Drive or Slack. It can't open files on your Mac or use Resolve.

1. **Turn it on.** A workspace admin opens **Admin → Permissions & roles** in ChatGPT, enables **Workspace agents**, and turns on **Allow users to create personal access tokens**.
2. **Build the agent.** In ChatGPT, click **Agents** in the sidebar and create an agent. In its instructions, explain what to do with a message from Episode Kickoff: it starts with your automation's instructions, followed by the episode's details as JSON. Connect the apps it needs, then publish it.
3. **Add an API channel.** In the agent builder, add an **API** channel and copy its trigger ID, which starts with `agtch_`.
4. **Make an access token.** In ChatGPT, open **Admin → Access tokens**, create a token with the **Workspace Agents** scope, and copy it. You must be able to run the agent yourself: if someone else built it, ask them to share it with you.
5. **Add the automation.** In Episode Kickoff, open **File → Automations…**, click **New** and choose **ChatGPT workspace agent**. Paste the **Trigger ID** and the **Access token**, add instructions if you like, tick your shows under **Runs for**, click **Test**, then **Save**.

After each run, **Open in ChatGPT** next to **Automations…** in the main window opens the agent's conversation, where its answer appears.

## ChatGPT app, through a shortcut

Works with any ChatGPT plan, and is best for short text. ChatGPT gets the episode's details as text, not its files.

1. Install the ChatGPT app for Mac and sign in.
2. In the Shortcuts app, make a shortcut, for example **Episode message**, with these actions:
   1. **Get Dictionary from Input**: Episode Kickoff gives the shortcut the episode's details as a JSON file.
   2. **Get Dictionary Value**, for the key `text`: the ready-made message, with your instructions and the episode's details.
   3. **Ask ChatGPT** (one of the ChatGPT app's actions), with that value as what to ask.
   4. An action that uses the answer, such as **Create Note**, **Send Message** or **Copy to Clipboard**.
3. In Episode Kickoff, open **File → Automations…**, click **New** and choose **Shortcut**. Pick the shortcut, write what ChatGPT should do under **Instructions**, and tick your shows under **Runs for**.
4. Click **Test**, then **Save**. If the shortcut asks for permission the first time it runs, allow it, so later runs don't stop to ask.

If ChatGPT is turned on in **System Settings → Apple Intelligence & Siri**, you can use the **Use Model** action with the ChatGPT model instead of **Ask ChatGPT**.

## Other ways

**Webhook**: Zapier, Make and n8n can start from a webhook and pass its `text` field to a ChatGPT or OpenAI step. In Episode Kickoff, click **New**, choose **Webhook**, and paste the webhook's URL. If the service needs a key, enter it under **Secret header**.

## Example instructions

Notes for the edit (Codex):

```text
Make notes.md in the episode folder with:
- the show, episode number, title and date
- a section for each take that lists its files
- a checklist for the edit: sync, comps, audio, main cut, versions, renders
Don't change, move or delete any other file.
```

A check in Resolve (Codex with a Resolve MCP server):

```text
Open the Resolve project named in the details. Check that each take's bin has a clip for every file in that take, and that none are offline. Write what you found to resolve-check.md in the episode folder. Don't change the project.
```

A message to your team (shortcut or workspace agent):

```text
Write a two-line message telling the team this episode is set up in Resolve and ready to edit. Include the show, the episode number and the Resolve project's name.
```

Tips:

- Say exactly what to make and where, for example "notes.md in the episode folder".
- Say what not to touch, for example "Don't move, rename or delete anything in RAW".
- To try new instructions on the last episode, use **Run → Run an Automation**.

> [!WARNING]
> Codex runs with your Mac user's permissions. The commands above keep Codex's own commands inside the episode folder, without internet access. A Resolve MCP server can do anything its tools allow in Resolve, and the Resolve command lets Codex use them without asking. Only loosen these settings if you trust what your instructions ask for.
