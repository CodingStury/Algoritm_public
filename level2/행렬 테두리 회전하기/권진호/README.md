> 날짜: 2021-12-27
> 작성자: 권진호 

## 문제 내용
https://programmers.co.kr/learn/courses/30/lessons/77485


## 문제의 핵심 및 중요 포인트
rows, cols를 통해 미리 행렬을 만들어둔다.

지정된 query를 list로 만든 다음, 만들어 둔 행렬의 요소를 지정해 해당 위치의 값 리스트를 생성한다

값 리스트는 변경될 요소이기 때문에 이 안에 있는 값중 가장 작은 값을 answer에 추가해 준다

value를 한칸씩 뒤로 민 다음에 이 값들을 만들어 둔 행렬에 삽입해 준다

## 개선해야 할 점

row, col 혼동이 발생해 오래 걸렸다. 다음엔 주의하자


## 소스 코드

```python
def solution(rows, columns, queries):
    answer = []
    solve_map = [[(i * columns + j + 1) for j in range(columns)] for i in range(rows)]

    # row : x  | col : y
    def create_rotate_index_list(query):
        min_row, max_row = min(query[0], query[2]) - 1, max(query[0], query[2]) - 1
        min_col, max_col = min(query[1], query[3]) - 1, max(query[1], query[3]) - 1

        result_list = []
        for i in range(min_col, max_col):  # 윗쪽
            result_list.append([min_row, i])

        for i in range(min_row, max_row):  # 오른쪽
            result_list.append([i, max_col])

        for i in range(max_col, min_col, -1):  # 아랫쪽
            result_list.append([max_row, i])

        for i in range(max_row, min_row, -1):  # 왼쪽
            result_list.append([i, min_col])

        return result_list

    for query in queries:
        rotate_index_list = create_rotate_index_list(query)
        value_list = [solve_map[i[0]][i[1]] for i in rotate_index_list]
        answer.append(min(value_list))

        value_list = [value_list[-1]] + value_list[:-1]
        for index, value in zip(rotate_index_list, value_list):
            solve_map[index[0]][index[1]] = value
    return answer
```

