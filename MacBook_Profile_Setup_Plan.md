# MacBook Pro Multi-Profile Setup Plan

## BLUF
- **Admin**: sterile toolbox. No personal data. Only installs, updates, fixes.
- **Work**: full dev + AI + comms while you're at sea. Messages on. Everything you need to build and ship.
- **Home**: family life, finance, Photos, YouTube. Messages off. Minimal dev, just viewers/editors.
- **One Apple ID** everywhere, but different iCloud toggles per profile so iCloud doesn't turn your laptop into a junk drawer.

---

## 1. Exact iCloud, Apps, and Rules per Profile

### Admin (Captain's Key)
**Purpose:** install, update, backup, recover. Then get out.

**Settings**
- Apple ID: signed in for App Store only  
- iCloud Drive: Off  
- Messages/FaceTime/Mail/Photos: Off  
- FileVault: On  
- Rapid Security Responses: Auto  

**Apps**
- App Store, System Settings, Disk Utility  
- Carbon Copy Cloner  
- Time Machine  
- OnyX  
- AppCleaner  
- iStat Menus  
- Little Snitch / Radio Silence  
- TG Pro  
- Mactracker  
- Sensei  

**Rules**
- Install apps to `/Applications` only  
- No browsing, downloads, or clutter  

---

### Work (Chief Engineer)
**Purpose:** coding, AI builds, ship docs, research, creative. Private, everything allowed.

**Settings**
- Apple ID: On  
- iCloud Drive: On, but Desktop & Documents **OFF**  
- iCloud toggles: Keychain, Notes, Reminders, Calendar, Messages ON; Photos OFF  
- Comms: Messages, FaceTime, Mail ON  
- Calendar: Separate Work/Home calendars  
- Notifications: "At Sea" Focus mode for full comms  

**Apps**
- VS Code, Cursor, Claude Code, ChatGPT  
- Adobe Suite, MS Office  
- Homebrew, iTerm2 / Warp, Xcode CLT  
- Postman, TablePlus, Notion  
- DaVinci Resolve / Final Cut, Pixelmator / Affinity  
- PDF Expert, Raycast, Rectangle, Tailscale  
- Topaz Video/Photo AI  

**Folders**
```
~/Projects            (active repos; always local)
~/Toolchains          (SDKs, venvs, nvm/pnpm caches)
~/Docs_Work           (draft reports, turnovers, manuals you edit)
~/Reference_Manuals   (read-only PDFs; big ones on OWC)
```

**Storage Rules**
- Small transfers: iCloud Drive › Bridge/Inbox  
- Large data: OWC/Work_Archive or Mac mini  

---

### Home (Family Bridge)
**Purpose:** family docs, finance, photos, media. No risky dev.

**Settings**
- Apple ID: On  
- iCloud Drive: On with Desktop/Documents **ON**  
- iCloud toggles: Photos ON (Optimize Storage), Messages OFF, Notes/Calendar/Reminders ON  
- Comms: FaceTime optional  
- Calendar: Home default; Work view-only  
- Notifications: "Shared Home" Focus blocks social/Messages  

**Apps**
- Safari / Arc  
- MS Office, Numbers / Pages / Keynote  
- PDF Expert  
- Music / TV  
- Notion (household pages)  
- 1Password  
- Preview  

**Rules**
- No dev tools; if needed, switch to Work  

---

## 2. Privacy & Notifications
- **Work profile:** "At Sea" Focus — Messages, News, Scores, FaceTime allowed.  
- **Home profile:** "Shared Home" Focus — block social, Slack, Messages.  
- **Mail:** active in both, VIP filtering in Home.  
- **Lock screen:** previews only when unlocked.  
- **Family account (optional):** no Apple ID, safe for others to use.  

---

## 3. Storage Layout
**Local (MacBook)**  
- `~/Projects`, `~/Toolchains`, `~/Docs_Work`, `~/Documents` (active only)

**iCloud Drive (Bridge)**  
- Bridge/Inbox for small transfers  
- Family_Docs for synced content  
- Photos optimized in Home

**OWC (Cold + Backup)**  
- APFS Vol 1: Time Machine (encrypted)  
- APFS Vol 2: Cold_Archive
```
/Cold_Archive/
  Work_Archive/
    Projects_Archived/
    Reference_Manuals/
    Video_Renders/
  Home_Archive/
    Taxes/
    Receipts/
    Yearly_Photo_Exports/
```

**Mac Mini**
- Host databases, AI models, heavy dev builds  

---

## 4. Dev Environment Isolation
- Homebrew installed system-wide; only sourced in Work shell.  
- SSH keys generated only in Work: `ssh-keygen -t ed25519 -C "work-m4pro"`  
- Node/Python managers only in Work (`nvm`, `pyenv`)  
- Docker optional; use Colima or remote mini builds  

---

## 5. Backup & Cleanup
**Automations**
- Time Machine to OWC (both profiles)  
- CCC weekly clone in Admin  
- Monthly archive ritual to Cold_Archive  
- Smart Folders: Files >2GB, Downloads >14 days  
- Empty Trash after 30 days  

**Example Cleanup Commands**
```bash
# Move old downloads to archive
find ~/Downloads -type f -mtime +14 -exec mv -n {} /Volumes/OWC/Cold_Archive/Downloads_Archive/ \;

# Archive stale repos (>60 days)
cd ~/Projects && for d in */.git; do
  repo="${d%/.git}"
  if [ -z "$(git --git-dir="$d" log --since='60 days ago' --pretty=oneline 2>/dev/null)" ]; then
    tar -czf "/Volumes/OWC/Cold_Archive/Work_Archive/Projects_Archived/${repo%/}_$(date +%Y%m%d).tgz" "$repo"
  fi
done
```

---

## 6. Setup Sequence
1. Create users: Admin, Work (Standard), Home (Standard)  
2. **Admin:** update macOS, FileVault, Rapid Security, OWC formatted for TM + Archive  
3. **Work:** Apple ID on, iCloud tuned, install dev + creative tools, create folders, Focus "At Sea"  
4. **Home:** Apple ID on, iCloud full sync, personal apps only, Focus "Shared Home"  
5. **Bridge:** iCloud Drive › Bridge/Inbox; OWC › Cold_Archive structure  

---

## 7. Sanity Checks
- Require Touch ID for passwords/Notes.  
- Minimal menubar.  
- Enable Fast User Switching.  
- Spotlight excludes "Developer" categories in Home.  

---

## 8. Cheat Sheet

**Work only:** brew, VS Code, Cursor, SSH keys, Postman, TablePlus, Claude/ChatGPT apps, Tailscale, SORA tools, PDF Expert, Notion (workspaces), Messages, FaceTime, Mail  
**Home only:** Photos, Music/TV, personal Notion pages, Office, finance docs, Messages off  
**Admin only:** CCC, TM, iStat, TG Pro, Little Snitch, OnyX, Sensei  
**Shared (Bridge):** small file handoffs only
