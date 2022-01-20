> 날짜: 2022-01-20
> 작성자: 이윤호

## 문제 내용

https://programmers.co.kr/learn/courses/30/lessons/92335



## 문제의 핵심 및 중요 포인트

소수를 구할때 제곱근 + 1 범위를 지정해야 한다.



## 개선해야 할 점

`문제를 풀고 난 후, 자신의 개선해야 할 점이 무엇인지 기입`

`e.g. '풀이 시간이 너무 길었다', '정렬 알고리즘 개념에 대해 공부가 필요하다' etc...`



## 소스 코드

```c++
#include <string>
#include <vector>
#include<sstream>
#include <cmath>

using namespace std;


int solution(int n, int k) {
    int answer = 0;

    if(n == 1)
    {
        return answer;
    }

    int tn = n;
    string kstr;

    //k진수로 변환
    int tk = k;
    while(n >= tk)
    {
        tk = tk * k;
    };
    tk /= k;

    while (tk != 1)
    {
    	int count;
        for (count = 0; tn >= tk; ++count)
        {
            tn -= tk;
        }
        kstr += to_string(count);
        tk /= k;
    }
    kstr += to_string(tn);


    //0으로 스플릿
    istringstream ss(kstr);
    string stemp;
    vector<string> pstring;
    while(getline(ss, stemp, '0'))
    {

    	if(stemp != "" && stemp != "1")
    	{
    		pstring.emplace_back(stemp);
    	}
        
    }

    
    //소수 판별
    for(int i = 0 ; i < pstring.size(); ++i)
    {
        bool pfind = true;
       
        unsigned long long int pn = stoull(pstring[i]);
        unsigned long long int sq = static_cast<unsigned long long int>(sqrt(pn) + 1);
        for (unsigned long long int j = 2; j < sq; ++j)
        {

            if (pn % j == 0)
            {
                pfind = false;
                break;
            }
        }

        if(pfind)
        {
            ++answer;
        }
    }

    return answer;
}
```

