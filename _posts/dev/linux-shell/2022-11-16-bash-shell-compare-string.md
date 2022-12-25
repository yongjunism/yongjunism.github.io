---
layout: post
title: "Shell Script에서 문자열 비교, 포함 확인하기"
subtitle: "Git: Rename folder to lowercase"
category: dev
tags: linux shell 
---

Bash 쉘 스크립트에서 두 문자열이 같은지 비교하고, 특정 문자열을 포함하는지 확인하는 법을 살펴보겠습니다. 

<!--more-->

* this unordered seed list will be replaced by the toc
{:toc}

## 두 문자열이 서로 같은지 비교하기

다른 프로그래밍 언어처럼 `==` 연산자로 두 문자열이 서로 같은지 비교할 수 있습니다.
```bash
#!/bin/bash

str1="Hello"
str2="World"

if [ $str1 == $str2 ]
then
    echo "Equal"
else
    echo "Not Equal"
fi
```

하지만, 위의 코드는 비교하는 문자열의 길이가 0일 떄, `unary operator expected` 에러를 맞닥뜨리게 됩니다.
해당 에러를 피하는 가장 간단한 방법은 아래 코드처럼 바로 변수를 `""`로 감싸주는 것인데요.

```bash
#!/bin/bash
str1="Hello"
str2="World"

if [ "$str1" == "$str2" ]
then
    echo "Equal"
else
    echo "Not Equal"
fi
```

혹은 아래 코드처럼 대괄호(squre bracket)을 두 번 덧대어주는 방식으로도 해결할 수 있습니다.

```bash
#!/bin/bash
str1="Hello"
str2="World"

if [[ $str1 == "$str2" ]]
then
    echo "Equal"
else
    echo "Not Equal"
fi
```

## 특정 문자열을 포함하는지 확인

두 가지 방법으로 문자열에 특정 문자열이 포함되어 있는지 확인할 수 있습니다.
* `[[ "$str1" == *str2* ]]`
* `[[ "$str1" =~ str2 ]]`

```bash
#!/bin/bash
str="Let's study Kubernetes together."

if [[ "$str1" == *study* ]]; then
    echo "It contains 'study'"
fi
```

[Git 설정] 중에는 `core.ignoreCase` 옵션이 있습니다.

> Internal variable which enables various workarounds to enable Git to work better on filesystems that are not case sensitive, like APFS, HFS+, FAT, NTFS, etc. For example, if a directory listing finds "makefile" when Git expects "Makefile", Git will assume it is really the same file, and continue to remember it as "Makefile".
>
> The default is false, except git-clone[1] or git-init[1] will probe and set core.ignoreCase true if appropriate when the repository is created.
>
> Git relies on the proper configuration of this variable for your operating and file system. Modifying this value may result in unexpected behavior.

APFS, HFS+, FAT, NTFS 등 case insensitive한 파일 시스템에서 정상적으로 작동하기 위해 존재하는 옵션값인데,
만약 이 옵션이 `true`로 되어 있다면 git은 파일/폴더명의 대소문자를 구분하지 않습니다.
OS의 파일 시스템이나 깃 레포와 직접적으로 연관되어 있는 옵션이기 때문에 이 값을 직접 변경할 경우 예상치 못한
결과가 발생할 수 있기 때문에 추천드리지 않습니다.

[Git 설정]: https://git-scm.com/docs/git-config#Documentation/git-config.txt-coreignoreCase

## Git 명령어로 변경하기

예제에서는 `src/My-Home` 폴더를 `src/my-home` 폴더로 변경하려 한다고 가정하고 진행하도록 하겠습니다.
폴더명을 예시로 들고 있지만 파일명을 변경할 때에도 동일한 방법을 사용하여 해결할 수 있습니다.

1. `git mv` [명령어]를 사용해서원본 폴더를 `tmp` 폴더로 이름을 변경합니다.

   ```shell
   git mv src/My-Home src/tmp
   ```

2. 생성한 임시 폴더(`tmp`)를 다시 원하는 폴더명으로 변경합니다.

   ```shell
   git mv src/tmp src/my-home
   ```

3. `git status` 명령어를 통해 정상적으로 폴더명이 변경되었는지 확인합니다.
   아래와 비슷하게 `renamed:` 로 시작하는 *Changes to be committed:*가 보인다면 성공입니다.

   ```shell
   Changes to be committed:
   (use "git restore --staged <file>..." to unstage)
         renamed:    src/My-Home/test.cpp -> src/my-home/test.cpp
   ```

[명령어]: https://git-scm.com/docs/git-mv

* [How to Compare Strings in Bash](https://linuxize.com/post/how-to-compare-strings-in-bash/)
{:.note title="출처"}