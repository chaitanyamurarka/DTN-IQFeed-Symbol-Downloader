# Repository Setup Instructions

Follow these steps to set up the DTN Symbol Downloader repository with GitHub Actions automation.

## 1. Create Repository Structure

Create the following directory structure in your repository:

```
dtn-symbol-downloader/
├── .github/
│   └── workflows/
│       └── download-symbols.yml
├── dtn_symbol_downloader.py
├── requirements.txt
├── README.md
├── LICENSE
├── .gitignore
└── SETUP.md
```

## 2. Add Files

1. Copy the Python script (`dtn_symbol_downloader.py`) to the root directory
2. Place `download-symbols.yml` in `.github/workflows/`
3. Add all other files (requirements.txt, README.md, etc.) to the root

## 3. GitHub Repository Settings

### Enable GitHub Actions

1. Go to Settings → Actions → General
2. Under "Actions permissions", select "Allow all actions and reusable workflows"
3. Under "Workflow permissions", select "Read and write permissions"
4. Check "Allow GitHub Actions to create and approve pull requests"

### Configure Releases (Optional)

If you want automatic releases:
1. Go to Settings → Actions → General
2. Ensure "Allow GitHub Actions to create and approve pull requests" is checked

## 4. First Run

### Manual Trigger

1. Go to the Actions tab
2. Select "Download DTN Symbols"
3. Click "Run workflow"
4. Optionally set parameters:
   - Resume from batch: Leave empty for fresh start
   - Create release: Check if you want a release

### Verify Schedule

The workflow will run automatically:
- Every 12 hours at 00:00 and 12:00 UTC
- Times in different t