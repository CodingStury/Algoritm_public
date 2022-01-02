> 날짜: 2022-01-13
> 작성자: 송명석

## 문제 내용

https://www.acmicpc.net/problem/2667



## 문제의 핵심 및 중요 포인트

깊이 또는 넓이 기반 탐색을 응용한 문제



## 개선해야 할 점

아이디어 구상은 빨랐는데 이를 구현하는데 시간을 다 잡아먹음

많은 연습으로 배열 다루는 속도를 높여야함



## 소스 코드

```c++
#define _CRT_SECURE_NO_WARNINGS
#include <string>
#include <vector>
#include <stack>
#include <algorithm>
#include <cstring>

using namespace std;


int counting(bool* stemp, char* bmap, int size, int y , int x)
{
    int count = 1;
    stack<pair<int, int>> spos;

    int uIndex = (y - 1) * size + x;
    int rIndex = y * size + x + 1;
    int dIndex = (y + 1) * size + x;
    int lIndex = y * size + x - 1;

    char cu = bmap[uIndex];
    char cr = bmap[rIndex];
    char cd = bmap[dIndex];
    char cl = bmap[lIndex];

    bool bu = stemp[uIndex];
    bool br = stemp[rIndex];
    bool bd = stemp[dIndex];
    bool bl = stemp[lIndex];


    if (!bu && cu == '1')
    {
        spos.push(make_pair(x, y - 1));
        stemp[uIndex] = true;
        ++count;
    }
    if (!br && cr == '1')
    {
        spos.push(make_pair(x + 1, y));
        stemp[rIndex] = true;
        ++count;
    }
    if (!bd && cd == '1')
    {
        spos.push(make_pair(x, y + 1));
        stemp[dIndex] = true;
        ++count;
    }
    if (!bl && cl == '1')
    {
        spos.push(make_pair(x - 1, y));
        stemp[lIndex] = true;
        ++count;
    }


    while(!spos.empty())
    {
	    for(size_t i = 0; i < spos.size(); ++i)
	    {
            pair<int, int> nextPos = spos.top();
            int nx = nextPos.first;
            int ny = nextPos.second;
            spos.pop();

            int nuIndex = (ny - 1) * size + nx;
            int nrIndex = ny * size + nx + 1;
            int ndIndex = (ny + 1) * size + nx;
            int nlIndex = ny * size + nx - 1;

            char ncu = bmap[nuIndex];
            char ncr = bmap[nrIndex];
            char ncd = bmap[ndIndex];
            char ncl = bmap[nlIndex];

            bool nbu = stemp[nuIndex];
            bool nbr = stemp[nrIndex];
            bool nbd = stemp[ndIndex];
            bool nbl = stemp[nlIndex];

            if (!nbu && ncu == '1')
            {
                spos.push(make_pair(nx, ny - 1));
                stemp[nuIndex] = true;
                ++count;
            }
            if (!nbr && ncr == '1')
            {
                spos.push(make_pair(nx + 1, ny));
                stemp[nrIndex] = true;
                ++count;
            }
            if (!nbd && ncd == '1')
            {
                spos.push(make_pair(nx, ny + 1));
                stemp[ndIndex] = true;
                ++count;
            }
            if (!nbl && ncl == '1')
            {
                spos.push(make_pair(nx - 1, ny));
                stemp[nlIndex] = true;
                ++count;
            }
	    }
    }
    
    return count;
}


vector<int> solution(int size, vector<string> map) {
    vector<int> answer;

    char* bmap = new char[(size + 2) * (size + 2)];
    memset(bmap, '0', sizeof(char) * (size + 2) * (size + 2));

    bool* stemp = new bool[(size + 2) * (size + 2)];
    memset(stemp, false, sizeof(bool) * (size + 2) * (size + 2));

    for(int i = 0; i < size  ; ++i)
    {
	    for(int j = 0; j < size; ++j)
	    {
	    	bmap[(i + 1) * (size + 2) + j + 1] = map[i][j];
	    }
    }

    for(int i = 1; i < size + 1; ++i)
    {
	    for(int j = 1; j < size + 1; ++j)
	    {
		    if(stemp[i * (size + 2) + j] == false)
		    {
                stemp[i * (size + 2) + j] = true;

                if(bmap[i * (size + 2) + j] == '1')
                {
                    answer.emplace_back(counting(stemp, bmap, size + 2, i, j));
                }

		    }
	    }
    }



    return answer;
}

int main()
{
    int size;
    scanf("%d", &size);

    vector<string> s;
    for(int i = 0; i < size; ++i)
    {
        char *temp = new char[size + 1];
        scanf("%s", temp);
        s.emplace_back(temp);
    }

    auto answer = solution(size, s);
    printf("%d\n", answer.size());

    sort(answer.begin(), answer.end());
    for(int i =0; i < answer.size(); ++i)
    {
        printf("%d\n", answer[i]);
    }

    return  0;
}
```

