> 날짜: 2021-12-18
> 
> 작성자: 송명석

## 문제 내용

https://leetcode.com/problems/product-of-array-except-self/



## 문제의 핵심 및 중요 포인트

Input 을 [a, b, c ...] 처럼 두고 answer의 규칙을 찾아내는 것이 핵심이다.



## 개선해야 할 점

비슷한 유형의 문제들을 풀어보면서, 풀이 시간을 단축해야 함.



## 소스 코드

```c++
class Solution
{
public:
    vector<int> productExceptSelf(vector<int>& nums)
    {
        // 나누기 사용하지 않고 O(n) complexity로 풀어야 함.
        // e.g.
        // [3, 2, 5, 4] => [40, 50, 24, 30]
        
        // [a, b, c, d] => [bcd, acd, abd, abc]
    
        // 기준 인덱스의 좌 우로 나눈다.
        // a => left: none, right: bcd
        // b => left: a,    right: cd
        // c => left: ab,   right: d
        // d => left: abc,  right: none
        
        // 즉 answer[i] = left[i] * right[i]
        
        // nums의 원소를 순방향으로 곱하여, left에 해당하는 값 리스트를 만든다.
        // nums의 원소를 역방향으로 곱하여, right에 해당하는 값 리스트를 만든다.
        // 두 리스트의 원소들을 서로 곱한 것이 답.
        
        vector<int> leftList(nums.size(), 0);
        vector<int> rightList(nums.size(), 0);
        vector<int> answer(nums.size(), 0);
        
        leftList[0] = 1;
        rightList[nums.size() - 1] = 1;
        
        for (int i = 1; i < nums.size(); i++)
        {
            leftList[i] = leftList[i - 1] * nums[i - 1];
        }
        
        for (int i = nums.size() - 1; i > 0; i--)
        {
            rightList[i - 1] = rightList[i] * nums[i];
        }
        
        for (int i = 0; i < nums.size(); i++)
        {
            answer[i] = leftList[i] * rightList[i];
        }
        
        return answer;
    }
};
```

