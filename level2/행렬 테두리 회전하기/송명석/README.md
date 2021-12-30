> 날짜: 2021-12-30
>
> 작성자: 송명석

## 문제 내용

https://programmers.co.kr/learn/courses/30/lessons/77485



## 문제의 핵심 및 중요 포인트

두 가지의 로직을 잘 구현하면 풀 수 있는 문제다.

- 직사각형 모양의 범위 중에서 테두리 칸만 선택하는 로직:

  (x1, y1), (x2, y2)의 x, y좌표값을 이용하면 테두리 칸을 쉽게 구할 수 있다.

- 테두리칸을 시계방향으로 한 칸 회전시키는 로직:

  해당 칸이 위쪽 테두리에 위치하고, x값이 x2보다 작으면 오른쪽으로 한 칸 이동.

  해당 칸이 오른쪽 테두리에 위치하고, y값이 y2보다 작으면 아래칸으로 한 칸 이동.

  ...

  위와 같은 로직으로 회전시킬 수 있다.



### 주의사항:

queries의 값들은 0 index 기준으로 되어있는 값들이 아니므로, 코딩할 때 for문 범위나 인덱스 값을 설정할 때 주의해야 한다.



## 개선해야 할 점

풀이 시간은 약 2시간으로 전보다는 줄었지만 더 단축해야 한다.

30분은 자잘한 범위 설정 실수로 디버깅하는데 잡아먹었다.

손글씨로 풀이과정을 적기엔 시간이 꽤 소요되니, 코딩할 때 주석으로 세세히 적어놓고 코딩해봐야겠다. 코딩 다 끝나면 주석 정리하는 식으로.



## 소스 코드

```c++
#include <string>
#include <vector>

using namespace std;

#define IDX_X1 0
#define IDX_Y1 1
#define IDX_X2 2
#define IDX_Y2 3
#define MAX_INT 10000

bool IsBorder(int x, int y, const vector<int>& query)
{
    int x1 = query[IDX_X1];
    int y1 = query[IDX_Y1];
    int x2 = query[IDX_X2];
    int y2 = query[IDX_Y2];
    
    if (x1 == x || x2 == x || y1 == y || y2 == y)
    {
        return true;
    }
    else
    {
        return false;
    }
}

int RotateBorderAndGetMinVal(vector<vector<int>>& matrix, const vector<int>& query)
{    
    if (0 == matrix.size() || 4 != query.size())
    {
        return -1;
    }
    
    vector<vector<int>> tempMatrix = matrix;
    int minValue = MAX_INT;
    // 0을 기준으로 한 값
    int x1 = query[IDX_X1] - 1;
    int y1 = query[IDX_Y1] - 1;
    int x2 = query[IDX_X2] - 1;
    int y2 = query[IDX_Y2] - 1;
    
    for (int x = x1; x <= x2; x++)
    {     
        for (int y = y1; y <= y2; y++)
        {
            // 테두리 칸이 아니면 continue
            // x, y 는 현재 0부터 시작하는 index 기준으로 되어있으므로 +1 해준 것이다.
            if (false == IsBorder(x + 1, y + 1, query))
            {
                continue;
            }
            
            // 회전시키기
            // x1(위쪽)이고, y2보다 작으면 오른쪽 이동
            if (x1 == x && y2 > y)
            {
                matrix[x][y + 1] = tempMatrix[x][y];
            }
            // y2(오른쪽)이고, x2보다 작으면 아래쪽 이동
            else if (y2 == y && x2 > x)
            {
                matrix[x + 1][y] = tempMatrix[x][y];
            }
            // x2(아래쪽)이고, y1보다 크면 왼쪽 이동
            else if (x2 == x && y1 < y)
            {
                matrix[x][y - 1] = tempMatrix[x][y];
            }
            // y1(왼쪽)이고, x1보다 크면 위쪽 이동
            else if (y1 == y && x1 < x)
            {
                matrix[x - 1][y] = tempMatrix[x][y];
            }
            
            // 그리고 최솟값 업데이트
            if (minValue > tempMatrix[x][y])
            {
                minValue = tempMatrix[x][y];
            } 
        }
    }
    
    return minValue;
}

vector<int> solution(int rows, int columns, vector<vector<int>> queries)
{
    vector<int> answer;
    
    vector<vector<int>> matrix;
    vector<int> tempVector;
    // 행렬 생성
    int cnt = 1;
    for (int i = 0; i < rows; i++)
    {
        tempVector.clear();
        for (int j = 0; j < columns; j++)
        {
            tempVector.push_back(cnt);
            cnt++;
        }
        matrix.push_back(tempVector);
    }
    
    // 최솟값 구하기
    for (int i = 0; i < queries.size(); i++)
    {
        vector<int>& query = queries[i];
        
        answer.push_back(
            RotateBorderAndGetMinVal(matrix, query)
        );    
    }
    
    return answer;
}
```
