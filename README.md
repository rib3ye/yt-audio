# yt-audio

Download YouTube audio in the highest quality the **macOS Music app** plays back.

It grabs the best available audio stream and produces an `.m4a` (AAC) file with
title/artist/album metadata, chapter markers, and cover art embedded — the format
Music handles natively. When YouTube offers a native AAC stream it's **remuxed with
no re-encoding** (zero quality loss); other sources (e.g. Opus) are transcoded to
AAC at highest VBR quality.

Need something else? `-f` also writes `mp3`, `opus`, `flac`, `alac`, or `wav` — each
always at the highest quality that format allows.

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
# Single track → ~/Downloads/
./yt-audio "https://www.youtube.com/watch?v=jNQXAC9IVRw"

# Download and import straight into the Music app
./yt-audio -a "https://www.youtube.com/watch?v=..."

# Pick the output directory
./yt-audio -d ~/Music/Imports "https://www.youtube.com/watch?v=..."

# Pick the output format (default: m4a)
./yt-audio -f mp3 "https://www.youtube.com/watch?v=..."

# Grab a whole playlist (off by default — a single video is downloaded otherwise)
./yt-audio --playlist "https://www.youtube.com/playlist?list=..."
```

### Options

| Flag | Description |
|------|-------------|
| `-d, --dir DIR` | Output directory (default: `$YT_AUDIO_DIR` or `~/Downloads`) |
| `-f, --format FMT` | Output format: `m4a`, `mp3`, `opus`, `flac`, `alac`, `wav` (default: `$YT_AUDIO_FORMAT` or `m4a`) |
| `-a, --add` | Add each finished file to the Music app |
| `--playlist` | Allow downloading a full playlist (default: single video) |
| `-h, --help` | Show help |

Set a permanent default directory or format with environment variables:

```sh
export YT_AUDIO_DIR="$HOME/Music/Imports"
export YT_AUDIO_FORMAT=mp3
```

### Formats

Every format is encoded at the highest quality it allows. When YouTube's stream is
already in the target codec, it's copied with no re-encoding.

| Format | What you get | Plays in Music |
|--------|--------------|----------------|
| `m4a` (default) | Native AAC stream, copied; otherwise AAC at highest VBR | yes |
| `mp3` | 320 kbps CBR, the MP3 ceiling | yes |
| `opus` | Native Opus stream, copied; otherwise 256 kbps | no |
| `flac` | Lossless FLAC | no |
| `alac` | Apple Lossless in an `.m4a` file | yes |
| `wav` | Uncompressed 16-bit PCM, no cover art (WAV has no slot for it) | yes |

`--add` refuses `opus` and `flac`, since Music can't play them. YouTube's source is
always lossy, so `flac`, `alac`, and `wav` preserve that source exactly — they can't
add quality back, they just avoid a second lossy encode.

## Put it on your PATH (optional)

Copy it into a directory on your PATH with `install` (sets executable perms in one step).
`~/.local/bin` is the conventional spot for personal scripts:

```sh
install -m 755 yt-audio ~/.local/bin/yt-audio
# then from anywhere:
yt-audio "https://www.youtube.com/watch?v=..."
```

If `~/.local/bin` isn't already on your PATH, add it (e.g. in `~/.zshrc`):
`export PATH="$HOME/.local/bin:$PATH"`.

This copies the script, so re-run the install after editing it to update the installed
copy. To uninstall: `rm ~/.local/bin/yt-audio`.

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
