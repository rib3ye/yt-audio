# yt-audio

Download YouTube audio for the **macOS Music app**.

By default, yt-audio saves `.m4a` (AAC) files to `~/Downloads`, with available
metadata, chapters, and cover art. Other [formats](#formats) are available with `-f`.

## Install

Install with [Homebrew](https://brew.sh) from the [rib3ye tap](https://github.com/rib3ye/homebrew-tap):

```sh
brew install rib3ye/tap/yt-audio
```

Homebrew installs `yt-dlp`, `ffmpeg`, and `atomicparsley` automatically.

To update, run `brew upgrade yt-audio yt-dlp`.
To uninstall, run `brew uninstall yt-audio`.

## Usage

```sh
yt-audio <youtube-url> [more-urls...]
```

Examples:

```sh
# Single track → ~/Downloads/
yt-audio "https://www.youtube.com/watch?v=jNQXAC9IVRw"

# Download and import straight into the Music app
yt-audio -a "https://www.youtube.com/watch?v=..."

# Pick the output directory
yt-audio -d ~/Music/Imports "https://www.youtube.com/watch?v=..."

# Pick the output format (default: m4a)
yt-audio -f mp3 "https://www.youtube.com/watch?v=..."

# Grab a whole playlist (off by default — a single video is downloaded otherwise)
yt-audio --playlist "https://www.youtube.com/playlist?list=..."
```

### Options

| Flag | Description |
|------|-------------|
| `-d, --dir DIR` | Output directory (default: `$YT_AUDIO_DIR` or `~/Downloads`) |
| `-f, --format FMT` | Output [format](#formats) (default: `$YT_AUDIO_FORMAT` or `m4a`) |
| `-a, --add` | Add each finished file to the Music app |
| `--playlist` | Allow downloading a full playlist (default: single video) |
| `-h, --help` | Show help |

To save your defaults, add these environment variables to `~/.zshrc`:

```sh
export YT_AUDIO_DIR="$HOME/Music/Imports"
export YT_AUDIO_FORMAT=mp3
```

## Formats

| Format | What you get |
|--------|--------------|
| `m4a` (default) | Native AAC, or conversion to AAC at highest VBR quality |
| `mp3` | 320 kbps CBR |
| `alac` | Apple Lossless in an `.m4a` file |
| `wav` | Uncompressed 16-bit PCM, without cover art |

All listed formats support `--add`. For `m4a`, yt-audio copies native AAC streams
without re-encoding.

YouTube audio is lossy. The `alac` and `wav` formats avoid another lossy
encode, but cannot restore lost quality.

## Manual PATH setup (optional)

To install manually, run these commands from the repository directory:

```sh
brew install yt-dlp ffmpeg
mkdir -p ~/.local/bin
install -m 755 yt-audio ~/.local/bin/yt-audio
```

If `~/.local/bin` is absent from your `PATH`, add this line to `~/.zshrc`:

```sh
export PATH="$HOME/.local/bin:$PATH"
```

Open a new terminal to use `yt-audio` from any directory.

After changes to the script, repeat the `install` command to update the copy.
To uninstall the manual copy, run `rm ~/.local/bin/yt-audio`.

## License

[MIT](LICENSE) © Noah Tsutsui
