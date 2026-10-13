# Before You Arrive: Laptop Setup for the AAIF Dallas Hands-On Workshop

**Saturday, October 31, 2026, 1:00 to 4:30 PM CDT, Richardson, TX. Doors open at 12:45 PM.**
**Trainer: David Berry**

This is a hands-on workshop. You will build with an AI agent on your own laptop, so please set it up before you arrive. Setup takes about 20 minutes on a laptop you control. If you get stuck, the troubleshooting section at the end covers the common problems.

**You need three things working before you walk in:**

1. **goose** installed (the desktop app, the command-line tool, or both).
2. **A model** goose can use: a cloud AI account or a model running on your laptop.
3. **A passing smoke test**, which proves goose can use tools on your machine.

---

## Step 0: Check your laptop (2 minutes)

- **You must be allowed to install software.** Many company laptops block installs, block command-line tools, or inspect web traffic in ways that break AI tools. If your laptop is managed by an employer, try the install this week, not on the day. If it fails, bring a personal laptop or plan to pair with someone.
- **Operating system:** macOS, Windows 10/11, or Linux.
- **Disk space:** about 1 GB for goose. If you plan to run a model locally, allow 5 to 10 GB more.
- **Bring your charger.** Outlets may be limited.

---

## Step 1: Install goose (5 to 10 minutes)

goose is an open-source AI agent, now hosted by the Agentic AI Foundation. It comes as a **desktop app** and a **command-line tool (CLI)**. Both share the same settings. **If you are not sure which to choose, install the desktop app.**

Official instructions: https://goose-docs.ai/docs/getting-started/installation

### macOS

**Desktop app:** download it from the installation page above, unzip it, and open the app. Or, with Homebrew:

```bash
brew install --cask block-goose
```

**CLI:** with Homebrew:

```bash
brew install block-goose-cli
```

Or with the install script:

```bash
curl -fsSL https://github.com/aaif-goose/goose/releases/download/stable/download_cli.sh | bash
```

### Windows

**Desktop app:** download the Windows version from the installation page above, unzip it, and run the app.

**CLI, in PowerShell:**

```powershell
Invoke-WebRequest -Uri "https://raw.githubusercontent.com/aaif-goose/goose/main/download_cli.ps1" -OutFile "download_cli.ps1"
.\download_cli.ps1
```

**CLI, in Git Bash** (recommended by the goose project if you already have Git for Windows):

```bash
curl -fsSL https://github.com/aaif-goose/goose/releases/download/stable/download_cli.sh | bash
```

If PowerShell refuses to run the script, your laptop's execution policy is blocking it. That is the kind of company restriction to find out about now, not at the workshop.

### Linux

**Desktop app (Debian/Ubuntu):** download the `.deb` from the installation page above, then:

```bash
sudo dpkg -i <downloaded-file>.deb
```

**CLI:**

```bash
curl -fsSL https://github.com/aaif-goose/goose/releases/download/stable/download_cli.sh | bash
```

### Check it worked

CLI users: open a new terminal and run `goose --version`. Desktop users: the app should open to a welcome screen.

---

## Step 2: Connect a model (5 minutes)

goose needs a model to think with. **Pick one option below.** If you can, set up a second one as a backup.

The first time you open goose, it walks you through choosing a provider. CLI users can run `goose configure` at any time and choose **Configure Providers**.

| You have... | Choose this in goose | What you need |
|---|---|---|
| An Anthropic API account | **Anthropic** | An API key from console.anthropic.com |
| A Claude Code subscription | **Claude ACP** (an Agent Client Protocol provider) | Node.js, then `npm install -g @agentclientprotocol/claude-agent-acp` |
| A Google account | **Google Gemini** | A free API key from Google AI Studio |
| A ChatGPT Plus or Pro subscription | **ChatGPT Subscription** (some menus list it as **ChatGPT Codex**) | You sign in through your browser. No key needed. |
| An OpenAI API account | **OpenAI** | An API key from platform.openai.com |
| GitHub Copilot | **GitHub Copilot** | You sign in through a device code. No key needed. |
| AWS Bedrock | **Amazon Bedrock** | AWS credentials set **before** you start goose (`AWS_PROFILE` and `AWS_REGION`) |
| Google Vertex AI | **GCP Vertex AI** | Google Cloud credentials set up in advance, plus `GCP_PROJECT_ID` and `GCP_LOCATION` |
| A model on your laptop | **Ollama** | See "Running a model locally" below |

**A common trap:** a chat-app subscription (Claude.ai, Gemini app) is not the same thing as an API key. Check the table for what your plan actually supports. If you use a pay-as-you-go API key, **set a spending limit** in your provider's console. A workshop like this usually costs a few dollars.

### Running a model locally (optional)

This option keeps everything on your laptop. It is slower, and small models are noticeably weaker at agent work than cloud models. **Set up a cloud option as a backup.**

1. Install Ollama from https://ollama.com.
2. Download a model that supports tool calling. With 16 GB of RAM or more:
   ```bash
   ollama pull qwen3:8b
   ```
   `llama3.1:8b` also works. Laptops with 8 GB of RAM will struggle; use a cloud option instead.
3. In goose, choose **Ollama** as the provider. Your downloaded models appear in the list.
4. Recommended: agents send long instructions, and a short context window cuts them off. If your machine has the memory, start Ollama with a larger window:
   ```bash
   OLLAMA_CONTEXT_LENGTH=16384 ollama serve
   ```

---

## Step 3: Smoke test (3 minutes)

This test proves goose can act on your machine, not just chat.

1. **Make an empty folder for the workshop**, for example `aaif-workshop` in your home folder. Do all workshop work there.
2. Start goose in that folder:
   - **CLI:** `cd` into the folder, then run `goose session`.
   - **Desktop:** start a new session and point it at that folder.
3. Paste this prompt:

   > Create a file called hello-aaif.txt in this folder containing today's date and the name of the model you are running on. Then read the file back to me.

**Pass:** goose creates the file, and the file is really in your folder. **You are ready.**

**Fail:** goose answers in chat but no file appears. That usually means your model does not support tool calling. Switch to a cloud provider, or to a model from the list above.

---

## Before the workshop: work safely

- **goose runs commands with your user privileges.** By default, the Developer extension uses Autonomous mode, and the empty workshop folder does not sandbox it from other accessible files. Before each session, switch to **Manual Approval** (`/mode approve` in the CLI or the mode selector in Desktop) and review every tool call. If the laptop contains sensitive files or credentials, use a separate OS account or disposable VM.
- **Do not paste secrets, customer data, or employer information** into any prompt.
- **During the workshop we will look at agent traces** (a step-by-step record of what an agent did). You will get an address to send yours to. By default, goose's trace export **does not include the content** of your prompts. We will talk about why that default matters.

---

## Troubleshooting

| Problem | Fix |
|---|---|
| **Windows: "goose is not recognized"** | goose installs to `%USERPROFILE%\.local\bin`. Add that folder to your PATH, then open a new terminal. In Git Bash: `echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc` then `source ~/.bashrc` |
| **Windows: errors saving your API key** | When `goose configure` asks, choose **not** to store the key in the keyring. Or set the key as an environment variable, for example `OPENAI_API_KEY`, and run `goose configure` again. |
| **Mac: desktop app opens with no window** | goose needs read and write access to `~/.config`. Fix that folder's permissions and relaunch. |
| **Windows WSL: install fails while extracting** | Run `sudo apt update && sudo apt install bzip2 -y`, then rerun the install. |
| **Certificate or SSL errors** | Your network or company laptop is probably inspecting traffic. Try another network. If it still fails, bring a different laptop. |
| **Local model is very slow or ignores tools** | Use a cloud provider for the workshop. Local models are a trade-off, and we will discuss it. |

**Still stuck?** Come at 12:45 PM when doors open, and say so at check-in. If your laptop cannot run goose, we will pair you with someone. You will still get the full workshop.

---

## Checklist

- [ ] My laptop lets me install software
- [ ] goose is installed and opens
- [ ] A model is connected (plus a backup if possible)
- [ ] The smoke test created `hello-aaif.txt` in my workshop folder
- [ ] If I use a paid API key, I set a spending limit
- [ ] Charger packed
