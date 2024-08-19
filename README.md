# KMP Github Actions (Kactions)

*Kactions* is a set of scripts written to provide easy-to-configure and ready-to-use compiler actions.
The aim is to allow new delevopers of Kotlin Multiplatform, to deploy their applications and libraries without much effort.
---
## Quick Start

add a `.yml` file in the directory `.github/workflows`. The *.github* folder must be in the root directory.

Copy and paste this code to the `.yml` file, based on your use case:
### Library Deploy to maven central.

```yaml
# MIT Licence
# Copyright 2024 feraxhp


name: Publish and Build library
run-name: 🚀 ¡Release! 🚀
on:
  workflow_dispatch:
  push:
    branches: [ "main" ]

jobs:
  library:
    name: S1
    uses: feraxhp/kactions/.github/workflows/publish.yml@v1.0.0
    with:
      module: ':ktheme' # << Your library module
    secrets:
      SIGNING_KEY: ${{ secrets.SIGNING_KEY }}
      MAVEN_CENTRAL_USERNAME: ${{ secrets.MAVEN_CENTRAL_USERNAME }}
      MAVEN_CENTRAL_PASSWORD: ${{ secrets.MAVEN_CENTRAL_PASSWORD }}
      SIGNING_KEY_ID: ${{ secrets.SIGNING_KEY_ID }}
      SIGNING_KEY_PASSWORD: ${{ secrets.SIGNING_KEY_PASSWORD }}

```

Add these secrets in your repository:

#### SIGNING_KEY_ID

Please run `gpg --version` to make sure if you already have it.

1. Install A GPG key manager for your system.
    - Linux: Depends on your distro
      - Ubuntu: `sudo apt-get install gnupg`
      - Fedora 40: Already install
      - Arch: [See Docs](https://wiki.archlinux.org/title/GnuPG)
    - Windows: [Download](https://www.gpg4win.org/)
    - MacOs: 
      1. Already install
      2. `brew install gpg`
2. Run this command to generate a gpg key:
    - `gpg --gen-key`
    - The password provided in this step is your `SIGNING_KEY_PASSWORD`
    - Copy your public key and store it on a safe place
    - You can see again your public key by running `gpg --list-keys`
3. Then you have to distribute your public key
    - `gpg --keyserver keyserver.ubuntu.com --send-keys <YOUR_PUBLIC_KEY>`
    - Additional servers:
      - `keyserver.ubuntu.com`
      - `keys.openpgp.org`
      - `pgp.mit.edu`
4. The last 8 Digits of your public key are your 'SIGNING_KEY_ID' 
#### SIGNING_KEY_PASSWORD
See the steps for SIGNING_KEY_ID >> 2 >> 2(-)
#### SIGNING_KEY
1. Generate a private ket with this command:
    - `gpg --armor --export-secret-keys <GPG_PUBLIC_KEY_ID> | cat -`
    - Copy it and store it on a save place
    - This is your `SIGNING_KEY` (Also call private key) 
#### MAVEN_CENTRAL_USERNAME & MAVEN_CENTRAL_PASSWORD
1. Register your self in [Maven Central](https://central.sonatype.com/?smo=true)
2. Register a namespace (if you're logging with GitHub. your name space must be set automatically)
3. Got to manage account (Your email, in the top right corner, then, view account)
4. Click on *Generate User Token* it must generate this xml:
```xml
<server>
 <id>${server}</id>
 <username>MAVEN_CENTRAL_USERNAME</username>
 <password>MAVEN_CENTRAL_PASSWORD</password>
</server>
```

### Publish Wasm or Js App to GitHub Pages

```yaml
# MIT Licence
# Copyright 2024 feraxhp


name: Publish Wasm / Js App to pages
run-name: 🚀 ¡Deploy to pages! 🚀
on:
  workflow_dispatch:
  push:
    branches: [ "main" ]

jobs:

  pages:
    name: S2
    uses: feraxhp/kactions/.github/workflows/pages.yml@v1.0.0
    with:
      platform: 'Wasm' # << Or 'Js'
      module: ':sample:composeApp'

```

---
## Road Map

### Compile (Actions)

- [x] KmpLibrary
- [x] Wasm App
- [x] Js App
- [ ] Android App << Work in progress
- [ ] Desktop App
  - [ ] Linux
  - [ ] Windows
  - [ ] MacOs
- [ ] Ios App

### Deployments (workflows)
- [x] Deploy to Pages
  - [x] Wasm
  - [x] Js
- [x] Deploy to Maven central
---
