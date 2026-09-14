# screen2gif — One-liner screen recording → GIF/MP4

Record any window (or full screen) on Windows straight to GIF or MP4 using only ffmpeg's built-in `gdigrab`. No screen-recording software, no dependencies beyond ffmpeg.

## Usage

```powershell
# Record a specific window by title (recommended — avoids capturing your desktop)
.\record.ps1 -Title "YourAppWindow" -Seconds 8

# Full screen (fallback)
.\record.ps1 -FullScreen -Seconds 8

# High-clarity MP4 instead of GIF (for README embedding)
.\record.ps1 -Title "YourAppWindow" -Seconds 15 -Format mp4
```

## record.ps1

```powershell
param(
  [string]$Title,
  [switch]$FullScreen,
  [int]$Seconds = 8,
  [ValidateSet('gif','mp4')][string]$Format = 'gif',
  [string]$Out = '.\demo'
)
$ErrorActionPreference = 'Stop'
$scale = 'scale=1000:-1,fps=10'
if ($Format -eq 'mp4') {
  $vf = 'scale=1000:-1,fps=15'; $enc = '-c:v libx264 -preset veryfast -crf 28 -pix_fmt yuv420p'
} else { $enc = '' }

if ($FullScreen) { $src = 'desktop' } else { $src = "title=`"$Title`"" }

if ($Format -eq 'gif') {
  ffmpeg -f gdigrab -framerate 10 -i $src -t $Seconds -vf "$scale" "$Out.gif"
} else {
  ffmpeg -f gdigrab -framerate 15 -i $src -t $Seconds -vf "$vf" $enc.Split(' ') "$Out.mp4"
}
Write-Host "Saved: $Out.$Format"
```

## Field-tested numbers

| Input | Output | Size |
|-------|--------|------|
| 2s desktop | GIF 1000w, 10fps | **303 KB** |
| 15s window | MP4 crf28 | **1.41 MB** |

## Tips

- **GIF size control**: width 800–1000, 8–10 fps, 6–10 s duration → keep under **1–3 MB** (GitHub compresses >5 MB).
- **Burn step numbers** into the recording with `drawtext` ("① click here") — cheaper than editing subtitles later.
- Recording by exact window title avoids leaking your desktop into the demo.

## License

MIT + no-resale clause (see repo LICENSE).
