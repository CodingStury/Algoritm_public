> 날짜: 2021-01-07
>
> 작성자: 송명석

## 문제 내용

https://www.acmicpc.net/problem/2667

단지번호 붙이기



## 문제의 핵심 및 중요 포인트

시작 탐색 지점에서 상하좌우로 BFS 탐색을 진행한다.

만약 다음 탐색 지점에 집이 있다면, 큐에 추가함과 동시에 방문체크, 카운팅을 수행하는 것이 중요하다.

주의: 시작 탐색 지점에서도 집의 개수를 +1 해주어야 한다.



## 개선해야 할 점

처음 풀어보는 BFS 문제여서 시간이 오래 걸렸다. 또한 자잘한 풀이 설계 실수도 해버려서, 디버깅에 시간을 잡아먹었다.

비슷한 문제들을 많이 풀어서, 풀이에 익숙해져야겠다.



## 소스 코드

```c++
#include <iostream>
#include <string>
#include <vector>
#include <queue>
#include <algorithm>

using namespace std;

int g_dx[4] = {0, 0, -1, 1};
int g_dy[4] = {-1, 1, 0, 0};


void GetInput(int& n, vector<vector<int>>& MyMap)
{
    vector<int> temp;
    string inputString = "";

    cin >> n;
    for (int i = 0; i < n; i++)
    {
        inputString.clear();
        temp.clear();

        cin >> inputString;
        for (int j = 0; j < n; j++)
        {
            temp.push_back(inputString[j] - '0');
        }
        MyMap.push_back(temp);
    }
}

void Init(vector<vector<bool>>& visited, int n)
{
    vector<bool> temp;

    for (int i = 0; i < n; i++)
    {
        temp.clear();
        for (int j = 0; j < n; j++)
        {
            temp.push_back(false);
        }
        visited.push_back(temp);
    }
}

bool IsInRange(int x, int y, int edge)
{
    if ((0 <= x && x < edge) && (0 <= y && y < edge))
    {
        return true;
    }
    else
    {
        return false;
    }
}

int GetNumHouse(int i, int j, const vector<vector<int>>& myMap, vector<vector<bool>>& visited)
{
    int numHouse = 0;
    queue<vector<int>> searchQueue;

    vector<int> startCoord;
    startCoord.push_back(i);
    startCoord.push_back(j);

    searchQueue.push(startCoord);
    visited[i][j] = true;
    numHouse++;

    // BFS 탐색 시작
    while (false == searchQueue.empty())
    {
        // 큐의 요소를 꺼낸다.
        vector<int> currCoord = searchQueue.front();
        searchQueue.pop();
        int currX = currCoord[0];
        int currY = currCoord[1];

        // 상하좌우 탐색하여 1일 때에만 큐에 집어넣으면서 방문체크
        for (int i = 0; i < 4; i++)
        {
            int nextX = currCoord[0] + g_dx[i];
            int nextY = currCoord[1] + g_dy[i];

            if (true == IsInRange(nextX, nextY, myMap.size())
                && false == visited[nextX][nextY]
                && 1 == myMap[nextX][nextY])
            {
                searchQueue.push(vector<int>{nextX, nextY});
                visited[nextX][nextY] = true;
                numHouse++;
            }
            else
            {
                // Do nothing...
            }
        }
    }

    return numHouse;
}

int main()
{
    int n = 0;
    vector<vector<int>> myMap;
    vector<vector<bool>> visited;
    int numHouse = 0;                 // 단지 내 집의 수
    vector<int> answer;          // 단지 내 집의 수를 저장하는 벡터

    // 입력값 n과 배열 데이터 받기
    GetInput(n, myMap);

    // 방문 벡터 초기화
    Init(visited, n);

    // (0, 0)부터 탐색 시작
    // 만약 0이거나 방문한 칸이면 continue
    for (int i = 0; i < n; i++)
    {
        for (int j = 0; j < n; j++)
        {
            if (0 == myMap[i][j] || true == visited[i][j])
            {
                continue;
            }
            else
            {
                numHouse = GetNumHouse(i, j, myMap, visited);
                answer.push_back(numHouse);
            }
        }
    }

    // count vector를 오름차순 정렬
    sort(answer.begin(), answer.end());

    // 총 단지수 출력
    cout << answer.size() << endl;
    
    // 한 줄에 하나씩 출력
    for (int i = 0; i < answer.size(); i++)
    {
        cout << answer[i] << endl;
    }

    return 0;
}
```
