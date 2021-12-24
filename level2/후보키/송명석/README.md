> 날짜: 2021-12-24
> 작성자: 송명석

## 문제 내용

https://programmers.co.kr/learn/courses/30/lessons/42890

후보키 - 2019 카카오 블라인드 채용 



## 문제의 핵심 및 중요 포인트

3시간동안 아래 로직처럼 짜보았는데 의외로 예외 케이스에서 많이 실패가 떴다..

1. DFS로 모든 키의 조합 탐색
2. 유일성 만족하는지 확인 (2중 set 자료구조 활용)
   최소성 만족하는지 확인 (std::set_intersection, 즉 교집합 활용)
3. 2의 검증을 통과했으면 후보키 리스트에 추가
4. DFS 탐색이 끝나면 후보키 리스트 크기가 답이 된다.

## 개선해야 할 점

문제 풀기 전에 어떻게 풀지 노트에 대략 써놓은 후에 코딩을 시작하는 편이다.

하지만 **디테일하게 쓰지 않아서** 막상 코딩하려보면 어떤 알고리즘을 써야 하는지 고민하는 시간이 많고, 한 파트를 완성한다 해도 제대로 짜놓지 않아 오류가 많이나서 오류를 잡는데 시간을 너무 사용했다.

자세하게 문제 풀이 방법을 계획하고 코딩하는 습관을 길러야 겠다.



## 소스 코드

아래는 3시간동안 짰는데 실패한 코드이다.

```c++
#include <string>
#include <vector>
#include <set>
#include <algorithm>

using namespace std;

set<set<int>> g_setVisited;
int g_nNumCandidateKey = 0;
vector<vector<int>> g_vecCandidateKey;

bool CheckVisit(const set<int>& setVisited)
{
    // 방문한 적이 있으면 true 리턴
    if (g_setVisited.end() != g_setVisited.find(setVisited))
    {
        return true;
    }
    else
    {
        g_setVisited.insert(setVisited);
        return false; 
    }
}

bool CheckUniqueness(const vector<int>& vecKeyIdx, const vector<vector<string>>& relation)
{
    set<set<string>> setHash;
    set<string> setString;
    
    cout << endl;
    
    for (int nRow = 0; nRow < relation.size(); nRow++)
    {
        setString.clear();
        for(int i = 0; i < vecKeyIdx.size(); i++)
        {
            setString.insert(relation[nRow][vecKeyIdx[i]]);
        }
        
        if (setHash.end() != setHash.find(setString))
        {
            return false;
        }
        else
        {
            setHash.insert(setString);
        }
    }
    
    return true;
}

bool CheckMinimality(vector<int> vecTarget)
{
    vector<vector<int>>::iterator vecIter;
    vector<int> vecIntersection;
    for (vecIter = g_vecCandidateKey.begin(); vecIter != g_vecCandidateKey.end(); vecIter++)
    {
        // set_intersection 사용하기 위해서는 정렬 필요
        sort((*vecIter).begin(), (*vecIter).end());
        sort(vecTarget.begin(), vecTarget.end());
        
        set_intersection(
            (*vecIter).begin(), (*vecIter).end(),
            vecTarget.begin(), vecTarget.end(),
            back_inserter(vecIntersection)
        );
        
        if (vecIntersection.size() == (*vecIter).size())
        {
            return false;
        }
    }
    
    return true;
}

void DFS(const vector<vector<string>>& relation, vector<int> vecKeyIdx, set<int> setVisited)
{
    // 방문 여부 확인 및 방문 체크
    if (true == CheckVisit(setVisited))
    {
        return;
    }
    
    // 유일성 및 최소성 검사
    // 유일성 만족하면 후보키 카운트 후 종료
    if (true == CheckUniqueness(vecKeyIdx, relation) &&
        true == CheckMinimality(vecKeyIdx))
    {
        g_vecCandidateKey.push_back(vecKeyIdx);
        g_nNumCandidateKey += 1;
        return;
    }
    // 만족하지 않으면 다음 탐색 시작
    else
    {
        for (int nCol = 0; nCol < relation[0].size(); nCol++)
        {
            // 방문한 적이 없으면 탐색
            if (setVisited.end() == setVisited.find(nCol))
            {
                setVisited.insert(nCol);
                vecKeyIdx.push_back(nCol);
                DFS(relation, vecKeyIdx, setVisited);
                setVisited.erase(nCol);
                vecKeyIdx.pop_back();
            }
            else
            {
                continue;
            }
        }
    }
}

int solution(vector<vector<string>> relation)
{
    int answer = 0;
    set<int> setVisited;
    vector<int> vecKeyIdx;
    
    for (int nColIdx = 0; nColIdx < relation[0].size(); nColIdx++)
    {
        setVisited.insert(nColIdx);
        vecKeyIdx.push_back(nColIdx);
        DFS(relation, vecKeyIdx, setVisited);
        vecKeyIdx.clear();
        setVisited.clear();
    }
    
    answer = g_nNumCandidateKey;
    
    return answer;
}
```



성공한 소스코드는 추후 작성 예정.

```cpp

```

