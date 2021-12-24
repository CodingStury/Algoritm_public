> 날짜: 2021-12-18
> 작성자: 송명석

## 문제 내용

https://programmers.co.kr/learn/courses/30/lessons/42890


## 문제의 핵심 및 중요 포인트

1. DFS를 통해 순서없는 인덱스 조합을 가져온다.
2. 가져온 인덱스 조합마다 후보키의 여부를 tuple == set.size()로 알아낸다.
3. 후보키 조건을 만족하는 인덱스 조합이 포함된 조합은 비교 리스트에서 제외한다.


## 개선해야 할 점

스터디 리뷰시간에 발견했는데 .append로 컬럼의 모든값을 합쳐서 set에 넣었는데
이러면 (1,01) 과 (10,1)이 동일한 듀플로 인식될 가능성이 있다.
따라서 (" ")를 레코드 사이에 추가해서 구분하도록 해야한다.


## 소스 코드

```java
import java.util.*;
import java.util.stream.Collectors; 
class Solution {
    
    private List<String> keyList = new ArrayList<>();
    private String [][] rel;
    private boolean[] visited;
    private int column;
    private int tuple;
    
    public int solution(String[][] relation) {
        
        int answer = 0;
        column = relation[0].length;
        tuple = relation.length;
        rel = relation;
 
        for(int i=0;i<column;i++){
            visited = new boolean[column];
            dfs("",i+1,0,0);
        }
        
        while(!keyList.isEmpty()){
            String key = keyList.remove(0);

            if(candidateKey(key.split(""))){
                keyList = keyList
                    .stream()
                    .filter(target -> !checkList(target,key))
                    .collect(Collectors.toList()); 
                System.out.println(key+" "+keyList);
                answer++;
            }
        }
        return answer;
    }
    private void dfs(String key,int n, int depth,int index){
        
        if(n == depth){
            keyList.add(key);
            return;
        }
        
        for(int i=index;i<column;i++){
            if(!visited[i]){
                visited[i]=true;
                dfs(key+(String.valueOf(i)),n,depth+1,i+1);
                visited[i]=false;
            }
        }
    }
    
    private boolean candidateKey(String []keys){
        
        HashSet<String> check = new HashSet<>();
        StringBuilder sb = new StringBuilder();
        
        for(int i=0;i<tuple;i++){
            for(String key : keys){
                sb.append(rel[i][Integer.parseInt(key)]);
            }
            check.add(sb.toString());
            sb.setLength(0);
        }
        
        return (check.size() == tuple);
    }
    
    private boolean checkList(String k, String key){
        
        String [] str = key.split("");
        
        for(String s : str){
            if(k.contains(s)){
                continue;
            }
            return false;
        }
        
        return true;
    }
}
```
