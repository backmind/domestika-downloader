# Node JS Tool to download full courses from Domestika

This script is a simple way to download a full course from Domestika.

> **Warning**
> You need to own the course you want to download. So you either have to have bought it or got it for "free" with your premium account.

## Requirements

- Node.js
- [N_m3u8DL-RE](https://github.com/nilaoda/N_m3u8DL-RE/releases) binary
- ffmpeg (required for H.265 transcoding and codec detection)

## Installation

### 1. Install N_m3u8DL-RE

Download the latest version from [N_m3u8DL-RE releases](https://github.com/nilaoda/N_m3u8DL-RE/releases). Scroll down to the 'Assets' section and download the appropriate binary for your machine:

| OS | Architectures |
|----|---------------|
| Windows | arm64, x64 |
| MacOS | arm64, x64 |
| Linux | arm64, x64 |

Unzip the file and place the binary in this repo's root folder.

> **Note:** On Windows the file is `N_m3u8DL-RE.exe`, on MacOS/Linux it's `N_m3u8DL-RE`. Do not rename.

### 2. Install ffmpeg

ffmpeg is required for H.265 transcoding and codec detection.

**Windows:**
```bash
winget install ffmpeg
```

**MacOS:**
```bash
brew install ffmpeg
```

**Linux (Debian/Ubuntu):**
```bash
sudo apt install ffmpeg
```

Verify installation:
```bash
ffmpeg -version
ffprobe -version
```

### 3. Install Node dependencies
```bash
npm i
```

## Configuration

Open `index.js` and configure the following variables:
```javascript
const course_urls = ['URL_1', 'URL_2'];  // Array of course URLs
const subtitle_lang = 'en';
const transcode_to_hevc = true;
const use_nvenc = true;
const max_concurrent_processes = 7;
const machine_os = 'win';  // 'win' or 'mac'
const cookies = [{ name: '_domestika_session', value: 'YOUR_COOKIE', domain: 'www.domestika.org' }];
const _credentials_ = 'YOUR_CREDENTIALS';
```

### Configuration Options

| Variable | Description | Default |
|----------|-------------|---------|
| `course_urls` | Array of course URLs to download | — |
| `subtitle_lang` | Subtitle language code (must be available) | `'en'` |
| `transcode_to_hevc` | Enable H.265 transcoding after download | `true` |
| `use_nvenc` | Use GPU (NVENC) for transcoding; `false` uses CPU (libx265) | `true` |
| `max_concurrent_processes` | Maximum parallel downloads/transcodes | `7` |
| `machine_os` | `'win'` for Windows, `'mac'` for MacOS/Linux | — |

### Getting Cookies and Credentials

1. Install the [Cookie-Editor](https://chrome.google.com/webstore/detail/cookie-editor/) browser extension
2. Log into Domestika
3. Open Cookie-Editor and find:
   - `_domestika_session` → copy Value into cookies array
   - `_credentials_` → copy Value into `_credentials_` variable

### Course URL Format

URLs must point to the course content page (ending in `/course`):
```
https://www.domestika.org/en/courses/3086-creating-animated-stories-with-after-effects/course
```

## Usage
```bash
npm run start
```

Output location: `domestika_courses/{coursename}/`

### Transcoding Behavior

When `transcode_to_hevc` is enabled:
- Videos are checked via ffprobe before transcoding
- Files already in H.265/HEVC are skipped
- Original files are replaced with transcoded versions

NVENC requires an NVIDIA GPU with encoding support. If unavailable, set `use_nvenc = false` to use CPU encoding (slower but universal).

## Troubleshooting

### Permission denied on MacOS/Linux
```bash
chmod +x N_m3u8DL-RE
```

### NVENC errors

If you see NVENC-related errors and don't have an NVIDIA GPU:
```javascript
const use_nvenc = false;
```

### Transcode failures

Ensure ffmpeg and ffprobe are in your PATH:
```bash
which ffmpeg
which ffprobe
```

## Special Thanks

Special thanks to [@Cybasaint](https://www.github.com/Cybasaint) for helping with the project and giving me access to his domestika account for testing.