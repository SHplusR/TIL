그래프 탐색 알고리즘 중 대표적인 두가지 방법은 dfs와 bfs이다.

DFS (Depth -First Search)

: 깊이 우선 탐색이다. 말 그대로 그래프를 일단 한번 들어간 길은 끝까지 파고 나온다. ( 그냥 내 식으로 설명한다 ^\_^)

그리고 스택을 사용한다.

예를들어 아래와 같은 그래프가 있을 때 

[##_Image|kage@cX1QlR/btsl4mHSw8A/wJzrWKSEfFxunvtjBLIkW1/img.png|CDM|1.3|{"originWidth":946,"originHeight":550,"style":"alignCenter","caption":"출처 : 동빈나 유튜브"}_##]

( 내가 직접 그리려 했는데 맥북에 그림판 키고...하기는 좀 글타)

방문기준 : 번호가 낮은 인접 노드, 방문한적이 없는 노드

시작노드 : 1

이라고 한다면, 해당 노드의 방문 경로는

1 2 7 6 8 3 4 5

가 된다.

방문기준에 따라 1->2->7->6 까지 가다가, 6에서 연결된 노드 중 방문하지 않은 노드가 없으므로 스택상에서 6을 꺼내고, 

6을 꺼냈을때 가장 최상위 노드인 7에서 다시 방문기준에 맞는 노드를 찾는다...

이런식으로 모든 노드를 방문했을때 스택에 들어간 순서대로 출력하면 노드 방문경로가 된다.

BFS (Breadth - First Search)

: 넓이 우선 탐색이다. 여러 길을 간다...(내식으로 설명22)

큐를 사용한다.

앞의 예제와 똑같이 생각한다면 

[##_Image|kage@EYMHE/btsl5i6DXvZ/OOT9OyFqeNNrnjWhTXbzI1/img.png|CDM|1.3|{"originWidth":946,"originHeight":550,"style":"alignCenter"}_##]

방문기준 : 번호가 낮은 인접노드, 방문한적이 없는 노드,

시작노드 : 1

이라고 한다면, 해당 노드 방문 경로는

1 2 3 8 7 4 5 6 

이 된다.

우선 큐에 1 을 넣는다. 큐는 선입선출이기 때문에 1을 꺼내면서 방문기준에 따라 2-> 3-> 8을 넣는다. 이후 큐의 선입선출 규칙에 따라 

차례대로 2에 관련된 노드(큐에 7을 넣음), 3에 관련된 노드(큐에 4,5넣음), 8에 관련된 노드(없으니 지나감), 7에 관련된 노드( 큐에 6을 넣음) 를 큐에 넣고 큐에 빼는 방법을 방법하면 큐에서 나온 순서대로 출력하면 노드 방문 경로가 된다.

* 백준 2606
import java.io.*;
import java.util.Arrays;
import java.util.LinkedList;
import java.util.Queue;

public class Main {
    static int n;
    static int line;
    static int[][] arr;
    static boolean[] check;
    static StringBuilder sb;

    static int count;
    static Queue<Integer> queue;

    public static void main(String[] args) throws IOException  {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        n = Integer.valueOf(br.readLine()); //7
        check = new boolean[n+1];
        line = Integer.valueOf(br.readLine()); //6
        arr = new int[line][2];
        sb = new StringBuilder();
        count = 0;
        Arrays.fill(check,false);
        queue = new LinkedList<>();
        for(int i =0; i<line; i++){
            String[] strings = br.readLine().split(" ");
             arr[i][0] = Integer.valueOf(strings[0]);
             arr[i][1] = Integer.valueOf(strings[1]);
        }
        bfs(1);
        System.out.println(sb);
    }
    public static void bfs(int v){
        queue.offer(v);
        check[v] = true;
        while(!queue.isEmpty()){
            int start = queue.poll();
            count++;
            for(int i =0; i<line; i++){
                if((arr[i][1] == start && !check[arr[i][0]]) || (arr[i][0] == start && !check[arr[i][1]])){
                    if(arr[i][1] == start){
                        queue.offer(arr[i][0]);
                        check[arr[i][0]] = true;
                    }
                    else{
                        queue.offer(arr[i][1]);
                        check[arr[i][1]] = true;
                    }
                }
            };
        }
        sb.append(count-1);
    }
}

아래와 같은 코드를 작성했는데, 흠..더 좋은 방법이 있을것같아 찾아보았다.

public static Map<Integer, ArrayList<Integer>> map = new HashMap<>();

난   static int[][] arr; 로 만들어 진행했는데 위와같이 map을 사용하는데 이떄 arraylist를 mapped value로도 사용이 되더라....