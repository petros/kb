# Automatically apply Git config to all repos under a folder

## Goal

Make every Git repo under `~/Development` automatically use these settings:

```ini
[user]
    email = petros@example.com
[commit]
    gpgsign = false
```

---

## 🪜 Steps

### Create a folder-specific `.gitconfig`

```bash
cat > ~/Development/.gitconfig <<'EOF'
[user]
    email = petros@example.com
[commit]
    gpgsign = false
EOF
```

This defines the Git settings you want applied automatically.

---

### Tell Git to include this config for all repos under that folder

> !NOTE
> zsh expands `**` unless quoted — so we must quote the pattern.

```bash
git config --global 'includeIf.gitdir:~/Development/**.path' ~/Development/.gitconfig
```

This adds the following section to your global `~/.gitconfig`:

```ini
[includeIf "gitdir:~/Development/**"]
    path = ~/Development/.gitconfig
```

Now, every Git repo (including submodules) inside `~/Development/` automatically inherits the `.gitconfig` file above.

---

### Confirm it works

From any repo under `~/Development`, run:

```bash
git config --show-origin user.email
git config --show-origin commit.gpgsign
```

Expected output:

```
file:/Users/petros/Development/.gitconfig   user.email=petros@example.com
file:/Users/petros/Development/.gitconfig   commit.gpgsign=false
```

---

### Handling local overrides (optional)

Local config (`--local`) always overrides included or global config.
If you previously set a local override and want to remove it:

```bash
git config --unset --local user.email
git config --unset --local commit.gpgsign
```

Then the inherited values from `~/Development/.gitconfig` will take effect.

---

## Notes

- Works for **all nested repos and submodules** under `~/Development/`.
- Safe to edit later with:
  ```bash
  git config --global --edit
  ```
- Does **not** affect repos outside that path.

