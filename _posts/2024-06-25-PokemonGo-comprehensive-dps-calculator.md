---
title: 포켓몬고 레이드 성능표(게임프레스 버전)
description: >-
    게임프레스의 Comprehensive DPS 계산을 통해 평가된 포켓몬고 레이드 성능표입니다.
date: 2024-06-26 15:00:00 +0900
categories: [포켓몬고, 분석]
tags: [DPS, TDO, ER, gamepress, 레이드성능]
image:
    path: /assets/img/comprehensive-dps-example.jpg
---

## 개요

아래의 레이드 성능표는 레이드 또는 체육관 배틀 상황에서의 일반적인 포켓몬 성능을 알아보기에 유용합니다. 게임프레스의 DPS 계산 공식[^analysis][^original]을 통해 평가되었으며 다음을 가정하고 있습니다.
- 실제 시뮬레이션이 아닌 자체 유도 공식을 통해 성능이 평가되었음
- 배틀에 참여하는 포켓몬은 레벨 40, 개체 100%로 가정함
- 5성급(레벨 40)의 무속성 레이드 보스를 상대로 함. 즉 포켓몬, 스킬 속성의 상성 관계가 고려되어 있지 않음

---

## 포켓몬고 레이드 성능표

{% include calculator_based_on_gamepress.html %}

---

## 참고

[^analysis]: 포켓몬고 Gamepress DPS 성능 계산 공식 분석, [link]({% post_url 2024-05-28-How-to-calculate-comprehensive-dps %})
[^original]: How to Calculate Comprehensive DPS, GamePress, [link](https://gamepress.gg/pokemongo/how-calculate-comprehensive-dps)
