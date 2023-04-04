```java
import java.util.*;

public class Main {

    static int[] temp;
    public static void main(String[] args) {
        Scanner s = new Scanner(System.in);

        int n = s.nextInt();

        int[] arr = new int[n];
        for (int i = 0; i < n; i++) {
            arr[i] = s.nextInt();
        }

        temp = new int[arr.length];
        mergeSort(arr, 0, arr.length - 1);

        int r = 0;
        for (int i = 0; i < arr.length; i++) {
            int sum = 0;
            for (int j = i; j >= 0; j--) {
                sum += arr[j];
            }
            r += sum;
        }

        System.out.println(r);
    }

    public static void mergeSort(int[] arr, int left, int right) {
        if (left == right) return;

        int mid = (left + right) / 2;

        mergeSort(arr, left, mid);
        mergeSort(arr, mid + 1, right);

        merge(arr, left, mid, right);
    }

    public static void merge(int[] arr, int left, int mid, int right) {
        int l = left;
        int r = mid + 1;
        int idx = l;

        while (l <= mid && r <= right) {
            if (arr[l] <= arr[r]) {
                temp[idx++] = arr[l++];
            } else {
                temp[idx++] = arr[r++];
            }
        }

        if (l > mid) {
            while (r <= right) {
                temp[idx++] = arr[r++];
            }
        } else {
            while (l <= mid) {
                temp[idx++] = arr[l++];
            }
        }

        for (int i = left; i <= right; i++) {
            arr[i] = temp[i];
        }
    }
}

=> 병합 정렬 사용
		merge 함수에서 마지막에 임시 배열을 처음 배열에 저장할 때, 처음 배열이 static으로 선언 된
		임시 배열의 주소 값을 참조하는 것임
```
