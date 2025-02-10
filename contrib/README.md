# CONTRIB

## Checkout out

```bash
git clone ~/.ansible/collections/ansible_collections/ans_e/ans_e_base
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