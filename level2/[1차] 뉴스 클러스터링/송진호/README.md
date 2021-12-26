> 날짜: 2021-12-18
> 작성자: 송명석

## 문제 내용

https://programmers.co.kr/learn/courses/30/lessons/17677


## 문제의 핵심 및 중요 포인트
1. 순서대로 2글자씩 묶어낸 조합을 모두 찾는다.
2. 주어진 조건에 맞게 필터링한다.
3. 유사도를 계산한다.


## 개선해야 할 점

1. 직접 구현한 뒤 자바 **Collectors**를 찾아보니 동일한 기능을 수행하는 메서드들이 많았다.
해당 부분에 대한 학습의 필요성을 느꼈다.

## 소스 코드

```java
import java.util.*;
import java.util.stream.Collectors;

class Solution {
    public int solution(String str1, String str2) {
        int answer = 0;
        // 전체 조합을 만들고
        // 대소문자는 통일 시키고 
        // 특수문자 포함된건 지우고
        // 원소는 각각으로 처리됨 -> {1,1} {1,1,1} -> 2 / 3
        // 유사도 비교한다. (교집합/합집합)
      
        List<String> list1 = this.makeSetToList(str1);
        List<String> list2 = this.makeSetToList(str2);

        
        
        HashMap<String, Long> map1 = list1
            .stream()
            .collect(Collectors.groupingBy(arg -> arg, HashMap::new, Collectors.counting()));
        HashMap<String, Long> map2 = list2
            .stream()
            .collect(Collectors.groupingBy(arg -> arg, HashMap::new, Collectors.counting()));
        
       
        
        HashSet<String> keySet = new HashSet<>();
        keySet.addAll(list1);
        keySet.addAll(list2);
        
        int union = 0;
        int intersection = 0;
        
        for(String key : keySet){
            if(map1.containsKey(key) && map2.containsKey(key)){
                // 둘다 키가 있다 ->  공통
                union += Math.max(map1.get(key), map2.get(key));
                intersection += Math.min(map1.get(key), map2.get(key));
            } else {
                union += map1.getOrDefault(key,(long)0);
                union += map2.getOrDefault(key,(long)0);
            }
        }

        return union != 0 ? (intersection*65536/union) : 65536;
    }
    private List<String> makeSetToList(String str){
        String []words = str.split("");
        List<String> list = new LinkedList<>();
        int start = 0;
        int end = words.length;
        for(int i=start; i<end-1;i++){
            list.add(words[i]+words[i+1]);
        }
        return this.validation(list);
    }
    private List<String> validation(List<String> list){
        return list.stream()
            .map(s -> s.toLowerCase())
            .filter(s -> s.matches("[a-z]{2}"))
            .collect(Collectors.toList());
    }
    
}
```