> 날짜: 2021-01-07
>
> 작성자: 송명석

## 문제 내용

https://programmers.co.kr/learn/courses/30/lessons/12982

예산



## 문제의 핵심 및 중요 포인트

정렬이 핵심



## 개선해야 할 점

문제를 대강 읽어서 삽질을 해버렸다. 예산을 남기지 말고 전부 써야만 하는 줄 알고 DFS 탐색 쓰는 등....

문제가 길어도 꼼꼼이 읽어야겠다.


## 소스 코드

```c++
#include <string>
#include <vector>
#include <algorithm>

using namespace std;

int solution(vector<int> d, int budget)
{
    int answer = 0;
    int i = 0;
    int budgetSum = 0;
    
    // d를 오름차순 정렬한다.
    sort(d.begin(), d.end());
    
    // budget으로 지원 가능한 최대 부서 수를 구한다.
    for (i = 0; i < d.size(); i++)
    {
        budgetSum += d[i];
        if (budget < budgetSum)
        {
            break;
        }
    }
    
    answer = i;
    
    return answer;
}
```

