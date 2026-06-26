# yt-audio

Download YouTube audio in the highest quality the **macOS Music app** plays back.

It grabs the best available audio stream and produces an `.m4a` (AAC) file with
title/artist/album metadata, chapter markers, and cover art embedded — the format
Music handles natively. When YouTube offers a native AAC stream it's **remuxed with
no re-encoding** (zero quality loss); other sources (e.g. Opus) are transcoded to
AAC at highest VBR quality.

## Install

The script needs [`yt-dlp`](https://github.com/yt-dlp/yt-dlp) and `ffmpeg`. If either is
missing when you run it, `yt-audio` offers to install them with Homebrew for you:

```
yt-audio: missing dependencies: yt-dlp ffmpeg
Install them now with "brew install yt-dlp ffmpeg"? [y/N]
```

Or install them up front yourself:

```sh
brew install yt-dlp ffmpeg
```

The script itself needs no install — just run it.

## Usage

```sh
./yt-audio <youtube-url> [more-urls...]
```

Examples:

```sh
# Single track → ~/Music/YouTube/
./yt-audio "https://www.youtube.com/watch?v=jNQXAC9IVRw"

# Download and import straight into the Music app
./yt-audio -a "https://www.youtube.com/watch?v=..."

# Pick the output directory
./yt-audio -d ~/Music/Imports "https://www.youtube.com/watch?v=..."

# Grab a whole playlist (off by default — a single video is downloaded otherwise)
./yt-audio --playlist "https://www.youtube.com/playlist?list=..."
```

### Options

| Flag | Description |
|------|-------------|
| `-d, --dir DIR` | Output directory (default: `$YT_AUDIO_DIR` or `~/Music/YouTube`) |
| `-a, --add` | Add each finished file to the Music app |
| `--playlist` | Allow downloading a full playlist (default: single video) |
| `-h, --help` | Show help |

Set a permanent default directory with an environment variable:

```sh
export YT_AUDIO_DIR="$HOME/Music/Imports"
```

## Put it on your PATH (optional)

```sh
ln -s "$PWD/yt-audio" /opt/homebrew/bin/yt-audio
# then from anywhere:
yt-audio "https://www.youtube.com/watch?v=..."
```

## Notes

- **Why `.m4a`/AAC?** Music plays AAC natively and YouTube serves a native AAC
  stream, so it's used as-is with no transcoding. That's the highest fidelity
  obtainable for Music without a pointless lossy→lossy conversion. (YouTube's Opus
  stream is sometimes a slightly higher bitrate, but Music can't play Opus, and
  transcoding it to AAC would lose more than it gains.)
- Cover art is embedded via `mutagen`, which ships with yt-dlp — no extra tools needed.
- Keep yt-dlp current (`brew upgrade yt-dlp`); YouTube changes break older versions.

## License

[MIT](LICENSE) © Noah Tsutsui
