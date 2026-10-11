# NEXDRIVE
**Personal File Explorer & Media Server**  
Creator branding: **Created By Shiv Yogi**

NEXDRIVE is a GitHub Pages frontend paired with a Python Flask server running on your Android phone. Your files stay on your device. GitHub Pages only hosts static frontend files.

## Current status
This repository contains a starter implementation and UI. Before exposing it to the internet, finish the security and integration checklist below. Do not treat a successful frontend load as proof that the backend is secure or fully production-ready.

## Project structure
- `index.html`: application shell and animated branding.
- `assets/css/style.css`: responsive Liquid Glass styling.
- `assets/js/app.js`: frontend navigation and API integration.
- `server/app.py`: Flask API, login, upload/download/stream endpoints and local reset command.
- `server/config.example.json`: example configuration. Copy to `config.json` and customize it.
- `server/auth.py`: scrypt password hashing and authentication decorator.
- `server/storage_scanner.py`: bounded scan of configured root folders.

## Run on Pydroid 3
1. Copy the `NEXDRIVE` folder to a location accessible in Pydroid 3.
2. Open Pydroid terminal and install dependencies:
   `pip install -r server/requirements.txt`
3. Copy `server/config.example.json` to `server/config.json`.
4. Edit `server/config.json`:
   - Add the exact GitHub Pages origin, such as `https://YOUR-USERNAME.github.io`.
   - Configure the actual internal/SD/USB NEXDRIVE folder paths.
   - Keep only folders you are authorized to access.
5. Start the backend:
   `cd NEXDRIVE/server`
   `python start.py`
6. On first start, create a strong password in the terminal.
7. The website UI currently needs a login screen/API flow to call `/api/login` before protected file actions. Do not expose the backend publicly until that integration is completed and tested.

## Password reset policy
There is intentionally no website password-change or forgot-password page.

The intended local-only command is `forget-pass` in the interactive server terminal while it is running. The command listener depends on the process having an interactive TTY. Some Pydroid execution modes do not provide a TTY or route typed input to a running process; in that case, this starter cannot receive the command live. Do not add a public reset endpoint as a workaround. Implement and test a suitable local-only control channel for your exact Pydroid workflow before relying on this feature.

## GitHub Pages
1. Create a new GitHub repository named `NEXDRIVE`.
2. Upload `index.html` and the `assets/` directory to the repository root.
3. In repository Settings → Pages, publish from the `main` branch and `/ (root)`.
4. The frontend will load, but it cannot access local files unless the Python backend and a reachable network/tunnel endpoint are running.

## Important security and integration notes
- The starter UI does not yet include a login form even though the backend has `/api/login`. Add a login flow before using protected APIs.
- The frontend's streaming URL and backend response handling should be integration-tested with authenticated sessions. Cross-origin cookies require HTTPS, exact-origin CORS, `credentials: include`, and correct cookie policy.
- Do not commit `server/config.json`, password data, tunnel tokens, session secrets, or private logs. `.gitignore` is not a way to erase secrets already committed.
- The default backend host is `127.0.0.1`, which is local-only. A tunnel must be configured separately and safely for remote access.
- SD/USB access depends on Android/Pydroid permissions and actual mount paths.
- The current file scanner lists files directly inside configured roots; nested folder browsing and cursor pagination need further backend work.
- File metadata is refreshed on request. No full media files are intentionally loaded into RAM.
- HTTP Range, authentication, uploads, disconnect handling, and memory behavior must be tested on the target Android device.

## Before remote deployment
- [ ] Add and test the login screen.
- [ ] Test `forget-pass` in the actual Pydroid terminal mode.
- [ ] Verify all protected endpoints require authentication.
- [ ] Verify the exact Pages origin and HTTPS tunnel behavior.
- [ ] Test HTTP Range seeking with large media.
- [ ] Test upload limits, conflicts, interrupted uploads, and storage errors.
- [ ] Test malicious path inputs.
- [ ] Verify secrets are not in Git history.
- [ ] Measure RAM during idle, scan, upload, and playback.
- [ ] Test on a second device over the intended network.
