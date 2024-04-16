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

두 번째 글에서는 **Jekyll 및 Chirpy 테마 설치 과정**에 대해 다루겠습니다. 이 과정에서 버전 충돌이나 어떤 문제가 자주 발생했는데, 에러 로그를 보고 구글링을 하거나 여러 블로그를 참고했습니다. 주먹구구식으로 하나씩 해결하기 위해 시간을 할애했지만 여전히 문제가 해결되지 않거나, 다른 문제가 발생하거나, 무언가 제대로 동작하지 않았습니다.[^blog_ref1][^blog_ref2][^blog_ref3][^blog_ref4][^blog_ref5][^blog_ref6][^blog_ref7]

따라서 **2024년 4월** 시점에서 처음부터 끝까지 문제없이 수행할 수 있는 방법을 정리해보았습니다. 물론 Window, Linux, Mac 등 운영체제마다 마주할 수 있는 문제나 세부 설치 과정이 다를 수 있습니다.

---
## **설치환경구성(remind)**

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


## **Jekyll 및 Chirpy 테마 설치**
### **Jekyll 설치**
>Jekyll is a blog-aware, static site generator in Ruby

gem 명령어를 통해 Jekyll 설치
```bash
gem install jekyll
# Successfully installed jekyll-4.3.3
```

> Jekyll 설치 이후 블로그를 생성할 경로에 가서 `jekyll new .` 명령어를 수행하면 기본 Jekyll 블로그를 생성합니다. 그리고 `bundle exec jekyll serve` 명령어를 통해 `localhost:4000` 주소에 기본 블로그를 로컬 서버에 띄울 수 있습니다.
{.prompt-info}

>일부 구글링 결과에 따르면 위와 같은 과정을 통하고 Chirpy 테마를 zip으로 받아서 복사 후 붙여넣기 하면 Chirpy 테마를 적용할 수 있다고 하는데, 개인적인 경험으로는 Chirpy 테마를 정상적으로 적용할 수 없었습니다. 따라서 Jekyll 설치 과정만 수행하고 다음 과정을 따르는 걸 권장합니다.
{.prompt-warning}

## **Chirpy 테마 설치**
Chirpy 공식 가이드[^chirpy_getting-started]에 따르면 Chirpy 테마를 설치하는 방법에는 두 가지가 있습니다.

> There are two ways to create a new repository for this theme:
> 
> - [**Using the Chirpy Starter**](https://chirpy.cotes.page/posts/getting-started/#option-1-using-the-chirpy-starter) - Easy to upgrade, isolates irrelevant project files so you can focus on writing.
> - [**GitHub Fork**](https://chirpy.cotes.page/posts/getting-started/#option-2-github-fork) - Convenient for custom development, but difficult to upgrade. Unless you are familiar with Jekyll and are determined to tweak or contribute to this project, this approach is not recommended.

첫 번째 방법은



### Clone

- 저장소 업데이트
```bash
git clone https://[your-github]:[your-github-personal-token]@github.com/[your-github]/jekyll-theme-chirpy.git
```


---
## 참고
[^jekyll_docs]: Jekyll on Ubuntu, [link](https://jekyllrb.com/docs/installation/ubuntu/)
[^chirpy_getting-started]: Getting Started, [link](https://chirpy.cotes.page/posts/getting-started/)
