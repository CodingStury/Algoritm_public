> 날짜: 2021-01-02
>
> 작성자: 송명석

## 문제 내용

https://programmers.co.kr/learn/courses/30/lessons/17677

[1차] 뉴스 클러스터링 - 2018 KAKAO BLIND RECRUITMENT



## 문제의 핵심 및 중요 포인트

1. str1과 str2의 다중집합을 구한다. 
   - e.g. aaAaB -> {"aa":3, "ab":1}
   - **정규표현식**으로 영문자만 있는지 검증
2. 두 집합의 다중합집합, 다중교집합의 원소 개수를 구한다.
   - 다중합집합은 두 집합을 합치되, 중복되는 키는 두 키 중에서 더 큰 값을 가지는 것을 채택한다. 두 집합이 합쳐지면, 키의 값만을 모두 더한다.
   - 다중교집합은 중복되는 키만을 찾아서, 두 키 중에서 더 작은 값을 가지는 것을 채택, 그리고 더한다.
3. 자카드 유사도를 구하고 그 값에서 65536을 곱한다.
   - 자카드 유사도는 double형으로 선언해야 하는 것에 주의한다.



## 개선해야 할 점

이번에도 풀이 설계를 좀 느슨하게 해서 코딩하면서 시간을 많이 잡아먹었다. 2시간 정도... 문제풀이 1시간, 함수 사용법 검색 30분, 디버깅 30분 ㅠㅠ

그리고 C++ STL 함수의 사용법을 제대로 알지 못해 검색하는데에도 시간을 많이 잡아먹었다.. **자주 쓰이는 함수들은 사용법을 숙지**하도록 하자. **복습 필수**

**문제 풀기 전 주석으로 문제 풀이 프로세스를 명시하고, 주의할 점을 적어두는 연습**을 하자.



## 소스 코드

```c++
#include <string>
#include <iostream>
#include <map>
#include <regex>
#include <algorithm>

using namespace std;

// map<string, int> 자료구조 형태의 다중집합 구하기
map<string, int> GetMultipleSet(string strString)
{
    map<string, int> mapMultipleSet;  // 다중집합
    regex re(R"([A-Za-z]{2})");
    
    if (0 == strString.length())
    {
        return map<string, int>();
    }
    
    // strString을 소문자로 변환
    transform(strString.begin(), strString.end(), strString.begin(), ::tolower);
    
    for (int i = 0; i < strString.length() - 1; i++)
    {
        // substring 구한 후, 다중집합에 없으면 추가.
        string strSubString = strString.substr(i, 2);
        
        if (mapMultipleSet.end() == mapMultipleSet.find(strSubString))
        {
            // substring이 영문자일 때에만 다중집합에 소문자 형태로 추가.
            if (true == regex_match(strSubString, re))
            {
                mapMultipleSet.insert(make_pair(strSubString, 1));
            }
            else
            {
                // Do nothing...
            }
        }
        // 있으면 count++
        else
        {
            mapMultipleSet[strSubString]++;
        }
    }
    
    return mapMultipleSet;
}

int GetNumElemMultiUnion(map<string, int> mapA, map<string, int> mapB)
{
    map<string, int>::iterator iterA;
    map<string, int>::iterator iterB;
    int nElemMultiUnion = 0;
    
    // mapB의 원소를 mapA에 추가한다.
    // 키의 중복이 발생하면 둘 중 더 큰 값을 추가한다.
    for (iterB = mapB.begin(); mapB.end() != iterB; iterB++)
    {
        map<string, int>::iterator iterElem = mapA.find(iterB->first);
        if (mapA.end() == iterElem)
        {
            mapA.insert(make_pair(iterB->first, iterB->second));
        }
        else
        {
            if (iterElem->second < iterB->second)
            {
                mapA[iterElem->first] = iterB->second;
            }
            else
            {
                // Do nothing...
            }
        }
    }
    
    for (iterA = mapA.begin(); mapA.end() != iterA; iterA++)
    {
        nElemMultiUnion += iterA->second;
    }
    
    return nElemMultiUnion;
}

int GetNumElemMultiIntersection(map<string, int> mapA, map<string, int> mapB)
{
    map<string, int>::iterator iterA;
    map<string, int>::iterator iterB;
    int nElemMultiIntersection = 0;
    
    // mapB와 mapA에 같은 키가 존재하면, 둘 중 작은 값을 채택하여 더한다.
    for (iterB = mapB.begin(); mapB.end() != iterB; iterB++)
    {
        map<string, int>::iterator iterElem = mapA.find(iterB->first);
        if (mapA.end() == iterElem)
        {
            continue;
        }
        else
        {
            if (iterElem->second < iterB->second)
            {
                nElemMultiIntersection += iterElem->second;
            }
            else
            {
                nElemMultiIntersection += iterB->second;
            }
        }
    }
    
    return nElemMultiIntersection;
}

int solution(string str1, string str2)
{
    int answer = 0;
    map<string, int> mapStr1;
    map<string, int> mapStr2;
    int nUnion = 0;             // 다중합집합의 원소 개수
    int nIntersection = 0;      // 다중교집합의 원소 개수
    double dJaccardSimilarity = 0.0;
    
    // str1과 str2의 다중집합을 구한다.   
    mapStr1 = GetMultipleSet(str1);
    mapStr2 = GetMultipleSet(str2);
    
    // 다중합집합, 다중교집합의 원소 개수를 구한다.
    nUnion = GetNumElemMultiUnion(mapStr1, mapStr2);
    nIntersection = GetNumElemMultiIntersection(mapStr1, mapStr2);
    
    // 자카드 유사도 * 65536을 계산한다.
    if (0 == nUnion)
    {
        dJaccardSimilarity = 1.0;
    }
    else
    {
        dJaccardSimilarity = static_cast<double>(nIntersection) / nUnion;
    }
    cout << dJaccardSimilarity << endl;
    answer = dJaccardSimilarity * 65536;
    
    return answer;
}
```
