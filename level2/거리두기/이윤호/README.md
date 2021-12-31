> 날짜: 2021-12-18
> 작성자: 이윤호

## 문제 내용

https://programmers.co.kr/learn/courses/30/lessons/81302?language=cpp



## 문제의 핵심 및 중요 포인트

경우의수 찾기, 



## 개선해야 할 점

시간 너무 오래걸림. IF문 으로 무지성 경우의수 찾기,





## 소스 코드

```c++
#include <string>
#include <vector>

using namespace std;

vector<int> solution(vector<vector<string>> places) {
    vector<int> answer;

    for(int i = 0; i <places.size(); ++i)
    {
        vector<pair<int, int>> pos;

        for(int x = 0; x < places[i].size(); ++x)
        {
	        for(int y = 0; y < places[i][x].size(); ++y)
	        {
		        if(places[i][x][y] == 'P')
		        {
                    pos.emplace_back(make_pair(x, y));
		        }
	        }
        }

        bool success = true;

        if(pos.size() == 0)
        {
            goto end;
        }

        for(int x = 0; x < pos.size() - 1; ++x)
        {
			for(int y = x + 1; y < pos.size(); ++y)
			{
                const int x1 = pos[x].first;
                const int y1 = pos[x].second;
                const int x2 = pos[y].first;
                const int y2 = pos[y].second;

                const int distance = abs(x1 - x2) + abs(y1 - y2);

                //맨해튼이 2보다 작을때
                if(distance < 2)
                {
                    success = false;
                    goto end;
                }
                else if(distance == 2)
                {
                    if(abs(x1 - x2) == 1)
                    {
                        if(y1 > y2)
                        {
                            if (!(places[i][x1][y1 - 1] == 'X' && places[i][x2][y2 + 1] == 'X'))
                            {
                                success = false;
                                goto end;
                            }
                        }
                        else
                        {
                            if (!(places[i][x1][y1 + 1] == 'X' && places[i][x2][y2 - 1] == 'X'))
                            {
                                success = false;
                                goto end;
                            }
                        }
                    }
                    else if(x1 + 2 == x2)
                    {
                        if(places[i][x1 + 1][y1] != 'X')
                        {
                            success = false;
                            goto end;
                        }
                    }
                    else
                    {
                        if (places[i][x1][y1 + 1] != 'X')
                        {
                            success = false;
                            goto end;
                        }
                    }
                }
			}
        }

    end:
        if(success)
        {
            answer.emplace_back(1);
        }
        else
        {
            answer.emplace_back(0);
        }
    }

   
    return answer;
}
```

