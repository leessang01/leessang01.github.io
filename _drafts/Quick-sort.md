---
title: 퀵 정렬(Quick Sort)
description: >-
    분할 정복 알고리즘을 기반으로 하는 퀵 정렬에 대한 내용입니다.
categories: [자료구조, 이론]
tags: [정렬, 시간복잡도, 알고리즘]
math: true
---

## 정의

퀵 정렬(Quick Sort)은 분할 정복(Divide and Conquer) 알고리즘에 기반한 정렬 알고리즘입니다. 퀵 정렬은 병합 정렬(Merge Sort)과 마찬가지로 배열을 **재귀적**으로 분할하지만, 병합 단계 없이 분할 과정에서 정렬을 수행한다는 점이 다릅니다. 이는 퀵 정렬이 매우 빠르고 공간 효율적이게 만드는 주요 특징 중 하나입니다.

## 알고리즘 단계

1. 피벗 선택(Pivot Selection)

   - 배열에서 하나의 원소를 피벗(pivot)으로 선택합니다. 피벗 선택 방법은 다양할 수 있지만 보통 첫 번째, 마지막, 중간 원소, 또는 무작위로 선택하는 방식이 사용됩니다.

2. 분할(Partitioning)

   - 피벗을 기준으로 배열을 두 개의 하위 배열로 분할합니다. 피벗보다 작은 원소들은 피벗의 왼쪽 하위 배열에, 피벗보다 큰 원소들은 오른쪽 하위 배열에 위치시킵니다. 피벗은 최종적으로 두 하위 배열 사이의 올바른 위치에 놓이게 됩니다.

3. 재귀적 정렬(Recursive Sort)
   
   - 분할된 두 하위 배열에 대해 재귀적으로 퀵 정렬을 수행합니다.

4. 정렬 완료

   - 재귀 호출이 완료되면 모든 하위 배열이 정렬되고, 전체 배열이 정렬된 상태가 됩니다.

## Pseudo Code

```python
QUICK-SORT(array, low, high)
    If low < high
        pivotIndex = PARTITION(array, low, high)
        QUICK-SORT(array, low, pivotIndex - 1)
        QUICK-SORT(array, pivotIndex + 1, high)

PARTITION(array, low, high)
    pivot = array[high]  // Choosing the last element as the pivot
    i = low - 1
    For j = low to high - 1
        If array[j] ≤ pivot
            i = i + 1
            Swap array[i] with array[j]
    Swap array[i + 1] with array[high]
    Return i + 1

```

## 성능

### 시간 복잡도

퀵 정렬의 시간 복잡도는 피벗 선택 방법과 데이터의 분포에 따라 다릅니다.

1. 최선의 경우(Best Case): $O(n \log n)$

   - 피벗이 항상 배열의 중앙에 가까운 경우, 배열이 매번 균등하게 분할되어 로그 깊이의 재귀 호출이 필요합니다.

2. 평균의 경우(Average Case): $O(n \log n)$

   - 대부분의 경우, 피벗이 배열을 적당히 균등하게 분할하기 때문에 로그 깊이의 재귀 호출이 필요합니다.

3. 최악의 경우(Worst Case): $O(n^2)$

   - 피벗이 배열의 최댓값이나 최솟값으로 선택되어, 배열이 최대한 불균형하게 분할되는 경우입니다. 예를 들어, 이미 정렬된 배열에 대해 첫 번째 또는 마지막 원소를 항상 피벗으로 선택하면 최악의 성능을 보입니다.

### 공간 복잡도

- TBD


## 예시

### 문제 1

- <kbd>5, 6, 4, 7, 2, 1, 3</kbd>

#### 피벗 선택

마지막 원소 <kbd>3</kbd>을 피벗으로 선택한다고 가정한다.

#### 분할

피벗보다 작은 원소들은 피벗의 왼쪽 하위 배열에, 피벗보다 큰 원소들은 오른쪽 하위 배열에 위치시킨다.

- Low: l, High: h, Pivot: P

```
   5 6 4 7 2 1 3 // swap arr[low] and arr[high]
   l         h p

   1 6 4 7 2 5 3
   l         h p
   
   1 6 4 7 2 5 3 // swap arr[low] and arr[high]
     l     h   p

   1 2 4 7 6 5 3
     l     h   p

   1 2 4 7 6 5 3 // swap arr[low] and arr[pivot]
     h l       p

   1 2 [3] 7 6 5 4 // 3 is sorted
   | p     l   h p
  l,h
```
```
   1 [2] [3] 7 6 5 4 // 2 is sorted and 1 is sorted as well
   h  |
     l,p
```
```
   [1] [2] [3] 7 6 5 4
               l,h   p

   [1] [2] [3] [4] 6 5 7
                   l h p

   [1] [2] [3] [4] 5 6 7
                   l h p

   [1] [2] [3] [4] 5 6 7
                     h p,l
                     
   [1] [2] [3] [4] 5 6 [7]
                   | p
                  l,h
```

#### 정복 & 병합

나누었던 하위 배열을 **정렬 순서를 고려하여** 하나의 배열로 다시 병합한다.

- <kbd>5, 6</kbd><kbd>4, 7</kbd><kbd>1, 2</kbd><kbd>3</kbd>
- <kbd>4, 5, 6, 7</kbd><kbd>1, 2, 3</kbd>
- <kbd>1, 2, 3, 4, 5, 6, 7</kbd>

#### 풀이 애니메이션

> 출처: http://algorithm.wiki/

![merge-sort](/assets/img/merge-sort.gif)

## 참고

- 구종만, 알고리즘 문제 해결 전략 1, 인사이트(2012)
- 윤성우, 열혈 자료구조, 오렌지미디어(2012)