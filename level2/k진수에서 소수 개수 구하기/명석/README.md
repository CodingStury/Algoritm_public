> 날짜: 2021-01-19
>
> 작성자: 송명석

## 문제 내용

https://programmers.co.kr/learn/courses/30/lessons/92335

k진수에서 소수 개수 구하기



## 문제의 핵심 및 중요 포인트

아래의 세 문제들을 풀면 쉽게 풀리는 문제이다.

- n을 k진수로 변환한다.
- 변환한 숫자 문자열을 문자 0으로 split한다.
- split된 숫자들 중에서 소수의 개수를 구한다.

추가로 n의 최댓값은 100만이므로 이를 3진수로 변환할 때에는 숫자 범위가 int형 범위를 넘어간다. 이를 잘 처리해주어야 한다.



## 개선해야 할 점

각 풀이 스텝마다 구현 시간이 오래 걸렸다. 이런 문제점은 코테 문제를 많이 풀어보는 것이 중요할 것 같다. 많이 풀어서 레퍼런스 사용에 익숙해지고, 유형에 익숙해져야겠다.



## 소스 코드

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
        // delim이 여러번 이어서 나오는 경우에는 subString이 ""가 되므로, 이를 처리해야 함.
        // e.g. 10000234
        if (0 < subString.length())
        {
            splitted.push_back(subString);
        }
        start = end + delim.length();
        end = str.find(delim, start);
    }
    subString = str.substr(start, end);  // end는 npos가 될 것임.
    // 마지막 문자가 delim인 경우에는 subString이 ""가 되므로, 이를 처리해야 함.
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
    
    // 양의 정수 n을 k진수 numString로 변환한다.
    numString = ConvertDecimalToBaseK(n, k);
    
    // 변환한 숫자 문자열을 0을 delimeter로 split한다.
    split(numString, "0", numList);
    
    // split한 숫자들 중에서 소수의 개수를 구한다.
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

