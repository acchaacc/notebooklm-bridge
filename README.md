<div align="center">

# YT NotebookLM Bridge

**A [Claude Code](https://github.com/anthropics/claude-code) Skill that bridges YouTube channels to Google NotebookLM - extract all video URLs from any YouTube channel and generate a ready-to-import source file for NotebookLM**

[![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)](https://www.python.org/)
[![Claude Code Skill](https://img.shields.io/badge/Claude%20Code-Skill-purple.svg)](https://www.anthropic.com/news/skills)
[![yt-dlp](https://img.shields.io/badge/Powered%20by-yt--dlp-red.svg)](https://github.com/yt-dlp/yt-dlp)

> Extract all video URLs from a YouTube channel, format them as a space-separated markdown file, and import directly into Google NotebookLM as a source. Let NotebookLM analyze an entire channel's content in one go.

[Installation](#installation) | [Quick Start](#quick-start) | [How It Works](#how-it-works) | [FAQ](#faq)

</div>

---

## The Problem

You want to use [Google NotebookLM](https://notebooklm.google.com) to analyze and research a YouTube channel's content, but:

- **Manual copy-paste**: You have to copy each video URL one by one into NotebookLM
- **Time-consuming**: Channels with hundreds of videos make this practically impossible
- **Easy to miss**: You might skip important videos when adding them manually
- **No batch import**: NotebookLM doesn't have a native "import entire channel" feature

## The Solution

This Claude Code Skill automates the entire process:

```
YouTube Channel URL → yt-dlp extracts all video URLs → Space-separated .md file → Import into NotebookLM
```

One command, all videos, ready to import.

---

## Installation

### Prerequisites

- [Claude Code](https://github.com/anthropics/claude-code) installed locally
- [yt-dlp](https://github.com/yt-dlp/yt-dlp) installed (`brew install yt-dlp` or `pip install yt-dlp`)

### Install the Skill

```bash
# 1. Create skills directory (if it doesn't exist)
mkdir -p ~/.claude/skills

# 2. Clone this repository
cd ~/.claude/skills
git clone https://github.com/acchaacc/yt-notebooklm-bridge

# 3. That's it! Open Claude Code and say:
"What are my skills?"
```

No virtual environment needed. No additional dependencies. Just `yt-dlp` and Python 3.

---

## Quick Start

### 1. Tell Claude Code what you want

```
"Extract all video URLs from this YouTube channel for NotebookLM: https://www.youtube.com/@ChannelName"
```

### 2. Claude handles everything

- Checks that `yt-dlp` is installed
- Extracts all video URLs from the channel
- Saves them as a space-separated `.md` file on your Desktop

### 3. Import into NotebookLM

1. Open [notebooklm.google.com](https://notebooklm.google.com)
2. Create a new notebook or open an existing one
3. Click **"Add source"**
4. Choose **"Copy and paste text"**
5. Paste the file content
6. NotebookLM will automatically parse all YouTube video links

---

## How It Works

### Architecture

```
~/.claude/skills/yt-notebooklm-bridge/
├── SKILL.md                          # Instructions for Claude
├── README.md                         # This file
├── scripts/
│   └── extract_channel_urls.py       # URL extraction script
├── .gitignore
└── LICENSE
```

### Workflow

```
Phase 1: Environment Check
    ↓
    Verify yt-dlp is installed
    ↓
Phase 2: Extract URLs
    ↓
    yt-dlp --flat-playlist extracts all video URLs
    ↓
Phase 3: Save & Guide
    ↓
    Save as .md file → Guide user to import into NotebookLM
```

### Output Format

The generated `.md` file contains all video URLs separated by spaces:

```
https://www.youtube.com/watch?v=VIDEO_1 https://www.youtube.com/watch?v=VIDEO_2 https://www.youtube.com/watch?v=VIDEO_3 ...
```

This format allows NotebookLM to automatically recognize and process each YouTube video as a source.

---

## Script Usage

### Basic Usage

```bash
python3 scripts/extract_channel_urls.py "https://www.youtube.com/@ChannelName"
```

### With Options

```bash
# Limit to 50 most recent videos
python3 scripts/extract_channel_urls.py "https://www.youtube.com/@ChannelName" --max-videos 50

# Custom output path
python3 scripts/extract_channel_urls.py "https://www.youtube.com/@ChannelName" --output ~/Desktop/my_channel.md
```

### Supported URL Formats

| Format | Example |
|--------|---------|
| Handle | `https://www.youtube.com/@ChannelName` |
| Channel ID | `https://www.youtube.com/channel/UCXXXXXXX` |
| Custom URL | `https://www.youtube.com/c/ChannelName` |

### Parameters

| Parameter | Description | Default |
|-----------|-------------|---------|
| `channel_url` | YouTube channel URL (required) | - |
| `--max-videos` | Maximum number of videos to extract | All |
| `--output` | Output file path | `~/Desktop/<channel_name>_notebooklm.md` |

---

## Common Commands

| What you say in Claude Code | What happens |
|-----------------------------|--------------|
| *"Extract YouTube channel videos for NotebookLM: [URL]"* | Extracts all videos and saves to Desktop |
| *"Get the latest 50 videos from [URL] for NotebookLM"* | Extracts 50 videos with `--max-videos` |
| *"Create a NotebookLM source from [channel URL]"* | Same as extract |

---

## FAQ

**Why space-separated instead of one URL per line?**
NotebookLM's text source parser recognizes URLs separated by spaces. This format ensures all videos are detected as individual sources when pasted.

**How long does extraction take?**
Depends on the channel size. Most channels (< 500 videos) finish within 1-2 minutes. Very large channels may take longer.

**Is there a limit on how many videos I can extract?**
No limit from this tool. Use `--max-videos` if you want to limit the number.

**Does this work with playlists?**
This skill is designed for channels, but `yt-dlp` can handle playlist URLs too. Just pass a playlist URL instead.

**What if yt-dlp is not installed?**
The skill will detect this and tell you how to install it: `brew install yt-dlp` or `pip install yt-dlp`.

---

## Troubleshooting

| Problem | Solution |
|---------|----------|
| `yt-dlp` not found | `brew install yt-dlp` or `pip install yt-dlp` |
| No videos extracted | Check that the URL is a channel page, not a single video |
| Extraction timeout | Use `--max-videos` to limit the number |
| Permission denied | Check file write permissions on Desktop |

---

## Limitations

- **yt-dlp required**: Must be installed separately (not bundled)
- **Public videos only**: Cannot extract private or unlisted videos
- **Network dependent**: Requires internet connection for extraction
- **NotebookLM limits**: NotebookLM has its own limits on number of sources per notebook

---

## License

MIT License - See [LICENSE](LICENSE) for details.

---

<div align="center">

Built as a Claude Code Skill for bridging YouTube channels to Google NotebookLM

**One command. All videos. Ready to research.**

```bash
cd ~/.claude/skills
git clone https://github.com/acchaacc/yt-notebooklm-bridge
```

</div>
