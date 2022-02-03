> 날짜: 2021-12-18
> 
> 작성자: 이윤호

## 문제 내용

https://leetcode.com/problems/product-of-array-except-self/



## 문제의 핵심 및 중요 포인트

O(n제곱)을 사용하지 않고 문제를 해결할 수 있는가 



## 개선해야 할 점

순회 횟수에 집착하지 말고 유연하게 생각해야함



## 소스 코드

```c++
class Solution {
public:
    vector<int> productExceptSelf(vector<int>& nums) {
        vector<int> answer(nums.size(), 1);
        

        int p = 1;
        for (int i = 1; i < nums.size(); ++i)
        {
            p = p * nums[i - 1];
            answer[i] = p;
        }

        p = 1;

        for(int i = nums.size() - 2; i >= 0; --i)
        {
            p = p * nums[i + 1];
            answer[i] = p * answer[i];
        }


        return  answer;
    }
};
```

