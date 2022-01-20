> ��¥: 2021-01-19
>
> �ۼ���: �۸�

## ���� ����

https://programmers.co.kr/learn/courses/30/lessons/92335

k�������� �Ҽ� ���� ���ϱ�



## ������ �ٽ� �� �߿� ����Ʈ

�Ʒ��� �� �������� Ǯ�� ���� Ǯ���� �����̴�.

- n�� k������ ��ȯ�Ѵ�.
- ��ȯ�� ���� ���ڿ��� ���� 0���� split�Ѵ�.
- split�� ���ڵ� �߿��� �Ҽ��� ������ ���Ѵ�.

�߰��� n�� �ִ��� 100���̹Ƿ� �̸� 3������ ��ȯ�� ������ ���� ������ int�� ������ �Ѿ��. �̸� �� ó�����־�� �Ѵ�.



## �����ؾ� �� ��

�� Ǯ�� ���ܸ��� ���� �ð��� ���� �ɷȴ�. �̷� �������� ���� ������ ���� Ǯ��� ���� �߿��� �� ����. ���� Ǯ� ���۷��� ��뿡 �ͼ�������, ������ �ͼ������߰ڴ�.



## �ҽ� �ڵ�

```c++
#include <string>
#include <vector>
#include <cmath>
#include <iostream>
#include <stdint.h>

using namespace std;

string ConvertDecimalToBaseK(int num, int k)
{
    int share = 0;
    int remainder = 0;
    int i = 0;
    string stringConverted = "";
    
    while(num >= k)
    {
        share = num / k;
        remainder = num % k;
        
        stringConverted.insert(0, to_string(remainder));
        num = share;
    }
    stringConverted.insert(0, to_string(share));
    
    return stringConverted;
}

void split(string str, string delim, vector<string>& splitted)
{
    int start = 0;
    int end = 0;
    string subString;
    
    end = str.find(delim, start);
    while (string::npos != end)
    {
        subString = str.substr(start, end - start);
        // delim�� ������ �̾ ������ ��쿡�� subString�� ""�� �ǹǷ�, �̸� ó���ؾ� ��.
        // e.g. 10000234
        if (0 < subString.length())
        {
            splitted.push_back(subString);
        }
        start = end + delim.length();
        end = str.find(delim, start);
    }
    subString = str.substr(start, end);  // end�� npos�� �� ����.
    // ������ ���ڰ� delim�� ��쿡�� subString�� ""�� �ǹǷ�, �̸� ó���ؾ� ��.
    if (0 < subString.length())
    {
        splitted.push_back(subString);
    }
}

bool IsPrimeNumber(int64_t num)
{
    if (1 >= num)
    {
        return false;
    }
    
    for (int64_t i = 2; i <= sqrt(num); i++)
    {
        if (0 == num % i)
        {
            return false;
        }
    }
    
    return true;
}

int solution(int n, int k)
{
    int answer = 0;
    string numString = "";
    vector<string> numList;
    
    // ���� ���� n�� k���� numString�� ��ȯ�Ѵ�.
    numString = ConvertDecimalToBaseK(n, k);
    
    // ��ȯ�� ���� ���ڿ��� 0�� delimeter�� split�Ѵ�.
    split(numString, "0", numList);
    
    // split�� ���ڵ� �߿��� �Ҽ��� ������ ���Ѵ�.
    for (string s : numList)
    {
        int64_t target = stoll(s);
        
        if (1 == target)
        {
            continue;
        }
        
        if (true == IsPrimeNumber(target))
        {
            answer++;
        }
    }
    
    return answer;
}
```

