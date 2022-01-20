> ��¥: 2021-01-17
>
> �ۼ���: �۸�

## ���� ����

https://programmers.co.kr/learn/courses/30/lessons/72412

���� �˻�



## ������ �ٽ� �� �߿� ����Ʈ

map �ڷᱸ�� Ȱ�� + ������ �̿��� ������ �� ���ϴ� �� �ٽ�.

ȿ���� �׽�Ʈ�� �����Ƿ� �� �������� info �迭�� ��� Ž���ϴ� ����� �߸��� ����̴�.



## �����ؾ� �� ��

������ �Ǽ��� ���.

istringstream Ȱ����� �����س��ƾ� �ڴ�.

lower_bound, upper_bound �Լ� ������ �ܿ�����.



## �ҽ� �ڵ�

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
        // info ���ڿ� �Ľ�
        iss.clear();
        iss.str(info);
        iss >> infoString[0][1] >> infoString[1][1] >> infoString[2][1] >> infoString[3][1] >> score;

        // ��� ����� info�� ����� map�� ����
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
    
    // query�� �Ľ��Ͽ� keyString �����
    iss >> language >> temp >> jobGroup >> temp >> career >> temp >> soulFood >> score;
    keyString = language + jobGroup + career + soulFood;
    
    // keyString�� �ش��ϴ� scores�� ������ 0 ����
    auto mapIter = infoMap.find(keyString);
    if (infoMap.end() == mapIter)
    {
        answer = 0;
    }
    else
    {
        const vector<int>& scores = mapIter->second;
        // score �̻� ���� �������� ��
        answer = scores.end() - lower_bound(scores.begin(), scores.end(), score);
    }
    
    return answer;
}

vector<int> solution(vector<string> infoList, vector<string> queries)
{
    vector<int> answer;
    map<string, vector<int>> infoMap;
    
    // info�κ��� ���� �� �ִ� ��� ����� infoString��
    // map{infoString: vec{score1, score2, score3, ...}} ���ٰ� �־��ش�.
    // e.g.
    // info: java backend junior pizza 150 �̶��, �� 16(2^4)���� key�� map�� �߰��ȴ�.
    // javabackendjuniorpizza: {150}
    // javabackendjunior-: {150}
    // ... �߷� ...
    // ----: {150}
    InsertInfoIntoMap(infoList, infoMap);
    
    // infoMap�� value�� ���� ���͸� �������� ����
    // ������ �ؾ� std::lower_bound �Լ� ��� ����.
    for (auto& item : infoMap)
    {
        sort(item.second.begin(), item.second.end());
        
    }
    
    // query�� �ش��ϴ� ���� ���ϱ�
    for (auto& query : queries)
    {
        answer.push_back(GetResultFromInfoMap(query, infoMap));
    }
    
    return answer;
}
```

