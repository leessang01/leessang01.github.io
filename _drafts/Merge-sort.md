---
title: 병합 정렬(Merge Sort)
description: >-
    분할 정복 알고리즘을 기반으로 하는 병합 정렬에 대한 내용입니다.
categories: [자료구조, 이론]
tags: [정렬, 시간복잡도, 알고리즘]
math: true
mermaid: true
---

## 정의

병합 정렬(Merge Sort)은 분할 정복(Divde and Conquer) 알고리즘에 기반한 정렬 알고리즘입니다. 주어진 배열을 비슷한 크기의 두 개의 하위 배열로 나누어 **재귀적**으로 정렬합니다. 그리고 최종적으로 두 하위 배열을 병합하여 정렬된 배열을 얻습니다.

## 알고리즘 단계

1. 분할(Divde)

   - 배열이 하나의 원소가 될 때까지 절반으로(즉 두 개의 하위 배열로) 나누는 과정을 반복합니다.

2. 정복(Conquer)

   - 각 하위 배열을 재귀적으로 정렬합니다. 이 과정에서 배열의 크기가 1이면 정렬된 것으로 간주합니다.

3. 병합(Merge)

   - 두 개의 정렬된 하위 배열을 하나의 정렬된 배열로 병합합니다.

> 분할 단계를 배열의 크기가 1이 될 때까지 반복하는 이유는 **재귀적 구현**을 위한 것입니다.
{: .prompt-info}

## Pseudo Code

```python
MERGE-SORT(array, left, right)
    If left < right
        middle = (left + right) / 2
        MERGE-SORT(array, left, middle)
        MERGE-SORT(array, middle + 1, right)
        MERGE(array, left, middle, right)

MERGE(array, left, middle, right)
    n1 = middle - left + 1
    n2 = right - middle
    Create temporary arrays L[1 ... n1] and R[1 ... n2]
    For i = 1 to n1
        L[i] = array[left + i - 1]
    For j = 1 to n2
        R[j] = array[middle + j]
    i = 1, j = 1, k = left
    While i <= n1 and j <= n2
        If L[i] <= R[j]
            array[k] = L[i]
            i = i + 1
        Else
            array[k] = R[j]
            j = j + 1
        k = k + 1
    Copy remaining elements of L[], if any
    While i <= n1
        array[k] = L[i]
        i = i + 1
        k = k + 1
    Copy remaining elements of R[], if any
    While j <= n2
        array[k] = R[j]
        j = j + 1
        k = k + 1
```

## 성능

### 시간 복잡도

병합 정렬의 시간 복잡도는 항상 $O(n\log n)$입니다. 여기서 $n$은 배열의 원소 개수입니다.

- 배열을 병합하는 과정은 $\log n$ 단계가 필요합니다.

- 각 병합 단계에서는 모든 원소를 한 번씩만 검사하게 되므로 $O(n)$만큼 시간이 소요됩니다.

### 공간 복잡도

병합 정렬은 **추가적인 배열 공간**을 필요로 하므로 공간 복잡도는 $O(n)$입니다. 이는 정렬된 하위 배열들을 저장하기 위해 **임시 메모리**가 필요하기 때문입니다.


## 예시

### 문제 1

- <kbd>5, 6, 4, 7, 2, 1, 3</kbd>

#### 분할

배열이 하나의 원소가 될 때까지(정복될 때까지) 주어진 배열을 두 개의 하위 배열로 나누는 과정을 반복한다.

- <kbd>5, 6, 4, 7</kbd><kbd>2, 1, 3</kbd>
- <kbd>5, 6</kbd><kbd>4, 7</kbd><kbd>2, 1</kbd><kbd>3</kbd>
- <kbd>5</kbd><kbd>6</kbd><kbd>4</kbd><kbd>7</kbd><kbd>2</kbd><kbd>1</kbd><kbd>3</kbd>

#### 정복 & 병합

나누었던 하위 배열을 **정렬 순서를 고려하여** 하나의 배열로 다시 병합한다.

- [<kbd>5</kbd><kbd>6</kbd>]<kbd>4</kbd><kbd>7</kbd><kbd>2</kbd><kbd>1</kbd><kbd>3</kbd>
- <kbd>5, 6</kbd>[<kbd>4</kbd><kbd>7</kbd>]<kbd>2</kbd><kbd>1</kbd><kbd>3</kbd>
- [<kbd>5, 6</kbd><kbd>4, 7</kbd>]<kbd>2</kbd><kbd>1</kbd><kbd>3</kbd>
- <kbd>4, 5, 6, 7</kbd>[<kbd>2</kbd><kbd>1</kbd>]<kbd>3</kbd>
- <kbd>4, 5, 6, 7</kbd><kbd>1, 2</kbd>[<kbd>3</kbd>]
- [<kbd>4, 5, 6, 7</kbd><kbd>1, 2, 3</kbd>]
- <kbd>1, 2, 3, 4, 5, 6, 7</kbd>

#### 풀이 애니메이션

> 출처: http://algorithm.wiki/

![merge-sort](/assets/img/merge-sort.gif)

## 참고

- 구종만, 알고리즘 문제 해결 전략 1, 인사이트(2012)
- 윤성우, 열혈 자료구조, 오렌지미디어(2012)