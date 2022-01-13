> 날짜: 2021-12-18
> 작성자: 송명석

## 문제 내용

https://programmers.co.kr/learn/courses/30/lessons/72412



## 문제의 핵심 및 중요 포인트

DB검색을 얼마나 효율적으로 빠르게 도출할수 있는지 묻는 문제



## 개선해야 할 점

set은 절대 사용하지 말자. 차라리 vector로 값을 받은후 나중에 sort한번 쳐주는게 더 빠르다.

메모리를 16배 더 사용해서라도 배열을 이용해서 1사이클에 탐색 가능하게 하는게 요즘 코테에서는 잘먹힌다.



## 소스 코드

```c++
#define _CRT_SECURE_NO_WARNINGS

#include <string>
#include <sstream>
#include <vector>
#include <unordered_map>
#include <map>
#include <algorithm>
using namespace std;

constexpr int cpp       = 0;  
constexpr int java      = 1; 
constexpr int python    = 2; 
constexpr int lang      = 3; 
constexpr int backend   = 0; 
constexpr int frontend  = 1; 
constexpr int job       = 2; 
constexpr int junior    = 0; 
constexpr int senior    = 1; 
constexpr int career    = 2; 
constexpr int chicken   = 0; 
constexpr int pizza     = 1; 
constexpr int food      = 2; 

vector<int> h[4][3][3][3];

vector<int> solution(vector<string> info, vector<string> query) {
    vector<int> answer;
    unordered_map<string, int> hindex;
        

    hindex.insert({ "cpp"      ,cpp });
    hindex.insert({ "java"     ,java });
    hindex.insert({ "python"   ,python });
    hindex.insert({ "lang"     ,lang });
    hindex.insert({ "backend"  ,backend });
    hindex.insert({ "frontend" ,frontend });
    hindex.insert({ "job"      ,job });
    hindex.insert({ "junior"   ,junior });
    hindex.insert({ "senior"   ,senior });
    hindex.insert({ "career"   ,career });
    hindex.insert({ "chicken"  ,chicken });
    hindex.insert({ "pizza"    ,pizza });
    hindex.insert({ "food"     ,food });


    for (int i = 0; i < info.size(); ++i)
    {
        stringstream ss(info[i]);
        string temp;
        vector<string> split;

        while (getline(ss, temp, ' '))
        {
            split.emplace_back(temp);
        }
        int i0 = hindex.at(split[0]);
        int i1 = hindex.at(split[1]);
        int i2 = hindex.at(split[2]);
        int i3 = hindex.at(split[3]);
        int key = stoi(split[4]);

        h[i0][i1][i2][i3].push_back(key);

        h[3][i1][i2][i3].push_back(key);
        h[i0][2][i2][i3].push_back(key);
        h[i0][i1][2][i3].push_back(key);
        h[i0][i1][i2][2].push_back(key);

        h[3][2][i2][i3].push_back(key);
        h[3][i1][2][i3].push_back(key);
        h[3][i1][i2][2].push_back(key);
        h[i0][2][2][i3].push_back(key);
        h[i0][2][i2][2].push_back(key);
        h[i0][i1][2][2].push_back(key);

        h[3][2][2][i3].push_back(key);
        h[3][2][i2][2].push_back(key);
        h[3][i1][2][2].push_back(key);
        h[i0][2][2][2].push_back(key);

    	h[3][2][2][2].push_back(key);
    }

    for (int x = 0; x < 4; ++x)
        for (int y = 0; y < 3; ++y)
            for (int z = 0; z < 3; ++z)
                for (int w = 0; w < 3; ++w)
                    sort(h[x][y][z][w].begin(), h[x][y][z][w].end());

    
    for (int i = 0; i < query.size(); ++i)
    {
        stringstream ss(query[i]);
        string temp;
        vector<string> qu;
        
        int value = 0;
        int c = -1;
        int key = -1;

        int i0 = 0;
        int i1 = 0;
        int i2 = 0;
        int i3 = 0;
        
        while (getline(ss, temp, ' '))
        {
            ++c;
            if(c == 7)
            {
                key = stoi(temp);
                break;
            }

            if (temp == "and") continue;

            if(temp == "-")
            {
	            if(c == 0)
	            {
                    i0 = 3;
	            }
                else if (c == 2)
                {
                    i1 = 2;
                }
                else if (c == 4)
                {
                    i2 = 2;
                }
                else if (c == 6)
                {
                    i3 = 2;
                }
            }
            else
            {
                if (c == 0)
                {
                    i0 = hindex.at(temp);
                }
                else if (c == 2)
                {
                    i1 = hindex.at(temp);
                }
                else if (c == 4)
                {
                    i2 = hindex.at(temp);
                }
                else if (c == 6)
                {
                    i3 = hindex.at(temp);
                }
            }
                
            
        }

        bool find = false;
        int si = 0;
        for(auto i = h[i0][i1][i2][i3].begin(); i != h[i0][i1][i2][i3].end(); ++i)
        {
	        if(*i >= key)
	        {
                find = true;
                break;
	        }
            ++si;
        }


        if(find == false)
        {
            answer.emplace_back(0);
            continue;
        }

        answer.emplace_back(h[i0][i1][i2][i3].size() - si);
    }
    

    return answer;
}

int main()
{
    vector<string> info;
    vector<string> query;

    info.emplace_back("java backend junior pizza 150");
    info.emplace_back("python frontend senior chicken 210");
    info.emplace_back("python frontend senior chicken 150");
    info.emplace_back("cpp backend senior pizza 260");
    info.emplace_back("java backend junior chicken 80");
    info.emplace_back("python backend senior chicken 50");

    query.emplace_back("java and backend and junior and pizza 100");
    query.emplace_back("python and frontend and senior and chicken 200");
    query.emplace_back("cpp and - and senior and pizza 250");
    query.emplace_back("- and backend and senior and - 150");
    query.emplace_back("- and - and - and chicken 100");
    query.emplace_back("- and - and - and - 100");

    auto s = solution(info, query);


    return 0;
}
```

