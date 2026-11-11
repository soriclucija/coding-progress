# coding-progress

Rendered output of the [Coding progress](https://soriclucija.github.io/coding-progress/) dashboard, which tracks progress on the meta-analysis coding sheets of the coders I supervise at Tilburg University.

Built with [Quarto](https://quarto.org) and R, and hosted on GitHub Pages.

## Getting started

### Prerequisites

- [Quarto](https://quarto.org/docs/get-started/)
- R with `readxl`, `htmltools` and `writexl`
- [Git](https://git-scm.com/), signed in to GitHub
- Positron (optional), use your preferred IDE

### Installation

The source (`tracker.qmd`, `styles.css`, `update.bat`, `run-hidden.vbs`) is kept locally because it contains local folder paths. Only `index.html` and this README are pushed to this repository.

### Usage

Build the dashboard:

```bash
quarto render tracker.qmd
```

Build and publish (renders, then commits and pushes `index.html` if anything changed):

```bash
update.bat
```

The output is written to `index.html`, which GitHub Pages serves from the `main` branch.

### How progress is counted

For each coder, the most recently modified `coding-XX*.xlsx` in their folder is used, regardless of sync-conflict names such as `coding-XX (2) Copy.xlsx`.

- **Papers coded:** unique `key` values with a `date_coded` entry, out of all unique keys
- **Groups coded:** rows belonging to coded papers

Each update also appends these values, plus the file used and its last-edited time, to a local log (`progress-log.csv` / `progress-log.xlsx`) that is not published.

### Schedule

A Windows scheduled task runs `update.bat` every hour:

```bash
schtasks /create /tn "coding-tracker" /sc hourly /tr "wscript.exe \"<path-to>\coding-tracker\run-hidden.vbs\""
```

To add a coder, edit the `folders` list at the top of `tracker.qmd` (order = Coder #1, #2, ...).

## Author

Lucija Sorić, Tilburg University