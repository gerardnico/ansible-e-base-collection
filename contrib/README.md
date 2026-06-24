# CONTRIB

## Local checkout out

```bash
CLONE_DIR=~/code/gerardnico/ansible-e-base-collection
git clone https://github.com/gerardnico/ansible-e-base-collection $CLONE_DIR
# install
COLLECTION_DIR=~/.kubee/collections/ansible_collections
rm -rf "$COLLECTION_DIR/ans_e"
mkdir -p "$COLLECTION_DIR/ans_e"
ln -s $CLONE_DIR "$COLLECTION_DIR/ans_e/ans_e_base"
```
Then in a requirement file:
```yaml
collections:
  - name: ans_e.ans_e_base # for local development only
```

## How to manage a image

From the project directory
```bash
source Dockerfiles/molecule-debian/12.8/.envrc
# then whatever command
dock-x build
dock-x run
dock-x stop
```

## How to start a test

* [molecule-run](molecule-run)