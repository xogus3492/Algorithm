```java
import java.util.*;
 
public class Main {
    
    static char[][] board;
    static int[] dx = {0, 1, 0, -1};
    static int[] dy = {1, 0, -1, 0};
    static boolean[][] visited;
    static ArrayList<Node> list;
    static int n = 12, m = 6;
    
    public static void main(String[] args) {
        Scanner scan = new Scanner(System.in);
        
        board = new char[n][m];
        for(int i = 0; i < n; i++) {
            String field = scan.nextLine();
            for(int j = 0; j < m; j++) {
                board[i][j] = field.charAt(j); 
            }
        }
        
        int count = 0;
        //board를 탐색하며 4개 이상 뭉쳐있는 노드를 확인한다.
        while(true) {
            boolean isFinished = true;
            visited = new boolean[n][m];
            for(int i = 0; i < n; i++) {
                for(int j = 0; j < m; j++) {
                    if(board[i][j] != '.') {
                        list = new ArrayList<>();
                        bfs(board[i][j], i, j);
                        
                        if(list.size() >= 4) {
                            isFinished = false; //연쇄가 일어났으므로 더 탐색해보아야 한다.
                            for(int k = 0; k < list.size(); k++) {
                                board[list.get(k).x][list.get(k).y] = '.'; // 터트려서 없앰
                            }
                        }
                    }
                }
            }
            if(isFinished) break;
            fallPuyos(); // 뿌요들을 떨어뜨린다.
            count++;
        }
        System.out.println(count);
    }
    
    public static void fallPuyos() {        
        for (int i = 0; i < m; i++) {
            for (int j = n - 1; j > 0; j--) {
                if (board[j][i] == '.') {
                    for (int k = j - 1; k >= 0; k--) {
                        if (board[k][i] != '.') {
                            board[j][i] = board[k][i];
                            board[k][i] = '.';
                            break;
                        }
                    }
                }
            }
        }
    }
    
    public static void bfs(char c, int x, int y) {
        Queue<Node> q = new LinkedList<>();
        list.add(new Node(x, y));
        q.offer(new Node(x, y));
        visited[x][y] = true;
        
        while(!q.isEmpty()) {
            Node current = q.poll();
            
            for(int i = 0; i < 4; i++) {
                int nx = current.x + dx[i];
                int ny = current.y + dy[i];
                
                if(nx >= 0 && ny >= 0 && nx < n && ny < m && visited[nx][ny] == false && board[nx][ny] == c) {
                    visited[nx][ny] = true;
                    list.add(new Node(nx, ny));
                    q.offer(new Node(nx, ny));
                }
            }
        }
    }
 
    public static class Node {
        int x;
        int y;
        
        public Node(int x, int y) {
            this.x = x;
            this.y = y;
        }
    }
}
```

[틀린 코드]

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.*;

public class Main {
    static int N, M;
    static String map[][];
    static boolean visited[][];
    static List<Puyo> puyos;
    static int moveN[] = {1, -1, 0, 0};
    static int moveM[] = {0, 0, 1, -1};
    static int result;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        N = 12;
        M = 6;
        map = new String[N][M];
        for (int i = 0; i < N; i++) {
            map[i] = br.readLine().split("");
        }

        result = 0;
        boolean flag = true;
        while (flag) {
            flag = false;
            visited = new boolean[N][M];
            for (int i = 0; i < N; i++) {
                for (int j = 0; j < M; j++) {
                    if (!map[i][j].equals(".") && !visited[i][j]) {
                        puyos = new ArrayList<>();
                        visited[i][j] = true;
                        puyos.add(new Puyo(i, j));
                        if (dfs(i, j, 1) >= 4) {
                            flag = true;
                            for (Puyo p : puyos) {
                                map[p.n][p.m] = ".";
                            }
                        }
                    }
                }
            }

            for (int i = 0; i < M; i++) {
                for (int j = N - 1; j > 0; j--) {
                    if (map[j][i].equals(".")) {
                        gravity(j, i);
                    }
                }
            }
            result++;
        }

        System.out.println(result - 1);
    }

    static int dfs(int n, int m, int cnt) {
        for (int i = 0; i < 4; i++) {
            int nextN = n + moveN[i];
            int nextM = m + moveM[i];

            if (nextN < 0 || nextM < 0 || nextN >= N || nextM >= M) continue;
            if (!map[nextN][nextM].equals(map[n][m]) || visited[nextN][nextM]) continue;

            puyos.add(new Puyo(nextN, nextM));
            visited[nextN][nextM] = true;
            cnt += dfs(nextN, nextM, cnt);
        }
        return cnt;
    }

    static void gravity(int n, int m) {
        for (int i = n; i >= 0; i--) {
            if (!map[i][m].equals(".")) {
                map[n][m] = map[i][m];
                map[i][m] = ".";
                return;
            }
        }
    }

    static class Puyo {
        int n;
        int m;

        Puyo(int n, int m) {
            this.n = n;
            this.m = m;
        }
    }
}
```
