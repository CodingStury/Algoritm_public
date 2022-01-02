> 날짜: 2021-12-12
>
> 작성자: 송명석

## 문제 내용

https://programmers.co.kr/learn/courses/30/lessons/81302

거리두기 확인하기 - 2021 카카오 채용연계형 인턴십



## 문제의 핵심 및 중요 포인트

DFS로 상하좌우의 칸을 탐색하는 것이 중요 포인트이다. DFS로 상하좌우를 탐색하면 **대각선 방향의 칸은 자동으로 탐색된다.**

탐색은 P에서부터만 시작하고, **각 탐색 시 방문배열을 꼭 초기화**해주어야 한다. 그리고 탐색 중 X를 만나면 탐색을 종료하고, 맨해튼 거리 내에 P를 만나면 unsafe로 판단한다. 그리고 바로 해당 place 탐색을 종료하고 다음 place 탐색을 시작한다.

탐색 중 맨해튼 거리를 넘어가면 안전이므로, 다음 P를 찾아서 탐색을 진행한다.



## 개선해야 할 점

DFS 풀이법을 잘 알지 못해, 삽질하다가 답안을 참조하면서 풀었다.

**DFS와 BFS 관련 문제들을 여러 개 풀어보아야 겠다.**



## 소스 코드

```c++
#include <string>
#include <vector>
#include <cstring>
#include <iostream>

using namespace std;

#define NUM_ROWS 5
#define NUM_COLS 5
#define SAFE 1
#define UNSAFE 0

int g_visited[NUM_ROWS][NUM_COLS] = {0,};
int g_dx[] = {0, 1, 0, -1};
int g_dy[] = {-1, 0, 1, 0};
int g_IsSafe = SAFE;


// 배열 범위 내에 있는 위치인지 확인
bool IsInRange(int posX, int posY)
{
    if ((0 <= posX && posX < NUM_ROWS) && (0 <= posY && posY < NUM_COLS))
    {
        return true;
    }
    else
    {
        return false;
    }
}

void DFS(int posX, int posY, int depth, const vector<string>& place)
{
    if (depth > 2 || UNSAFE == g_IsSafe)
    {
        return;
    }

    if (0 < depth && 'P' == place[posX][posY])
    {
        g_IsSafe = UNSAFE;
        return;
    }
    
    g_visited[posX][posY] = 1;
    for (int i = 0; i < (sizeof(g_dx)/sizeof(*g_dx)); i++)
    {
        int nextPosX = posX + g_dx[i];
        int nextPosY = posY + g_dy[i];
        if (true == IsInRange(nextPosX, nextPosY) && 'X' != place[nextPosX][nextPosY])
        {
            if (0 == g_visited[nextPosX][nextPosY])
            {
                DFS(nextPosX, nextPosY, depth + 1, place);
            }
        }
    }
}

vector<int> solution(vector<vector<string>> places)
{
    vector<int> answer;
    vector<vector<string>>::iterator itrPlaces;
    bool bStopFlag = false;
    
    for (itrPlaces = places.begin(); itrPlaces != places.end(); itrPlaces++)
    {
        vector<string> place = *itrPlaces;
        bStopFlag = false;
        g_IsSafe = SAFE;
        
        for(int posX = 0; posX < NUM_ROWS; posX++)
        {
            for(int posY = 0; posY < NUM_COLS; posY++)
            {
                // 탐색은 'P'에서부터만 시작한다.
                if ('P' == place[posX][posY])
                {
                    // 방문 배열은 각 탐색마다 초기화해주어야 한다.
                    // 한 예로, P(0,0)에서 시작한 탐색이 P(0,1) P(1,1)를 방문했다 하더라도,
                    // P(2,1)에서 시작한 탐색이 P(1,1)을 방문해야 하는 경우가 생기기 때문이다.
                    memset(g_visited, 0, sizeof(g_visited));
                    DFS(posX, posY, 0, place);
                    
                    // unsafe라면 해당 place는 더 이상 탐색할 필요가 없다.
                    if (UNSAFE == g_IsSafe)
                    {
                        bStopFlag = true;
                        break;
                    }
                }
            }
            if (true == bStopFlag)
            {
                break;
            }
        }
        
        if (SAFE == g_IsSafe)
        {
            answer.push_back(1);
        }
        else
        {
            answer.push_back(0);
        }
    }
    
    return answer;
}
```
