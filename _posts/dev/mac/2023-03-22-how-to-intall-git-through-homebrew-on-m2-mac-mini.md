---
layout: post
title: "Apple Silicon(M1, M2, M3)에 homebrew 설치하기"
subtitle: "How to install Homebrew on Apple Silicon(M1, M2, M3)"
category: dev
tags: mac
---
Apple Silicon(M1, M2, M3)에 homebrew를 설치하는 방법입니다.  
인텔 맥에 설치할 때는 [brew.sh](https://brew.sh/) 들어가자마자 보이는 아래 명령어를 그대로 실행했는데요.  

```
$ /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"\
```

M2 Mac Mini에는 바로 적용하니 **zsh command not found brew**라는 에러가 발생합니다.  
homebrew의 기본 경로가 달라서 발생하는 것으로, 어차피 환경변수를 추가해야하니 터미널만을 이용해서 homebrew를 설치하겠습니다.  

## 설치 방법

### 1. 먼저 root 권한을 받아옵니다.

```
$ sudo -i
```

### 2. /opt 경로에 homebrew 폴더를 생성하고 소유자를 변경합니다.

```
$ cd /opt
$ mkdir homebrew
$ sudo chown -R $(whoami) /opt/homebrew
```

### 3. curl 명령어로 homebrew를 받아와서 압축을 해제합니다.

```
$ curl -L https://github.com/Homebrew/brew/tarball/master | tar xz --strip 1 -C homebrew
```

### 4. 환경변수를 추가 및 적용합니다.

```
$ echo 'export PATH="/opt/homebrew/bin:$PATH"' >> ~/.zshrc
$ source ~/.zshrc
```

### 5. [brew.sh](https://brew.sh/)에 있던 명령어를 다시 실행합니다.

```
$ /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"\
```

### 6. 잘 설치되었는지 brew 버전을 확인합니다.

```
$ brew --version
```


다음 포스팅에서는 homebrew로 git을 설치하고, 로컬과 연동해보도록 하겠습니다.
