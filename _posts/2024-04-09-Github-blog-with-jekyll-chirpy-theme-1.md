---
title: Jekyll에 Chirpy 테마 적용하여 Github 블로그 만들기 (1)
date: 2024-04-09 11:30:00 +0000
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

첫 번째 글에서는 **환경구성**에 대해 다루어 보겠습니다. 이 과정에서 버전 충돌이나 어떤 문제가 자주 발생했는데, 에러 로그를 보고 구글링을 하거나 여러 블로그를 참고했습니다. 주먹구구식으로 하나씩 해결하기 위해 시간을 할애했지만 여전히 문제가 해결되지 않거나, 다른 문제가 발생하거나, 무언가 제대로 동작하지 않았습니다.[^blog_ref1][^blog_ref2][^blog_ref3][^blog_ref4][^blog_ref5][^blog_ref6][^blog_ref7]

따라서 **2024년 4월** 시점에서 처음부터 끝까지 문제없이 수행할 수 있는 방법을 정리해보았습니다. 물론 Window, Linux, Mac 등 운영체제마다 마주할 수 있는 문제나 세부 설치 과정이 다르겠지만 각 라이브러리의 버전은 꼼꼼히 확인하여 설치하는 것을 권장합니다.

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


### **Ruby 설치**
> 최신 Chirpy 테마를 적용하려면 `ruby v3.0` 이상을 설치하는 것을 권장합니다. 처음에 단순히 `sudo apt install ruby-full`로 시도했다가 `ruby v2.7.x`이 설치되었고 이후 Chirpy 테마 초기화 후 `bundle install` 과정에서 버전 충돌로 인해 낭패를 봤습니다.\
> Github에서는 `ruby v2.7` 버전을 사용하는 것 같은데 어떻게 충돌이 없는지 의아하네요.
{: .prompt-warning }

<details>
<summary><b>ruby v2.7 설치하면 bundle install 단계에서 마주할 에러 로그 (click me)</b></summary>
{% highlight terminal %}
Bundler found conflicting requirements for the Ruby version:
 In Gemfile:
  Ruby
  
  jekyll-theme-chirpy was resolved to 6.5.5, which depends on
   Ruby (>=3.0)

Ruby (>=3.0), which is required by gem 'jekyll-theme-chirpy', is not available in the local ruby installation.
{% endhighlight %}
</details>

아래의 순서를 따라 여러 ruby 버전을 설치 및 관리해주는 **rbenv**를 통해 **ruby**와 **gem**을 설치할 수 있습니다[^ruby_installation].


#### **rbenv 설치**
> rbenv - manage your application's Ruby environment

저장소 업데이트
```bash
sudo apt update
```

 ruby를 빌드하기 위한 필요한 라이브러리 설치
```bash
sudo apt install git curl autoconf bison build-essential \
libssl-dev libyaml-dev libreadline6-dev zlib1g-dev \
libncurses5-dev libffi-dev libgdbm6 libgdbm-dev libdb-dev
```

curl fsSL[^fssl_meaning]을 통해 rbenv 설치
```bash
curl -fsSL https://github.com/rbenv/rbenv-installer/raw/HEAD/bin/rbenv-installer | bash
```

환경변수 추가 및 갱신 (bash 기준)
```bash
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(rbenv init -)"' >> ~/.bashrc
source ~/.bashrc
```

설치된 rbenv 버전 확인
```bash
rbenv -v
# rbenv 1.2.0-87-ge8b7a27
```


#### **rbevn를 통해 ruby 설치**
rbenv를 통해 현재 설치 가능한 ruby 버전 목록 확인
```bash
rbenv install -l
# 3.0.6
# 3.1.4
# 3.2.3
# 3.3.0
```

설치 가능한 목록 중 원하는 버전으로 설치하고 사용할 버전 적용(global). 그리고 터미널을 닫고 새로 켠 다음 ruby 버전 확인
```bash
rbenv install 3.3.0
rbenv global 3.3.0
ruby -v
# ruby 3.3.0 (2023-12-25 revision 5124f9ac75) [x86_64-linux]
```


#### **설치된 gem 버전 확인**
> RubyGems is a package manager for Ruby

ruby 설치 시 딸려오는 gem 버전 확인
```bash
gem -v
# 3.5.3
```

#### **설치된 bundle(bundler) 버전 확인** 
> bundle - Ruby Dependency Management

ruby 설치 시 딸려오는 bundle(bundler) 버전 확인
```bash
bundle -v
# Bundler version 2.5.7
```

---
### **Node 설치 (Github Fork only)**
> 이 부분을 누락하면 이후 Chirpy 테마를 Github Fork로 설치하고 초기화하는 과정 `bash tools/init`에서 문제가 발생합니다.
{: .prompt-warning }

아래의 순서를 따라 여러 node 버전을 설치 및 관리해주는 **nvm(node version manager)**을 통해 **node(node.js)**와 **npm**을 설치할 수 있습니다[^node_installation].


#### **nvm 설치**
curl fsSL[^fssl_meaning]을 통해 nvm 설치
```bash
curl -fsSL https://raw.githubusercontent.com/nvm-sh/nvm/master/install.sh | bash
```

터미널을 닫고 새로 켠 다음 nvm 버전 확인
```bash
nvm -v
# 0.39.7
```


#### **nvm을 통해 node 설치**
>`nvm list-remote` 명령어를 통해 나열되는 버전 중 현재 Ubuntu 버전에 적합한 node를 설치해야 합니다. 예를 들어  `Ubuntu 20.04.6 LTS`에서는 `node v20.12.1`이 정상 동작했지만 `Ubuntu 16.04.7 LTS`에서는 더 낮은 버전을 설치해야 했습니다.\
>단, 최신 Chirpy 테마(현재 시점에서 `v6.5.5` 기준)에서는 일부 라이브러리에서 `node v18` 이상을 요구하고 있기 때문에 Chirpy 테마 초기화 과정(`bash tools/init`)에서 경고 로그가 발생할 수 있습니다.
{: .prompt-warning }

<details>
<summary><b>Ubuntu 버전에 맞지 않는 node를 설치한 경우 에러 로그 (click me)</b></summary>
{% highlight terminal %}
$ node -v
node: /lib/x86_64-linux-gnu/libm.so.6: version 'GLIBC_2.27' not found (required by node)
{% endhighlight %}
</details>

nvm을 통해 현재 설치 가능한 버전 목록 확인
```bash
nvm list-remote
```

nvm을 통해 원하는 node 버전 설치. 제 환경에서는 `--lts` 옵션을 통해 최신 LTS 버전인 `v20.12.2`를 설치하였습니다.

```bash
nvm install [<version>]
```

현재 설치된 node 버전 확인
```bash
nvm ls
# ->     v20.12.1
#         v21.7.2
#         system
```

원하는 node 버전 적용
```bash
nvm use 20.12.1
# Now using node v20.12.1 (npm v10.5.0)
```
 
node 버전 적용 확인
```bash
node -v
# v20.12.1
```


#### **설치된 npm 버전 확인**
node 설치 시 딸려오는 npm 버전 확인
```bash
npm -v
# 10.5.0
```


## **정리**
> Ubuntu 환경에서 Jekyll을 이용해 블로그 구조를 생성하고, Chirpy 테마를 적용하여, Github을 통해 호스팅하기

이번 글에서는 서두에서 언급한 전체 과정 중 가장 선행되는 환경구성에 대해 살펴보았습니다. 여러 블로그에서 참고할 만한 자료가 많았지만 오래 전에 작성된 글이라 현재 시점에서 글대로 수행되지 않거나 알 수 없는 오류를 마주하는 경우가 많았습니다. 특히 이런 오류를 마주하고 해결해가는 글이 생각보다 많았는데, 개인적으로 내린 결론은 1) 처음에 환경구성에 문제가 있었거나 2) 다음 글에서 살펴 볼 Jekyll 및 Chirpy 테마 설치 단계에서 chirpy starter나 zip 파일을 통하는 것 자체에서 비롯된 문제였습니다. (물론 Window나 Mac에서는 수행해보지 않아서 다른 문제가 원인일 수도 있습니다.)

이번 글을 통해 설치를 무사히 마쳤다면 다음 글에서 다룰 **Jekyll 및 Chirpy 테마 설치 과정**에서는 큰 문제가 없을 것이라 기대합니다. 감사합니다.

## **참고**

[^jekyll_docs]: Jekyll on Ubuntu, [link](https://jekyllrb.com/docs/installation/ubuntu/)
[^blog_ref1]: Jekyll Chirpy 테마 사용하여 블로그 만들기, [link](https://www.irgroup.org/posts/jekyll-chirpy/)
[^blog_ref2]: 이거하나로 GitHub Blog 만들기, [link](https://www.handongbee.com/posts/GitHub-Blog-%EC%8B%9C%EC%9E%91%ED%95%98%EA%B8%B0/)
[^blog_ref3]: 나만의 블로그 만들기 Git hub blog!! (github.io), [link](https://supermemi.tistory.com/entry/%EB%82%98%EB%A7%8C%EC%9D%98-%EB%B8%94%EB%A1%9C%EA%B7%B8-%EB%A7%8C%EB%93%A4%EA%B8%B0-Git-hub-blog-GitHubio)
[^blog_ref4]: Github 블로그 만들기 - 1. 시작하기, [link](https://tired-o.github.io/posts/github-blog-1/)
[^blog_ref5]: Github 블로그 만들기 (1), [link](https://devpro.kr/posts/Github-%EB%B8%94%EB%A1%9C%EA%B7%B8-%EB%A7%8C%EB%93%A4%EA%B8%B0-(1)/)
[^blog_ref6]: 개인 Blog 만드는 절차 with Jekyll & GitHub Pages, [link](https://cjy-tech.github.io/make-blog-with-jekyll-and-github_pages/)
[^blog_ref7]: Jekyll Chirpy(v6.0.1) 테마를 활용한 Github 블로그 만들기(2023.6 기준), [link](https://jjikin.com/posts/Jekyll-Chirpy-%ED%85%8C%EB%A7%88%EB%A5%BC-%ED%99%9C%EC%9A%A9%ED%95%9C-Github-%EB%B8%94%EB%A1%9C%EA%B7%B8-%EB%A7%8C%EB%93%A4%EA%B8%B0(2023-6%EC%9B%94-%EA%B8%B0%EC%A4%80)/#%EC%BB%A4%EC%8A%A4%ED%84%B0%EB%A7%88%EC%9D%B4%EC%A7%95-%EA%B0%84-%EC%9D%B4%EC%8A%88-%ED%95%B4%EA%B2%B0)
[^ruby_installation]: Linux : Ubuntu 20.04 : Ruby 설치 방법, 예제, 명령어, [link](https://jjeongil.tistory.com/1970)
[^node_installation]: Linux : Ubuntu 22.04 : Node.js and npm 설치 방법, 예제, 명령어, [link](https://jjeongil.tistory.com/2106)
[^fssl_meaning]: curl -fsSL 옵션 의미, [link](https://explainshell.com/explain?cmd=curl+-fsSL+example.org#)
