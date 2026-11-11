# Coding progress

Live dashboard of progress on the meta-analysis coding sheets: **https://soriclucija.github.io/coding-progress/**

It shows, overall and for each coder:

- **Papers coded**: unique Zotero `key`s with a `date_coded` value, out of all unique keys in the sheet
- **Remaining**: papers without a `date_coded` yet
- **Groups coded**: rows belonging to coded papers (one paper can have several groups/rows)

The page updates automatically about once an hour while the maintainer's computer is on. The time of the last update is shown at the bottom of the page.

## How it works

The coding sheets live in synced cloud folders (OneDrive / Nextcloud), one folder per coder. Every hour, a scheduled task on the maintainer's computer:

1. finds the most recently modified `coding-XX*.xlsx` in each folder. Because of sync conflicts, the newest file is not always the one with the cleanest name, so the file name is ignored apart from the `coding-` prefix, and Excel lock files (`~$...`) are skipped
2. counts progress with R (`readxl`)
3. renders the dashboard with Quarto (`tracker.qmd` → `index.html`)
4. appends the numbers to a local log (`progress-log.csv` / `.xlsx`)
5. commits and pushes `index.html` to this repo, which GitHub Pages serves

Only `index.html` and this README are published. The source, folder paths, file names and the log stay local (see `.gitignore`).

## Maintaining it

All files are in the local `coding-tracker` folder.

| File | Purpose |
|---|---|
| `tracker.qmd` | Settings (coder folders, colours, column names) and the dashboard |
| `styles.css` | Look of the page |
| `update.bat` | Renders the page and pushes it if anything changed |
| `run-hidden.vbs` | Runs `update.bat` without opening a window (used by the scheduled task) |
| `progress-log.csv` / `.xlsx` | Local history: one row per coder per update, incl. file used and last-edited time |
| `last-render.log` | Output of the last run; check this first if the page stops updating |

**Update now:** double-click `update.bat`.

**Add a coder:** add their folder to `folders` at the top of `tracker.qmd` (order = Coder #1, #2, ...) and add a colour to `palette` if there are more coders than colours.

**Change the columns used:** edit `key_col` and `date_col` in `tracker.qmd`.

**Scheduled task:** created once in Command Prompt with

```
schtasks /create /tn "coding-tracker" /sc hourly /tr "wscript.exe \"<path-to>\coding-tracker\run-hidden.vbs\""
```

Remove it with `schtasks /delete /tn "coding-tracker" /f`.

**Requirements:** R with `readxl`, `htmltools` and `writexl`; Quarto; Git (signed in to GitHub).

---

© Lucija Soric
