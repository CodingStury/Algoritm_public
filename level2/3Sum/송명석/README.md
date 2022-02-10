> 날짜: 2022-02-10\
> 작성자: 송명석

## 문제 내용

https://leetcode.com/problems/3sum/



## 문제의 핵심 및 중요 포인트

DFS로 두 수를 선택하는 모든 경우의 수를 구한 후, 나머지 하나의 수는 for문을 돌려 구하는 방식으로 풀었다가 시간 초과..

그래서 https://youtu.be/jzZsG8n2R9A 를 참조하여 풀었다.



## 개선해야 할 점

풀이를 떠올렸으면 우선 시간복잡도 생각을 한 후에 풀어야 할 것 같다.

열심히 풀어놓고 시간 초과 뜨면 참 허무하다.



## 소스 코드

### 시간 초과 뜬 코드
```c++
#include <vector>
#include <set>
#include <iostream>
#include <algorithm>

using namespace std;

class Solution
{
private:
    set<vector<int>> m_answer;

public:
    void FindThreeSum(int idx, int depth, vector<int> sumTarget, vector<int>& nums, vector<bool>& visited)
    {
        sumTarget.push_back(nums[idx]);

        depth++;

        // depth가 2이면 방문체크 안된 원소들 탐색
        if (2 == depth)
        {
            for (int i = 0; i < nums.size(); i++)
            {
                if (true == visited[i])
                {
                    continue;
                }
                else
                {
                    if (nums[i] * -1 == sumTarget[0] + sumTarget[1])
                    {
                        sumTarget.push_back(nums[i]);

                        sort(sumTarget.begin(), sumTarget.end());
                        // set 자료구조이므로 중복 추가는 되지 않음
                        m_answer.insert(sumTarget);

                        return;
                    }
                }
            }
        }

        // 다음 depth로 들어감
        for (int i = 0; i < nums.size(); i++)
        {
            if (true == visited[i])
            {
                continue;
            }
            else
            {
                visited[i] = true;
                FindThreeSum(i, depth, sumTarget, nums, visited);
                visited[i] = false;
            }
        }
    }

    vector<vector<int>> threeSum(vector<int>& nums)
    {
        vector<vector<int>> answer;
        vector<bool> visited(nums.size(), false);
        vector<int> sumTarget;

        for (int i = 0; i < nums.size(); i++)
        {
            visited[i] = true;
            FindThreeSum(i, 0, sumTarget, nums, visited);
            visited[i] = false;
        }

        answer.assign(m_answer.begin(), m_answer.end());

        return answer;
    }
};
```
\

### 수정한 코드
```c++
#include <vector>
#include <algorithm>

class Solution
{
public:
    vector<vector<int>> threeSum(vector<int>& nums)
    {
        vector<vector<int>> answer;
        vector<int> temp;
        int a = 0;
        int l = 0;
        int r = 0;
        int threeSum = 0;

        // nums를 오름차순 정렬
        // nums = [-1, 0, 1, 2, -1, -1]
        // -> [-1, -1, -1, 0, 1, 2]
        sort(nums.begin(), nums.end());

        for (int i = 0; i < nums.size(); i++)
        {
            a = nums[i];

            if (i >= 1)
            {
                if (a == nums[i - 1])
                {
                    continue;
                }
            }

            l = i + 1;
            r = nums.size() - 1;
            while (l < r)
            {
                threeSum = a + nums[l] + nums[r];
                if (threeSum > 0)
                {
                    r--;
                }
                else if (threeSum < 0)
                {
                    l++;
                }
                else
                {
                    answer.push_back(vector<int>{a, nums[l], nums[r]});

                    // 만약 nums = [-2, -2, 0, 0, 0, 0, 2, 2] 이고 l = 2, r = 7 이면,
                    // l++를 해도 똑같은 값인 0이므로 중복되는 값은 skip하도록 한다.
                    l++;
                    while (l < r && nums[l - 1] == nums[l])
                    {
                        l++;
                    }
                }
            }
        }

        return answer;
    }
};
```
