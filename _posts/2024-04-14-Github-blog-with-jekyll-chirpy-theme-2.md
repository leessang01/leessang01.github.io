---
title: Jekyll에 Chirpy 테마 적용하여 Github 블로그 만들기 (2)
date: 2024-04-14 11:30:00 +0000
categories:
  - 블로그
tags:
  - github
  - jekyll
  - chirpy
---
* TOC
{: toc}
---
**Ubuntu 환경**에서 Jekyll을 이용해 블로그 구조를 생성하고, Chirpy 테마를 적용하여, Github을 통해 호스팅하기까지 과정을 기록합니다[^jekyll_docs].

[첫 번째 글]({% post_url 2024-04-09-Github-blog-with-jekyll-chirpy-theme-1 %})에서는 필수 라이브러리를 설치하며 환경구성을 수행했습니다. 두 번째 글에서는 **Jekyll 설치와 Chirpy 테마 적용**에 다루어 보겠습니다. Jekyll 설치 자체는 간단하지만 Chirpy 테마를 적용하는 것은 공식가이드 **Getting Started**[^chirpy_getting-started]를 따르지 않을 경우 올바르게 동작하지 않을 수 있습니다.

---
## **설치환경구성**

| Item   | Version                                                    | by Command                        |
| ------ | ---------------------------------------------------------- | --------------------------------- |
| OS     | Ubuntu 20.04.6 LTS                                         | `cat /etc/os-release`             |
| Ruby   | ruby 3.3.0 (2023-12-25 revision 5124f9ac75) [x86_64-linux] | `ruby -v`                         |
| Gem    | 3.5.3                                                      | `gem -v`                          |
| Bundle | Bundler version 2.5.7                                      | `bundle -v`                       |
| Node   | v20.12.1                                                   | `node -v`                         |
| npm    | 10.5.0                                                     | `npm -v`                          |
| Jekyll | jekyll 4.3.3                                               | `bunlde exec jekyll -v`           |
| Chirpy | jekyll-theme-chirpy (6.5.5)                                | `bundle info jekyll-theme-chirpy` |

이번 글에서는 Ubuntu 환경에서 위 목록 중 다음의 항목에 대해서 다룹니다.
- [ ] Ruby
- [ ] Gem
- [ ] Bundle
- [ ] Node
- [ ] npm
- [x] Jekyll
- [x] Chirpy

## **공식가이드 개요**

Chirpy 공식 가이드[^chirpy_getting-started]에 따르면 Chirpy 테마를 적용하여 Jekyll 사이트를 생성하는 방법에는 두 가지가 있습니다.

> There are two ways to create a new repository for this theme:
> 
> - [**Using the Chirpy Starter**](https://chirpy.cotes.page/posts/getting-started/#option-1-using-the-chirpy-starter) - Easy to upgrade, isolates irrelevant project files so you can focus on writing.
> - [**GitHub Fork**](https://chirpy.cotes.page/posts/getting-started/#option-2-github-fork) - Convenient for custom development, but difficult to upgrade. Unless you are familiar with Jekyll and are determined to tweak or contribute to this project, this approach is not recommended.

[첫 번째 방법](#방법-1-chirpy-starter로-블로그-생성하기)은 Chirpy Starter를 이용하는 방식으로 설명에 따르면 버전 업그레이드가 쉽고, 프로젝트와 무관한 파일들을 분리하기 쉽기 때문에 블로그 글을 작성하는 데 집중할 수 있다는 특징이 있습니다.

[두 번째 방법](#방법-2-chirpy-개발자-버전으로-블로그-생성하기)은 Github Fork를 이용하는 방식으로 개발자 버전을 이용하는 셈입니다. 특징으로는 커스텀하기 편리하지만 업그레이드 하기 어려우니 Jekyll에 친숙하거나 Chirpy 테마 프로젝트에 변경 또는 기여하고자 하는 것이 아니라면 권장하지 않는다고 합니다. 

두 가지 방법의 장단점이 있고, 설치 방법도 다르기 때문에 각자의 목적에 알맞은 것을 선택하면 됩니다. 개인적으로는 첫 번째 방법을 권장합니다. 물론 이번 글에서는 두 방법 모두 다룰 예정이니 구분하여 참고하시면 될 것 같습니다. 

>일부 글에서는 Chirpy 테마를 zip으로 받아서 복사 후 붙여넣기 하면 Chirpy 테마를 적용할 수 있다고 하는데, 개인적인 경험으로는 그 방법을 통해서는 Chirpy 테마가 정상적으로 동작하지 않았습니다. 블로그 서버를 띄울 때 js가 없다고 하고 이로 인해 다크모드 전환 버튼이 동작하지 않는 등 여러 문제가 발생했습니다.
{: .prompt-warning}

## **Github 계정**
최종적으로 Github에 블로그 프로젝트를 올려 호스팅되는 것이 목적이기 때문에 Github 계정은 반드시 필요합니다. 또한 Chirpy 테마의 개발자 버전을 설치하려면 Github Fork 및 git clone을 해야하기 때문에 Github 계정이 필요합니다.

이 내용을 다루는 자료가 많기 때문에 이 글에서는 다루지 않고 필요한 키워드만 적었습니다.
- Github 계정
- Github personal API token

## **Jekyll 설치 및 로컬 환경 테스트**

>Jekyll is a blog-aware, static site generator in Ruby

Chirpy 테마를 설치할 때 Jekyll 또한 포함되어 설치되기 때문에 **반드시 필요한 과정은 아닙니다**. 단지 Jekyll의 기본 블로그를 생성해보고, 로컬 환경에서 서버를 띄워 블로그가 잘 동작하는지 확인하는 데 목적이 있습니다.

gem 명령어를 통해 Jekyll을 설치하고, 로컬 환경에서 기본 블로그를 생성할 수 있습니다[^jekyll_quick-start]. 특히 마지막 줄 명령어는 로컬 환경에서 개발중일 때 자주 사용하는 명령어이므로 기억해두면 좋습니다.
```bash
gem install jekyll
jekyll new my-awesome-blog
cd my-awesome-blog
bundle exec jekyll serve
# 브라우저로 http://localhost:4000에 접속하여 생성된 블로그 페이지 확인
```

## **방법 1. Chirpy Starter로 블로그 생성하기**

### **Chirpy 테마 가져오기**
Github에 로그인한 상태에서 [**Chirpy Starter**](https://github.com/cotes2020/chirpy-starter) 저장소로 이동합니다. 그리고 <kbd>Use this template</kbd> 버튼을 클릭하여 해당 템플릿으로 내 저장소를 생성합니다. 저장소 이름(repository name)은 본인의 Github 계정으로 하여 `USERNAME.github.io` 형태로 하고, <kbd>Create repository</kbd>를 클릭하여 저장소 생성을 완료합니다.

예를 들어 사용자 이름이 `username`이라면 아래와 같은 저장소가 생성됩니다.
- `https://github.com/username/username.github.io`

### **Chirpy 테마 내려받기**
앞서 내 원격 저장소에 생성한 Chirpy Starter 템플릿을 git clone을 통해 내려받습니다. 이때 Github에서 발행한 personal API token을 함께 입력해줘야 git push 또는 git pull과 같이 원격 저장소와 통신 할 때 계정 정보를 매번 입력해야하는 번거로움을 줄일 수 있습니다.

예를 들어 발행한 토큰이 `abcd`이고, 사용자 이름이 `username`이라면 다음과 같이 입력하면 됩니다.
- `git clone https://abcd@github.com/username/username.github.io.git`  

```bash
git clone https://[your-github-token]@github.com/[username]/[repo].git
```

>2021년 8월 13일 이후 Github에서는 원격 저장소에 git 명령어로 인증이 필요한 작업에 계정 비밀번호 인증을 더 이상 지원하지 않습니다. 따라서 PAT(Personal Access Token)를 따로 발급받고 인증 과정에 이것을 대신 사용해야합니다[^github_pat].
{: .prompt-info}

### **Chirpy 테마 의존성 설치하기**
git clone으로 내려받은 경로로 이동하여 아래의 명령어로 필요한 의존성을 설치합니다. Gemfile에 `gem "jekyll-theme-chirpy"` 형태로 의존성이 명시되어 있기 때문에 Chirpy 테마를 설치할 수 있습니다. 이 과정에 Jekyll도 포함되어 설치됩니다.
```shell
bundle install
# Bundle complete!
```

### **로컬환경에서 블로그 띄우기**
이제 Chirpy Starter를 이용한 테마 설치는 모두 마쳤습니다. 로컬 환경에서 빌드 및 배포된 블로그를 확인하려면 다음의 명령어로 서버를 띄우고, 로그에 찍힌 Server address에서 확인할 수 있습니다. 테마가 올바르게 적용되었는지 좌측 하단의 다크모드 전환 버튼이나 여러 버튼을 눌러보며 확인할 수 있습니다.
```shell
bundle exec jekyll serve
# Server address: http://127.0.0.1:4000/
# Server running... press ctrl-c to stop
```

## **방법 2. Chirpy 개발자 버전으로 블로그 생성하기**
### **Chirpy 테마 가져오기**
Github에 로그인한 상태에서 [**Chirpy Jekyll Theme**](https://github.com/cotes2020/jekyll-theme-chirpy) 저장소로 이동합니다. 그리고 <kbd>Fork</kbd> 버튼을 클릭하여 해당 저장소를 내 저장소로 포크합니다. 저장소 이름(repository name)은 본인의 Github 계정으로 하여 `USERNAME.github.io` 형태로 하고, <kbd>Create fork</kbd>를 클릭하여 저장소 생성을 완료합니다.

예를 들어 사용자 이름이 `username`이라면 아래와 같은 저장소가 생성됩니다.
- `https://github.com/username/username.github.io`

### **Chirpy 테마 내려받기**
앞서 내 원격 저장소에 포크한 Chirpy Jekyll Theme을 git clone을 통해 내려받습니다. 이때 Github에서 발행한 personal API token을 함께 입력해줘야 git push 또는 git pull과 같이 원격 저장소와 통신 할 때 계정 정보를 매번 입력해야하는 번거로움을 줄일 수 있습니다.

예를 들어 발행한 토큰이 `abcd`이고, 사용자 이름이 `username`이라면 다음과 같이 입력하면 됩니다.
- `git clone https://abcd@github.com/username/username.github.io.git`  

```bash
git clone https://[your-github-token]@github.com/[username]/[repo].git
```

>2021년 8월 13일 이후 Github에서는 원격 저장소에 git 명령어로 인증이 필요한 작업에 계정 비밀번호 인증을 더 이상 지원하지 않습니다. 따라서 PAT(Personal Access Token)를 따로 발급받고 인증 과정에 이것을 대신 사용해야합니다[^github_pat].
{: .prompt-info}

### **Chirpy 테마 초기화하기**
개발자 버전으로 내려받은 상황에서는 초기화 과정이 반드시 필요합니다. 단, 초기화 수행 전에 [첫 번째 글]({% post_url 2024-04-09-Github-blog-with-jekyll-chirpy-theme-1 %})에서 다룬 **Node 설치**가 반드시 선행되어야 합니다.
git clone으로 내려받은 경로로 이동하여 아래의 명령어로 Chirpy 테마 개발자 버전을 초기화 합니다. 
```shell
bash tools/init
# HEAD is now at 7a7818b chore(release): 6.5.5
# [INFO] Initialization successful!
```
>해당 스크립트를 보면 `checkout_latest_release()`와 같은 함수에서 최신 릴리즈 버전으로 `git reset --hard`를 수행하고, 여러 자바스크립트 파일을 빌드합니다. 따라서 반드시 git 이력이 있어야 하며 node가 필요한 이유이기도 합니다. 이 초기화 과정을 제대로 수행하지 않으면 Chirpy 테마가 제대로 적용되지 않고 여러 문제가 발생합니다.
{: .prompt-info}


### **Chirpy 테마 의존성 설치하기**
git clone으로 내려받은 경로에서 아래의 명령어로 필요한 의존성을 설치합니다.
```shell
bundle install
# Bundle complete!
```

### **로컬환경에서 블로그 띄우기**
이제 Chirpy 개발자 버전을 이용한 테마 설치는 모두 마쳤습니다. 로컬 환경에서 빌드 및 배포된 블로그를 확인하려면 다음의 명령어로 서버를 띄우고, 로그에 찍힌 Server address에서 확인할 수 있습니다. 테마가 올바르게 적용되었는지 좌측 하단의 다크모드 전환 버튼이나 여러 버튼을 눌러보며 확인할 수 있습니다.
```shell
bundle exec jekyll serve
# Server address: http://127.0.0.1:4000/
# Server running... press ctrl-c to stop
```

**정리**
> Ubuntu 환경에서 Jekyll을 이용해 블로그 구조를 생성하고, Chirpy 테마를 적용하여, Github을 통해 호스팅하기

이번 글에서는 Jekyll 설치와 Chirpy 테마를 적용하는 방법에 대해서 알아보았습니다. Jekyll은 Chirpy 테마 설치 과정 중 함께 설치되기 때문에 별도의 설치 과정이 필요없지만 Jekyll의 기본 블로그를 생성하고 로컬 환경에서 띄워 볼 수 있었습니다. 그리고 Chirpy 테마 설치는 Starter를 이용하는 방법과 개발자 버전을 이용하는 방법이 있었으며 Chirpy 테마 개발에 기여할 목적이 있거나 복잡한 튜닝을 하실 분이 아니라면 개인적으로는 전자의 방법을 추천합니다. Starter가 Chirpy 테마의 버전 업데이트도 더 쉽기 때문입니다[^chirpy_upgrade-guide].

---
## 참고
[^jekyll_docs]: Jekyll on Ubuntu, Jekyll, [link](https://jekyllrb.com/docs/installation/ubuntu/)
[^jekyll_quick-start]: Jekyll Quick-start instructions, Jekyll, [link](https://jekyllrb-ko.github.io)
[^chirpy_getting-started]: Getting Started, Chirpt, [link](https://chirpy.cotes.page/posts/getting-started/)
[^github_pat]: Support for password authentication was removed, Github, [link](https://stackoverflow.com/questions/68775869/message-support-for-password-authentication-was-removed)
[^chirpy_upgrade-guide]: Upgrade Guide, Chirpy, [link](https://github.com/cotes2020/jekyll-theme-chirpy/wiki/Upgrade-Guide)