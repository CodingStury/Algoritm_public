## 문제 내용

`https://programmers.co.kr/learn/courses/30/lessons/72412`

## 문제의 핵심 및 중요 포인트

`문제를 풀 때 사용한 알고리즘이나 전략, 중요 포인트 등을 기입`

dict에 list index를 저장해 두고 쿼리 결과로 나온 index list 를 교집합 해 list len을 반환하자

## 개선해야 할 점

`문제를 풀고 난 후, 자신의 개선해야 할 점이 무엇인지 기입`

`e.g. '풀이 시간이 너무 길었다', '정렬 알고리즘 개념에 대해 공부가 필요하다' etc...`

자료구조와 탐색 알고리즘에 문제가 있었음.

## 소스 코드

```python
def solution(info, query):
    answer = []
    lang = {}
    position = {}
    career = {}
    soul = {}
    score = {}
    info_dict_list = [lang, position, career, soul, score]
    for info_index, info_item in enumerate(info):
        for index, item in enumerate(info_item.split(' ')):
            if index == 4:
                item = int(item)
            if item in info_dict_list[index]:
                info_dict_list[index][item].append(info_index)
            else:
                info_dict_list[index][item] = [info_index]

    for query_item in query:
        query_result_set = set(list(range(len(info))))
        query_list = list(enumerate(query_item.replace(' and ', ' ').split(' ')))
        for index, item in query_list[:-1]:
            if item == '-':
                continue
            query_result_set = query_result_set & set(info_dict_list[index][item])
        # 수는 따로 합시다
        over_score_key_list = [x for x in score.keys() if x >= int(query_list[-1][1])]
        over_score_index = []
        for i in over_score_key_list:
            over_score_index += score[i]

        query_result_set = query_result_set & set(over_score_index)
        answer.append(len(list(query_result_set)))
    return answer
```

효율성에서 개발림

# 다른 방법에서 궁금한점

어떻게 푼걸까? - 1. 자료구조에 문제가 있었음 2. 끝에 탐색할 때 이진 탐색 실시

### 1. 자료 구조

```python
data = {}
for a in ['cpp', 'java', 'python', '-']:
    for b in ['backend', 'frontend', '-']:
        for c in ['junior', 'senior', '-']:
            for d in ['chicken', 'pizza', '-']:
                data.setdefault((a, b, c, d), list())
for i in info:
    i = i.split()
    for a in [i[0], '-']:
        for b in [i[1], '-']:
            for c in [i[2], '-']:
                for d in [i[3], '-']:
                    data[(a, b, c, d)].append(int(i[4]))
```

튜플 그 자체로 dict의 키가 될 수 있다는 사실이 놀랍다. 자주 써먹을 수 있는 구조인듯

### 이진 탐색

```python
pool = data[tuple(q[:-1])]
find = int(q[4])
l = 0
r = len(pool)
mid = 0
while l < r:
    mid = (r + l) // 2
    if pool[mid] >= find:
        r = mid
    else:
        l = mid + 1
answer.append(len(pool) - l)
```

아에 쓸 생각을 못했다 - 탐색 알고리즘 공부 필요

# 다시 푼 풀이법

```python

def solution_2(info, query):
    answer = []
    data = {}
    for a in ['cpp', 'java', 'python', '-']:
        for b in ['backend', 'frontend', '-']:
            for c in ['junior', 'senior', '-']:
                for d in ['chicken', 'pizza', '-']:
                    data.setdefault((a, b, c, d), list())
    for i in info:
        i = i.split()
        for a in [i[0], '-']:
            for b in [i[1], '-']:
                for c in [i[2], '-']:
                    for d in [i[3], '-']:
                        data[(a, b, c, d)].append(int(i[4]))

    for k in data:
        data[k].sort()

    for query_item in query:
        q = query_item.replace(' and ', ' ').split(' ')

        pool = data[tuple(q[:-1])]
        find = int(q[4])
        l = 0
        r = len(pool)
        mid = 0
        while l < r:
            mid = (r + l) // 2
            if pool[mid] >= find:
                r = mid
            else:
                l = mid + 1
        answer.append(len(pool) - l)
    return answer
```

ㄹㅇ 완패 공부 더 하자