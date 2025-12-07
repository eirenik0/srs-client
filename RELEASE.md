# Releasing

This document describes how to release a new version of `srs-client`.

## Quick Start

```bash
# For a patch release (0.2.0 → 0.2.1)
make release

# For a minor release (0.2.0 → 0.3.0)
make release.minor

# For a major release (0.2.0 → 1.0.0)
make release.major

# Preview changes without executing
make release.dry
```

## Prerequisites

- Ensure you have `cargo-release` and `git-cliff` installed:
  ```bash
  cargo install cargo-release git-cliff
  ```
- Clean working directory (all changes committed)
- On the `main` branch

## Automated Release (Recommended)

Using `cargo-release` which handles everything automatically:

### Patch Release
```bash
make release      # Same as make release.patch
make release.patch
```
- Bumps patch version (0.2.0 → 0.2.1)
- Updates changelog
- Updates README.md version references
- Creates git tag
- Pushes to GitHub
- Publishes to crates.io

### Minor Release
```bash
make release.minor
```
- Bumps minor version (0.2.0 → 0.3.0)
- Same automation as patch release

### Major Release
```bash
make release.major
```
- Bumps major version (0.2.0 → 1.0.0)
- Same automation as patch release

### Dry Run
```bash
make release.dry
```
- Preview all changes without executing
- Safe way to verify what will happen

## Manual Release

If you prefer more control over the process:

### Complete Manual Release
```bash
make release.manual
```
This will guide you through:
1. Checking for uncommitted changes
2. Prompting for new version number
3. Updating changelog with `git-cliff`
4. Committing changes
5. Creating git tag
6. Pushing to GitHub with tags
7. Reminder to publish to crates.io

### Step-by-Step Manual Release
```bash
# 1. Check working directory is clean
make release.check-changes

# 2. Update version (will prompt)
make release.update-version

# 3. Update changelog
make release.update-changelog

# 4. Commit changes
make release.commit

# 5. Create tag
make release.tag

# 6. Push to GitHub
make release.push

# 7. Publish to crates.io
make publish
```

## Post-Release

After release:
- The CI/CD pipeline automatically runs tests on the tagged version
- Documentation is built and validated
- The release appears on GitHub releases and crates.io

## Configuration

Release settings are configured in `Cargo.toml`:
- Version format and update rules
- Changelog generation with `git-cliff`
- README.md version updates
- Allowed release branch (`main`)

## Troubleshooting

### Release fails halfway
- Check the error message
- Fix the issue
- Run `make release.dry` to verify
- Try the release again

### Need to redo a release
```bash
# Delete the failed tag
git tag -d v0.2.1
git push origin :refs/tags/v0.2.1

# Start over
make release
```

### Working directory not clean
```bash
# Commit or stash changes
git add .
git commit -m "WIP"

# Or stash
git stash
```

Remember to always run `make release.dry` first if you're unsure about the changes!