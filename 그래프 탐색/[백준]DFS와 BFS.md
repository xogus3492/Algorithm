```java
import java.util.*;

public class Main {
    static int N;
    static int M;
    static int V;
    static boolean[] visited;
    static LinkedList<Integer>[] relationship;

    public static void main(String[] args) {
        Scanner s = new Scanner(System.in);
        N = s.nextInt();
        M = s.nextInt();
        V = s.nextInt();
        relationship = new LinkedList[N + 1];

        for (int i = 1; i <= N; i++) {
            relationship[i] = new LinkedList<>();
        }

        for (int i = 0; i < M; i++) {
            int a = s.nextInt();
            int b = s.nextInt();
            relationship[a].add(b);
            relationship[b].add(a);
        }

        for (int i = 1; i <= N; i++) {
            Collections.sort(relationship[i]);
        }

        //dfs
        visited = new boolean[N + 1];
        dfs(V);
        //dfsStack(V);
        System.out.println();

        //bfs
        visited = new boolean[N + 1];
        bfs(V);
    }

    static void dfs(int V) {
        visited[V] = true;
        System.out.print(V + " ");

        for (int i : relationship[V]) {
            if (!visited[i]) {
                dfs(i);
            }
        }
    } // 재귀함수 방식

    static void dfsStack(int V) {
        Stack<Integer> stack = new Stack<>();
        visited[V] = true;
        stack.push(V);

        while (!stack.isEmpty()) {
            int v = stack.pop();
            System.out.print(v + " ");

            Iterator<Integer> iter = relationship[v].listIterator();
            while (iter.hasNext()) {
                int w = iter.next();
                if (!visited[w]) {
                    visited[w] = true;
                    stack.push(w);
                }
            }
        }

    } // 스택 방식

    static void bfs(int V) {
        Queue<Integer> q = new LinkedList<>();
        visited[V] = true;
        q.add(V); // [V]

        while (!q.isEmpty()) {
            int v = q.poll(); // [] 반환 후 삭제
            System.out.print(v + " ");

            Iterator<Integer> iter = relationship[v].listIterator();
            while (iter.hasNext()) {
                int w = iter.next();
                if (!visited[w]) {
                    visited[w] = true;
                    q.add(w);
                }
            }
        }
    } // Queue 방식
}
```
