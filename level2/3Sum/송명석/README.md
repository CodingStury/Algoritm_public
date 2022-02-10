> ��¥: 2022-02-10\
> �ۼ���: �۸�

## ���� ����

https://leetcode.com/problems/3sum/



## ������ �ٽ� �� �߿� ����Ʈ

DFS�� �� ���� �����ϴ� ��� ����� ���� ���� ��, ������ �ϳ��� ���� for���� ���� ���ϴ� ������� Ǯ���ٰ� �ð� �ʰ�..

�׷��� https://youtu.be/jzZsG8n2R9A �� �����Ͽ� Ǯ����.



## �����ؾ� �� ��

Ǯ�̸� ���÷����� �켱 �ð����⵵ ������ �� �Ŀ� Ǯ��� �� �� ����.

������ Ǯ����� �ð� �ʰ� �߸� �� �㹫�ϴ�.



## �ҽ� �ڵ�

### �ð� �ʰ� �� �ڵ�
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

        // depth�� 2�̸� �湮üũ �ȵ� ���ҵ� Ž��
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
                        // set �ڷᱸ���̹Ƿ� �ߺ� �߰��� ���� ����
                        m_answer.insert(sumTarget);

                        return;
                    }
                }
            }
        }

        // ���� depth�� ��
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

### ������ �ڵ�
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

        // nums�� �������� ����
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

                    // ���� nums = [-2, -2, 0, 0, 0, 0, 2, 2] �̰� l = 2, r = 7 �̸�,
                    // l++�� �ص� �Ȱ��� ���� 0�̹Ƿ� �ߺ��Ǵ� ���� skip�ϵ��� �Ѵ�.
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
