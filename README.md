# Github Greenary

A small repo that commits to itself about four times a day, at pseudo-random times, fully automated via GitHub Actions.

## How it works

- [`.github/workflows/auto-commit.yml`](.github/workflows/auto-commit.yml) has four `schedule` (cron) triggers spread roughly 6 hours apart.
- Each run first sleeps a random amount (0–59 minutes) so the actual commit time isn't fixed, then picks a random line from a small list of messages, appends it with a UTC timestamp to [`LOG.md`](LOG.md), and pushes the commit using the workflow's built-in `GITHUB_TOKEN`.
- No local machine, cron job, or manual input is needed — it runs entirely on GitHub's servers.

## One-time setup after creating the GitHub repo

1. Push this folder to a new GitHub repo (see setup instructions provided alongside this file).
2. In the repo, go to **Settings → Actions → General → Workflow permissions**, and select **"Read and write permissions"**. Without this, the push step in the workflow will fail with a 403.
3. Optionally trigger it once by hand: **Actions tab → Auto Commit → Run workflow** (this works because of the `workflow_dispatch` trigger), to confirm it runs end-to-end before waiting for the schedule.

## Notes

- GitHub does not guarantee scheduled workflows fire at the exact minute — expect some drift, especially during high load, on top of the intentional random delay.
- GitHub automatically disables scheduled workflows on repos with 60 days of no activity. Since this workflow commits multiple times a day, that won't happen here as long as it keeps running successfully.
