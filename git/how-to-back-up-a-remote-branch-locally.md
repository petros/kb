# How to back up a remote branch locally

```bash
# 1. Create a backup tag from the current remote branch
git fetch origin
git tag backup/alekos origin/alekos

# 2. Do risky work + force push if needed
git checkout alekos
# ... changes ...
git push --force-with-lease origin alekos

# 3. To restore old state if things go wrong
git checkout alekos
git reset --hard backup/alekos
git push --force-with-lease origin alekos
```
