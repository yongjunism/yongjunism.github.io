---
layout: post
title: "M2 Mac Mini에서 homebrew를 통해 git 설치하기"
subtitle: "How to install Git through Homebrew on M2 Mac Mini"
category: dev
tags: M2 MacMini homebrew git
---

M2 Mac Mini에 git을 설치하려고 합니다.
brew.sh에서 내려받아도 되지만, 결국 cli 환경에서 환경변수를 추가해주어야 합니다.
저는 터미널에서 homebrew를 먼저 설치하고, 환경변수를 추가하겠습니다. 


```bash
cd /opt
```

```bash
sudo mkdir homebrew
```

```bash
sudo chown -R $(whoami) /opt/homebrew
```

```bash
curl -L https://github.com/Homebrew/brew/tarball/master | tar xz --strip 1 -C homebrew
```

```bash
echo "export PATH=/opt/homebrew/bin:$PATH" >> ~/.zshrc
source ~/.zshrc
```

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"\
```

```bash
brew install git
```
