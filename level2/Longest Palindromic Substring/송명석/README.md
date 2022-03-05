> 날짜: 2022-03-05\
> 작성자: 송명석

## 문제 내용

https://leetcode.com/problems/longest-palindromic-substring/



## 문제의 핵심 및 중요 포인트

방법 1:
- 모든 substring을 구한 후, 구한 substring들이 palindromic string인지 검사
- 모든 substring을 구하는데에는 $O(N^2)$, 그리고 palindromic string 검사에는 O(N)이므로 시간복잡도는 **$O(N^3)$**
- 이렇게 푸니 시간 초과.

방법 2:
- palindromic string의 종류는 총 2가지가 있다. 하나는 홀수인 경우(e.g. a b a), 다른 하나는 짝수인 경우(e.g. a b b a)이다.
- input string의 index 0부터 차례로 홀수/짝수인 경우의 palindromic string 검사를 수행한 후, palindromic string이라면 기존의 palindromic string과 길이를 비교하여 더 큰 길이의 palindromic string을 저장하도록 함.
- 시간복잡도는, input string에 대한 for문 O(N), 그리고 해당 index에서 홀수/짝수의 palindromic string 검사를 해야 하므로 O(N) 이므로 **$O(N^2)$**
- 추가로 아래와 같은 **최적화 방법**이 있다. 이를 적용하면 런타임이 약 1/4 정도 빨라진다.
  - a a a x a a a b b c 가 있다고 하면, a a a b b c 부분은 탐색하지 않아도 된다. 왜냐하면 앞의 탐색에서 이미 길이가 7인 palindromic string이 발견되었기 때문이다.

방법 3:
- **Manacher's algorithm** 적용. 시간복잡도는 무려 **amortized O(N)**이다. 얼핏 보면 O(N^2)이지만 두 번째 반복문은 탐색이 진행될수록 도는 경우가 거의 없어지므로 amortized O(N)이 된다.
- 알고리즘 설명: https://www.youtube.com/watch?v=gLUJZ9NSZ5M
- 이 방법은 방법 2보다 런타임이 약 2~3배 정도 빠르다.

## 개선해야 할 점

복습을 해야 한다. 1~2달 전에 푼 문제들 다시 풀라고 하면 못 풀 것 같다.



## 소스 코드

### 방법 2
```c++
class Solution
{
public:
    string longestPalindrome(string s)
    {
        int startIdx = 0;
        int endIdx = 0;
        int maxLen = 0;
        string answer = "";
        
        for (int i = 0; i < s.length(); i++)
        {
            // 최적화
            if (s.length() == i + (maxLen/2))
            {
                break;
            }
            
            // case 1: 홀수
            startIdx = i;
            endIdx = i;
            
            while(startIdx >= 0 & endIdx < s.length())
            {
                if (s[startIdx] != s[endIdx])
                {
                    break;
                }
                else
                {
                    if (endIdx - startIdx + 1 > maxLen)
                    {
                        maxLen = endIdx - startIdx + 1;
                        answer = s.substr(startIdx, maxLen);
                    }
                    startIdx--;
                    endIdx++;
                }
            }
            
            // case 2: 짝수
            startIdx = i;
            endIdx = i + 1;
            
            while(startIdx >= 0 & endIdx < s.length())
            {
                if (s[startIdx] != s[endIdx])
                {
                    break;
                }
                else
                {
                    if (endIdx - startIdx + 1 > maxLen)
                    {
                        maxLen = endIdx - startIdx + 1;
                        answer = s.substr(startIdx, maxLen);
                    }
                    startIdx--;
                    endIdx++;
                }
            }
        }
        
        return answer;
    }
};
```

### 방법 3
```c++
class Solution
{
public:
    string longestPalindrome(string s)
    {
        string answer = "";
        string strExtended = "";
        vector<int> pldrmSize;
        int r = 0; // right
        int c = 0; // center
        int maxLen = 0;
        int centerIdx = 0;
        
        // 문자열 변환
        // x a b b a c ->
        // # x # a # b # b # a # c #
        for (int i = 0; i < s.length(); i++)
        {
            strExtended += "#" + s.substr(i, 1);
        }
        strExtended.append("#");
        
        pldrmSize.assign(strExtended.length(), 0);
        
        for (int i = 1; i < strExtended.length() - 1; i++)
        {
            if (i < r)
            {
                pldrmSize[i] = min(r - i, pldrmSize[2*c - i]);
            }
            
            while (0 <= i - 1 - pldrmSize[i] && strExtended.length() > i + 1 + pldrmSize[i])
            {
                if (strExtended[i - 1 - pldrmSize[i]] == strExtended[i + 1 + pldrmSize[i]])
                {
                    pldrmSize[i]++;
                }
                else
                {
                    break;
                }
            }
            
            // r과 c 업데이트
            if (i + pldrmSize[i] > r)
            {
                r = i + pldrmSize[i];
                c = i;
            }
            // maxLen과 centerIdx 업데이트
            if (pldrmSize[i] > maxLen)
            {
                maxLen = pldrmSize[i];
                centerIdx = i;
            }
        }
        
        answer = s.substr((centerIdx - maxLen) / 2, maxLen);
        return answer;
    }
};
```

