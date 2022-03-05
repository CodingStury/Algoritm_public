> ��¥: 2022-03-05\
> �ۼ���: �۸�

## ���� ����

https://leetcode.com/problems/longest-palindromic-substring/



## ������ �ٽ� �� �߿� ����Ʈ

��� 1:
- ��� substring�� ���� ��, ���� substring���� palindromic string���� �˻�
- ��� substring�� ���ϴµ����� $O(N^2)$, �׸��� palindromic string �˻翡�� O(N)�̹Ƿ� �ð����⵵�� **$O(N^3)$**
- �̷��� Ǫ�� �ð� �ʰ�.

��� 2:
- palindromic string�� ������ �� 2������ �ִ�. �ϳ��� Ȧ���� ���(e.g. a b a), �ٸ� �ϳ��� ¦���� ���(e.g. a b b a)�̴�.
- input string�� index 0���� ���ʷ� Ȧ��/¦���� ����� palindromic string �˻縦 ������ ��, palindromic string�̶�� ������ palindromic string�� ���̸� ���Ͽ� �� ū ������ palindromic string�� �����ϵ��� ��.
- �ð����⵵��, input string�� ���� for�� O(N), �׸��� �ش� index���� Ȧ��/¦���� palindromic string �˻縦 �ؾ� �ϹǷ� O(N) �̹Ƿ� **$O(N^2)$**
- �߰��� �Ʒ��� ���� **����ȭ ���**�� �ִ�. �̸� �����ϸ� ��Ÿ���� �� 1/4 ���� ��������.
  - a a a x a a a b b c �� �ִٰ� �ϸ�, a a a b b c �κ��� Ž������ �ʾƵ� �ȴ�. �ֳ��ϸ� ���� Ž������ �̹� ���̰� 7�� palindromic string�� �߰ߵǾ��� �����̴�.

��� 3:
- **Manacher's algorithm** ����. �ð����⵵�� ���� **amortized O(N)**�̴�. ���� ���� O(N^2)������ �� ��° �ݺ����� Ž���� ����ɼ��� ���� ��찡 ���� �������Ƿ� amortized O(N)�� �ȴ�.
- �˰��� ����: https://www.youtube.com/watch?v=gLUJZ9NSZ5M
- �� ����� ��� 2���� ��Ÿ���� �� 2~3�� ���� ������.

## �����ؾ� �� ��

������ �ؾ� �Ѵ�. 1~2�� ���� Ǭ ������ �ٽ� Ǯ��� �ϸ� �� Ǯ �� ����.



## �ҽ� �ڵ�

### ��� 2
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
            // ����ȭ
            if (s.length() == i + (maxLen/2))
            {
                break;
            }
            
            // case 1: Ȧ��
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
            
            // case 2: ¦��
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

### ��� 3
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
        
        // ���ڿ� ��ȯ
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
            
            // r�� c ������Ʈ
            if (i + pldrmSize[i] > r)
            {
                r = i + pldrmSize[i];
                c = i;
            }
            // maxLen�� centerIdx ������Ʈ
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

