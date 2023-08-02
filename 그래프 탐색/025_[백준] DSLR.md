```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.*;

public class Main {

    static boolean visited[];

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st;

        int T = Integer.parseInt(br.readLine());
        while (T-- > 0) {
            st = new StringTokenizer(br.readLine());
            int A = Integer.parseInt(st.nextToken());
            int B = Integer.parseInt(st.nextToken());

            visited = new boolean[10000];
            System.out.println(bfs(A, B));
        }
    }

    static String bfs(int A, int B) {
        Queue<Integer> queue = new LinkedList<>();
        Queue<String> commands = new LinkedList<>();
        queue.add(A);
        commands.add("");
        visited[A] = true;

        while (!queue.isEmpty()) {
            int a = queue.poll();
            String command = commands.poll();

            if (a == B) {
                return command;
            }

            int d = D(a);
            if (!visited[d]) {
                queue.add(d);
                commands.add(command + "D");
                visited[d] = true;
            }

            int s = S(a);
            if (!visited[s]) {
                queue.add(s);
                commands.add(command + "S");
                visited[s] = true;
            }

            int l = L(a);
            if (!visited[l]) {
                queue.add(l);
                commands.add(command + "L");
                visited[l] = true;
            }

            int r = R(a);
            if (!visited[r]) {
                queue.add(r);
                commands.add(command + "R");
                visited[r] = true;
            }

        }

        return "";
    }

    static int D(int a) {
        return (a * 2) % 10000;
    }

    static int S(int a) {
        return a == 0 ? 9999 : a - 1;
    }

    static int L(int a) {
        return (a / 1000) + ((a % 1000) * 10);
    }

    static int R(int a) {
        return ((a % 10) * 1000) + (a / 10);
    }
}

=> 복붙할 때 바꿔줘야하는 변수 꼭 바꿔 실수하지 않기!
```
