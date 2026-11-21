# Cupitor data template

Create your own Cupitor data repository with **Use this template → Create a new repository**. Keep `cupitor.json` and store language data under `db/language/`.

## Transcribe audio to SRT

This template includes a manual GitHub Actions workflow and all its Python scripts. No transcription API key or access to the original Cupitor repository is required.

1. In your new repository, open **Actions**. If GitHub asks, enable workflows.
2. Select **Transcribe MP3 to SRT**, then **Run workflow**.
3. Enter a direct audio URL or a public Google Drive file share link. Drive files must be shared with **Anyone with the link**.
4. Set the spoken language to `sv`, `en`, `de`, or another supported Whisper language code. Use `auto` for automatic detection.
5. Start the workflow and wait for it to finish.
6. Open the completed run and download the **mp3-transcription** artifact. Extract the ZIP to get the audio and its matching `.srt` file.

The downloader preserves the source filename when available; otherwise it uses `audio.mp3`. The subtitle uses the same base name. Swedish (`sv`) uses KB-Whisper medium; other languages and `auto` use Whisper medium. The first run downloads the model and Python dependencies; later runs can reuse cached files.

Artifacts are kept for **7 days**. Download and save them before they expire. The workflow does not commit generated audio or subtitles to your repository. You can import the downloaded files into Cupitor.

The workflow runs only when manually started, on GitHub-hosted Ubuntu runners using your repository owner's GitHub Actions allowance. GitHub Actions must be allowed by the account or organization that owns the repository. Private audio requiring authentication is not supported.

### Included files

- `.github/workflows/mp3-to-srt.yml` — manual transcription workflow.
- `tools/language/download-audio.py` — direct URL and public Google Drive downloader.
- `tools/language/make-srt.py` — Whisper transcription and SRT output.
- `tools/language/test_download_audio.py` — downloader regression tests, also run by the workflow.

To run the downloader tests locally with Python 3.11 or newer and `curl` installed:

```sh
python -m venv .venv
.venv/bin/python -m pip install gdown==5.2.2
.venv/bin/python -m unittest discover -s tools/language -p test_download_audio.py -v
```

## Template updates

New repositories receive the files present when they are created from this template. Repositories already created from it do not receive later updates automatically. Copy the workflow and its scripts into an existing repository to add transcription there.
