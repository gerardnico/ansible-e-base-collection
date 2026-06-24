# Release

## Steps

* Change the version in [galaxy.yml](../galaxy.yml)
* Create the commit
```bash
git commit -m "chore: release 1.0.1"
git push origin main
```
* create the tag
```bash
# no v required by ansible to work with >=
git tag -a 1.0.1 -m "Release 1.0.1"
git push origin 1.0.1
```
* create the release
```bash
gh release create 1.0.1 \
  --title "1.0.1" \
  --generate-notes
```