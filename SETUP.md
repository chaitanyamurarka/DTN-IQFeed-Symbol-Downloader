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
- Times in different timezones:
  - 00:00 UTC = 8:00 PM EST (previous day) / 5:00 PM PST (previous day)
  - 12:00 UTC = 8:00 AM EST / 5:00 AM PST

## 5. Monitoring

### Check Workflow Status

1. Go to Actions tab
2. Click on a workflow run to see details
3. Download artifacts from successful runs

### Email Notifications

GitHub will send email notifications for:
- Failed workflows
- First successful workflow after a failure

### Issues on Failure

The workflow automatically creates an issue if the download fails.

## 6. Customization

### Change Schedule

Edit `.github/workflows/download-symbols.yml`:

```yaml
schedule:
  # Run every 6 hours
  - cron: '0 */6 * * *'
  
  # Run once daily at 3 AM UTC
  - cron: '0 3 * * *'
  
  # Run Monday and Thursday at 2 PM UTC
  - cron: '0 14 * * 1,4'
```

### Adjust Retention

Change artifact retention period:

```yaml
retention-days: 30  # Keep for 30 days instead of 7
```

### Disable Releases

Remove or comment out the "Create Release" step if you don't want automatic releases.

## 7. Data Access

### Via GitHub UI

1. Go to Actions → Select a workflow run
2. Scroll to "Artifacts" section
3. Download the compressed files

### Via GitHub API

```bash
# List artifacts
curl -H "Authorization: token YOUR_GITHUB_TOKEN" \
  https://api.github.com/repos/USERNAME/REPO/actions/artifacts

# Download specific artifact
curl -L -H "Authorization: token YOUR_GITHUB_TOKEN" \
  https://api.github.com/repos/USERNAME/REPO/actions/artifacts/ARTIFACT_ID/zip \
  -o symbols.zip
```

### Via Releases

If releases are enabled, find them at:
`https://github.com/USERNAME/REPO/releases`

## 8. Troubleshooting

### Workflow Not Running

- Check Actions are enabled in repository settings
- Verify the workflow file is in `.github/workflows/`
- Check workflow syntax with GitHub's workflow editor

### Download Failures

- Check the issue created by the workflow
- Look at workflow logs for specific error
- Try manual run with resume option

### Storage Concerns

- Artifacts are automatically deleted after retention period
- Each run creates ~173 MB of data
- Consider shorter retention or less frequent runs if needed

## 9. Best Practices

1. **Monitor First Runs**: Watch the first few automated runs to ensure they complete successfully

2. **Set Up Notifications**: Configure GitHub notifications to alert you of failures

3. **Regular Cleanup**: The workflow automatically cleans up batch files, but monitor your artifact storage

4. **Version Control**: Tag stable versions of the script for reference

5. **Documentation**: Update README.md with any customizations you make

## 10. Security Notes

- The script doesn't require authentication
- No sensitive data is stored
- All data is publicly available from DTN
- Consider repository visibility based on your needs

---

For more help, check the [GitHub Actions documentation](https://docs.github.com/en/actions) or create an issue in the repository.