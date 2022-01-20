> 날짜: 2021-01-17
>
> 작성자: 송명석

## 문제 내용

https://programmers.co.kr/learn/courses/30/lessons/72412

순위 검색



## 문제의 핵심 및 중요 포인트

map 자료구조 활용 + 정렬을 이용한 지원자 수 구하는 게 핵심.

효율성 테스트가 있으므로 각 쿼리마다 info 배열을 모두 탐색하는 방법은 잘못된 방법이다.



## 개선해야 할 점

여전히 실수가 잦다.

istringstream 활용법을 공부해놓아야 겠다.

lower_bound, upper_bound 함수 사용법도 외워두자.



## 소스 코드

```c++
#include <string>
#include <iostream>
#include <vector>
#include <sstream>
#include <algorithm>
#include <map>

using namespace std;

void InsertInfoIntoMap(const vector<string>& infoList, map<string, vector<int>>& infoMap)
{
    int score = 0;
    istringstream iss;
    string infoString[4][2] = {
        {"-"},
        {"-"},
        {"-"},
        {"-"}
    };
    string keyString = "";
    
    for (string info : infoList)
    {
        // info 문자열 파싱
        iss.clear();
        iss.str(info);
        iss >> infoString[0][1] >> infoString[1][1] >> infoString[2][1] >> infoString[3][1] >> score;

        // 모든 경우의 info를 만들어 map에 삽입
        for(int i = 0; i < 2; i++)
        {
            for(int j = 0; j < 2; j++)
            {
                for(int k = 0; k < 2; k++)
                {
                    for(int l = 0; l < 2; l++)
                    {
                        keyString = infoString[0][i] + infoString[1][j] + infoString[2][k] + infoString[3][l];
                        vector<int> scores;
                        infoMap.insert(make_pair(keyString, scores));
                        infoMap[keyString].push_back(score);
                    }
                }
            }
        }
    }
}

int GetResultFromInfoMap(string query, const map<string, vector<int>>& infoMap)
{
    istringstream iss(query);
    string language = "";
    string jobGroup = "";
    string career = "";
    string soulFood = "";
    int score = 0;
    string temp = "";
    string keyString = "";
    int answer = 0;
    
    // query를 파싱하여 keyString 만들기
    iss >> language >> temp >> jobGroup >> temp >> career >> temp >> soulFood >> score;
    keyString = language + jobGroup + career + soulFood;
    
    // keyString에 해당하는 scores가 없으면 0 리턴
    auto mapIter = infoMap.find(keyString);
    if (infoMap.end() == mapIter)
    {
        answer = 0;
    }
    else
    {
        const vector<int>& scores = mapIter->second;
        // score 이상 받은 지원자의 수
        answer = scores.end() - lower_bound(scores.begin(), scores.end(), score);
    }
    
    return answer;
}

vector<int> solution(vector<string> infoList, vector<string> queries)
{
    vector<int> answer;
    map<string, vector<int>> infoMap;
    
    // info로부터 만들 수 있는 모든 경우의 infoString을
    // map{infoString: vec{score1, score2, score3, ...}} 에다가 넣어준다.
    // e.g.
    // info: java backend junior pizza 150 이라면, 총 16(2^4)개의 key가 map에 추가된다.
    // javabackendjuniorpizza: {150}
    // javabackendjunior-: {150}
    // ... 중략 ...
    // ----: {150}
    InsertInfoIntoMap(infoList, infoMap);
    
    // infoMap의 value인 점수 벡터를 오름차순 정렬
    // 정렬을 해야 std::lower_bound 함수 사용 가능.
    for (auto& item : infoMap)
    {
        sort(item.second.begin(), item.second.end());
        
    }
    
    // query에 해당하는 답을 구하기
    for (auto& query : queries)
    {
        answer.push_back(GetResultFromInfoMap(query, infoMap));
    }
    
    return answer;
}
```

