> ��¥: 2021-01-07
>
> �ۼ���: �۸�

## ���� ����

https://programmers.co.kr/learn/courses/30/lessons/12982

����



## ������ �ٽ� �� �߿� ����Ʈ

������ �ٽ�



## �����ؾ� �� ��

������ �밭 �о ������ �ع��ȴ�. ������ ������ ���� ���� ��߸� �ϴ� �� �˰� DFS Ž�� ���� ��....

������ �� �Ĳ��� �о�߰ڴ�.


## �ҽ� �ڵ�

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
    
    // d�� �������� �����Ѵ�.
    sort(d.begin(), d.end());
    
    // budget���� ���� ������ �ִ� �μ� ���� ���Ѵ�.
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

