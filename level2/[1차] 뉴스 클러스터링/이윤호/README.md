> 날짜: 2021-12-18
> 작성자: 이윤호

## 문제 내용

`https://programmers.co.kr/learn/courses/30/lessons/17677#`



## 문제의 핵심 및 중요 포인트

`문제를 풀 때 사용한 알고리즘이나 전략, 중요 포인트 등을 기입`



## 개선해야 할 점

더럽게 오래걸림, 정확도가 떨어짐



## 소스 코드

```c++
#include <vector>


#include <string>
#include <vector>
#include <algorithm>

using namespace std;

vector<string> split(string str)
{
	vector<string> t;
	for(int i = 0; i <str.size() - 1; ++i)
	{
		string s = "  ";

		if(str[i] >= 'a' && str[i] <= 'z')
		{
			s[0] = str[i];
			s[0] = s[0] - 'a' + 'A';
		}
		else if (str[i] >= 'A' && str[i] <= 'Z')
		{
			s[0] = str[i];
		}

		if (str[i + 1] >= 'a' && str[i + 1] <= 'z')
		{
			s[1] = str[i + 1];
			s[1] = s[1] - 'a' + 'A';
		}
		else if (str[i + 1] >= 'A' && str[i + 1] <= 'Z')
		{
			s[1] = str[i + 1];
		}

		if(s[0] == ' ' || s[1] == ' ')
		{
			continue;
		}

		t.emplace_back(s);
	}
	return t;
}

int solution(string str1, string str2) {
    int answer = 0;

	

	vector<string> a = split(str1);
	vector<string> b = split(str2);

	if(a.size() == 0 && b.size() == 0)
	{
		return 65536;
	}
	vector<string> va(a);
	vector<string> vb(b);


	sort(va.begin(), va.end());
	sort(vb.begin(), vb.end());
	va.erase(unique(va.begin(), va.end()), va.end());
	vb.erase(unique(vb.begin(), vb.end()), vb.end());

	if(va.size() == vb.size() && va[0] == vb[0] && va.size() == 1)
	{
		double n = min(a.size(), b.size());
		double u = max(a.size(), b.size());
		double r = n / u * 65536;
		return r;
	}

	vector<string> ca(a);
	vector<string> cb(b);
	
	vector<string> vu(a);

	for(int i = 0; i < b.size(); ++i)
	{
		auto f = find(ca.begin(), ca.end(), b[i]);
		if(ca.end() == f)
		{
			vu.push_back(b[i]);
		}
		else
		{
			ca.erase(f, f + 1);
		}
	}

	vector<string> vn;

	for (int i = 0; i < a.size(); ++i)
	{
		auto f = find(cb.begin(), cb.end(), a[i]);
		if (cb.end() != f)
		{
			vn.push_back(a[i]);
			cb.erase(f, f + 1);
		}
	}


	double n = vn.size();
	double u = vu.size();
	double r = n / u * 65536;
	return r;
}

int main()
{
	solution("aa aa bb bb", "AAAA BBBB");

	return 0;
}
```

