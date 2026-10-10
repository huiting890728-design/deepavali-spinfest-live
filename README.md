# Deepavali SpinFest — shared live game

## Requirements
Node.js 18+; no external packages required.

## Start locally
macOS/Linux:
```sh
cd Deepavali-SpinFest-Live
ADMIN_PASSWORD='choose-a-strong-password-here' npm start
```
Windows PowerShell:
```powershell
cd Deepavali-SpinFest-Live
$env:ADMIN_PASSWORD='choose-a-strong-password-here'
npm start
```
Open http://localhost:3000 . Click **Admin** and log in with your password.

## Share with other laptops
Deploy this entire Node.js project to a host that supports a persistent Node server and a persistent disk, set the `ADMIN_PASSWORD` environment variable, enable HTTPS, and share its public URL. A static HTML host (such as Netlify Drop) is **not** sufficient. Keep the server process running throughout the event. The host must preserve `data.json` across restarts; ephemeral hosting will lose event results.

For a same-Wi-Fi trial, start the server on the organiser laptop and share `http://<organiser-laptop-LAN-IP>:3000` with other devices on that Wi-Fi, if permitted by firewall/network policy. Use HTTPS for internet deployment.

## Features
- All visitors share one game and one event state using Server-Sent Events.
- Public visitors can initiate a spin; the server prevents simultaneous spins and chooses results securely.
- Admin-only password-protected configuration, quiz answer reveal, grading, reset and CSV export.
- Weighted random, guaranteed allocation, predetermined result modes. The active mode is visible to everyone.
- 17 bilingual Deepavali questions; answers are only sent to public devices after admin reveal.
- Event results are written to `data.json` and survive server restart. Spin in progress after restart is not automatically replayed.

## Operational limitations / production hardening
This is a functional shared-game starter, **not a production-hardened public deployment**. It uses in-memory admin sessions, a single Node process, a JSON-file store, and public unauthenticated spin requests. Anyone with the public link can trigger a spin; distribute the URL carefully or add participant login / a host-enabled spin control before using at a real 200-person event. Add rate limits, CSRF protection, HTTPS, backups, server monitoring, and stronger transaction handling for public internet deployment. Do not run multiple server instances against the same JSON file. Predetermined outcomes must be disclosed as such; they are not random draws.

## Live Admin Control Panel upgrade
The organiser can pause/resume the event, change the global game mode, probabilities, prize inventory, participant list and spin duration between spins, and set/remove a special outcome configuration for a future spin. Use the **Next / future spin override** section to configure a single spin. Overrides are logged, and completed results are immutable. Click **Refresh audit log** to inspect recent changes.

**Important:** If editing while others are spinning, wait until the current spin ends. The server rejects changes during an active spin. Editing the participant list during an event can change who is assigned to upcoming spin numbers; avoid reordering it mid-event. This is still a single-server prototype; public participants can start spins unless the organiser pauses the game.
