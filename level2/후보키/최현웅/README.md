> 날짜: 2022-01-20
> 작성자: 최현웅

## 문제 내용

<https://programmers.co.kr/learn/courses/30/lessons/42890>



## 문제의 핵심 및 중요 포인트

모든 Feature에 대한 경우의 수를 뽑아서 중복이 없는 조합을 찾는 것이 핵심이나, 로직상
Feature 이름에 대한 경우의 수를 뽑고 나서, 해당하는 레코드를 알맞게 가져오는게 힘들었음

## 개선해야 할 점
첫 번째로 풀때 소스코드가 매우 복잡했는데, 구글링을 통해서 깔끔하게 코드를 개선했다1.
구글링의 힘을 점점 덜 빌려야 하는데 이런 것들에 대해서 빠르게 익숙해질 필요가 있다.

## 소스 코드

```python
from itertools import combinations

def solution(relation):
    n_row = len(relation)
    n_col = len(relation[0])

    candidate = list()
    for i in range(1, n_col+1):
        candidate.extend(combinations(range(n_col), i))  
        # 모든 경우의 수 저장

    final = []
    for keys in candidate:
        tmp = [tuple([item[key] for key in keys]) for item in relation] 
        # 주어진 키로 리스트의 index별 아이템 뽑아내기
        if len(set(tmp)) == n_row: 
            #set로 변경후 사라진게 없다면 key로 사용해도 무방
            final.append(keys)

    answer = set(final[:])
    for i in range(len(final)):
        for j in range(i+1, len(final)):
            if len(final[i]) == len(set(final[i]).intersection(set(final[j]))):
                # key 중에 겹치는 부분이 있는 것을 삭제
                answer.discard(final[j])
    return(len(answer))
```
