> ��¥: 2021-12-18
> 
> �ۼ���: ����ȣ

## ���� ����

https://leetcode.com/problems/product-of-array-except-self/



## ������ �ٽ� �� �߿� ����Ʈ

O(n����)�� ������� �ʰ� ������ �ذ��� �� �ִ°� 



## �����ؾ� �� ��

��ȸ Ƚ���� �������� ���� �����ϰ� �����ؾ���



## �ҽ� �ڵ�

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

