---
title: 코드 블록에서 특정 라인 강조하기
description: >-
    Jekyll 4.4.0 버전에서부터 지원하는 코드 블록에서 특정 라인을 강조하는 기능을 도입하는 방법에 대해서 소개합니다.
date: 2024-07-17 16:00:00 +0900
categories: [블로그, 기능]
tags: [jekyll, mark lines, highlight lines]
---

글을 작성하다보면 코드 블록을 종종 사용하게 된다. 이때 코드상에서 특정 라인을 강조하고 싶어도 이 블록은 이미 코드 형태로 강조된 영역이라 서식을 적용할 수 없다.
이것저것 검색해보니 **Jekyll 4.4.0** 버전부터 원하던 기능이 추가되었다.[^marking]

하지만 현재 사용중인 Chirpy 테마 최신 버전(7.0.1)에서는 **Jekyll 4.3.3**을 사용하기 때문에 아직 지원되지 않는 기능이다.
또 다시 이것저것 검색해보니 해당 기능을 사용할 수 있는 방법이 있어서 소개하고자 한다.[^helpful-answer]

> **Marking specific lines (since 4.4.0)**\
> You can mark specific lines in a code snippet by using the optional argument `mark_lines`. This argument takes a space-separated list of line numbers which must be wrapped in double quotes. For example, the following code block will mark lines 1 and 2 but not line 3:
> ```liquid
> {% raw %}{% highlight ruby mark_lines="1 2" %}
> def foo
>   puts 'foo'
> end
> {% endhighlight %}{% endraw %}
> ```

## highlight.rb 코드 가져오기

Jekyll 4.4.0 버전에서 구현된 `mark_lines` 기능이 담긴 코드를 `master` 브랜치로부터 가져온다. 즉 [highlight.rb](https://github.com/jekyll/jekyll/blob/master/lib/jekyll/tags/highlight.rb#L83) 링크를 타고 가서 이 rb 파일을 직접 다운로드 하고, 내 Jekyll 블로그의 `_plugins` 폴더에 넣어준다. 블로그가 구동될 때 (highlight 기능이 추가된) 이 rb 파일로 덮어쓰는 듯 하다.

## CSS 추가하기

내 Jekyll 블로그의 `assets/css` 폴더에 `jekyll-theme-chirpy.scss` 파일에 `hll` 스타일 양식을 원하는대로 정의한다. scss 파일이 없다면 생성하면 된다.
아래 형광펜으로 칠한 부분이 추가한 내용이다.[^highlight]

{% highlight ruby mark_lines="8 9 10 11" %}
---
---

@import 'main';

/* append your custom style below */

.hll{
  display: inline;
  box-shadow: inset 0 -10px 0 #D9FCDB;
}
{% endhighlight %}

## 사용하기

앞서 "Marking specific lines"에서 예시로 나온 코드 블록으로 작성하면 다음과 같이 첫 번째, 두 번째 줄이 강조된 코드를 얻을 수 있다.

{% highlight ruby mark_lines="1 2" %}
def foo
  puts 'foo'
end
{% endhighlight %}

## 한계

기본(화이트) 모드에 적합한 색상으로 `hll`을 정의했는데, 다크 모드로 전환하니 코드 글자색도 변하면서 강조한 부분이 가려진다.
각 모드에 적합한 형광색으로 바뀌도록 개선해야겠다.

---
[^marking]: Jekyll, [Marking Specific Lines](https://jekyllrb.com/docs/liquid/tags/#marking-specific-lines)
[^helpful-answer]: Stackoverflow, [How to highlight the specific lines of code snippet present in markdown using jekyll?](https://stackoverflow.com/a/78463786/15127300)
[^highlight]: 블로그, [CSS로 형광펜 효과 만들기](https://bomee88.github.io/css/highlight/css-highlight)
