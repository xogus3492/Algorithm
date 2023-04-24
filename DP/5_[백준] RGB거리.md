```java
import java.util.*;

public class Main {
    final static int RED = 0;
    final static int GREEN = 1;
    final static int BLUE = 2;

    static int[][] dp;
    static int[][] rgb;

    public static void main(String[] args) {
        Scanner s = new Scanner(System.in);

        int N = s.nextInt();

        dp = new int[N][3];
        rgb = new int[N][3];

        for (int i = 0; i < rgb.length; i++) {
            rgb[i][RED] = s.nextInt();
            rgb[i][GREEN] = s.nextInt();
            rgb[i][BLUE] = s.nextInt();
        }

        dp[0][RED] = rgb[0][RED];
        dp[0][GREEN] = rgb[0][GREEN];
        dp[0][BLUE] = rgb[0][BLUE];

        System.out.println(Math.min(dynamic(N-1, RED), Math.min(dynamic(N-1, GREEN), dynamic(N-1, BLUE))));
    }

    static int dynamic(int n, int color) {

        if (dp[n][color] == 0) {

            if (color == RED) {
                dp[n][RED] = Math.min(dynamic(n-1, GREEN), dynamic( n-1, BLUE)) + rgb[n][color];
            } else if (color == GREEN) {
                dp[n][GREEN] = Math.min(dynamic(n-1, RED), dynamic( n-1, BLUE)) + rgb[n][color];
            } else {
                dp[n][BLUE] = Math.min(dynamic(n-1, RED), dynamic( n-1, GREEN)) + rgb[n][color];
            }

        }

        return dp[n][color];
    }
}

=> 재귀를 사용하는 동적 프로그램 문제의 경우,
	재귀 함수 내부의 <if (dp[n][color] == 0)> 이러한 조건문을 적용시켜 
	배열 범위 벗어남 에러를 방지하기 위해 
	dp[0][RED] = rgb[0][RED];
  dp[0][GREEN] = rgb[0][GREEN];
  dp[0][BLUE] = rgb[0][BLUE];
	이런 식으로 초기화를 시켜놔야 함

	이 문제는 모든 경우의 수의 각 자리에 최소 비용을 누적시키는 방식임
```
