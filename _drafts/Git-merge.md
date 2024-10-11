---
title: Git 병합 방법(git-merge)
description: >-
    Git에서 브랜치를 관리하고 병합하는 과정에 대한 설명
categories: [Git, git-merge]
tags: [브랜치관리, 브랜치병합]
mermaid: true
---

## 개요

Git에서 브랜치를 병합한다는 것은 서로 다른 **개발작업 이력**을 하나로 합치는 것을 뜻합니다. 병합은 상황에 따라 다양하게 적용할 수 있습니다. 예시를 통해 알아봅시다.

## Merge

> `git-merge` - Join two or more development histories together

대표적인 병합 방법입니다. 


```mermaid
---
title: git-merge (fast-forward)
---
gitGraph
   commit id: "C1"
   commit id: "C2"
   branch feature
   checkout feature
   commit id: "C3"
   commit id: "C4"
   checkout main
   merge feature
```

```mermaid
gitGraph
   commit id: "C1"
   commit id: "C2"
   commit id: "C3"
   commit id: "C4"
   checkout main
```

### Merge (fast-forward)



### Merge (recursive)

### Merge 충돌 해결

## Rebase and Merge

## Merge with Squash

## 참고

![summary-of-lecture](/assets/img/how-to-speak.png)