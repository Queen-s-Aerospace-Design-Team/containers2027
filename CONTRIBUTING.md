# Contributing

`main` is protected: every change lands via a pull request with at least one approving review — no direct pushes, including from repo admins.

## Workflow

1. **Branch off `main`**, named `<yourname>/<short-description>` or `fix/...` / `feature/...`.
   ```bash
   git checkout main
   git pull origin main
   git checkout -b yourname/short-description
   ```
2. **Commit and push**, then open a pull request into `main`.
3. **Get one approval**, then merge.

## Before opening a PR touching a Dockerfile

Actually rebuild and test the image locally — don't just eyeball the diff:
```bash
./build.sh <folder> local      # e.g. ./build.sh devcontainer-px4 local
```
This never touches GHCR (no credentials needed) and loads straight into your own Docker daemon, so you can point a local devcontainer at `:local` to verify before anyone publishes anything.

Only someone with a `GHCR_TOKEN` (`write:packages` scope for the org) runs `./build.sh <folder> push` to actually publish `:latest` — do that after merge, not as part of the PR itself, so what gets published always matches reviewed, merged code.

If you change a Dockerfile's `LABEL`/user setup/similar metadata-only instructions, keep them positioned **after** the expensive build steps (PX4-Autopilot compile, package installs) rather than near the top — an early `LABEL`/`ENV` change invalidates every cached layer after it, forcing a full rebuild for what should be a one-line edit.
