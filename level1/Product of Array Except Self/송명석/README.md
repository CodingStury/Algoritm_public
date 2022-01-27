> ��¥: 2021-12-18
> 
> �ۼ���: �۸�

## ���� ����

https://leetcode.com/problems/product-of-array-except-self/



## ������ �ٽ� �� �߿� ����Ʈ

Input �� [a, b, c ...] ó�� �ΰ� answer�� ��Ģ�� ã�Ƴ��� ���� �ٽ��̴�.



## �����ؾ� �� ��

����� ������ �������� Ǯ��鼭, Ǯ�� �ð��� �����ؾ� ��.



## �ҽ� �ڵ�

```c++
class Solution
{
public:
    vector<int> productExceptSelf(vector<int>& nums)
    {
        // ������ ������� �ʰ� O(n) complexity�� Ǯ��� ��.
        // e.g.
        // [3, 2, 5, 4] => [40, 50, 24, 30]
        
        // [a, b, c, d] => [bcd, acd, abd, abc]
    
        // ���� �ε����� �� ��� ������.
        // a => left: none, right: bcd
        // b => left: a,    right: cd
        // c => left: ab,   right: d
        // d => left: abc,  right: none
        
        // �� answer[i] = left[i] * right[i]
        
        // nums�� ���Ҹ� ���������� ���Ͽ�, left�� �ش��ϴ� �� ����Ʈ�� �����.
        // nums�� ���Ҹ� ���������� ���Ͽ�, right�� �ش��ϴ� �� ����Ʈ�� �����.
        // �� ����Ʈ�� ���ҵ��� ���� ���� ���� ��.
        
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

