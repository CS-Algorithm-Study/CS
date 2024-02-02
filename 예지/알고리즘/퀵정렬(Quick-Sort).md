# 퀵정렬이란?

퀵정렬(Quick-Sort)은 평균 속도 (NlogN)을 자랑하는 매우 빠른 알고리즘이다.

분할정복(Divide & Conquer) 알고리즘의 하나로 다른 원소와 비교만으로 정렬을 수행하는 비교 정렬에 속하는 알고리즘이다.

## 정렬과정

> 퀵정렬은 임이의 피봇(pivot)을 기준으로 좌측에는 피봇보다 작은 값을 두고 우측에는 피봇보다 큰 값을 두는 방식으로 정렬한다.
> 
![다운로드 (1)](https://github.com/CS-Algorithm-Study/CS/assets/48826098/2d7b6a17-48b1-4c22-9b6e-0acce4cdf77c)
![다운로드](https://github.com/CS-Algorithm-Study/CS/assets/48826098/6862cb48-6433-4a6c-a8a2-7c42bfe8a10a)



1. 피봇을 선택한다.
2. left는 왼쪽에서 오른쪽으로 가면서 피봇보다 더 큰 수를 찾는다.
3. right는 오른쪽에서 왼쪽으로 가면서 피봇보다 더 작은 수를 찾는다.
4. 찾은 지점에서 left와 right를 교환한다.
5. 위 과정을 left랑 right가 바뀔 때까지 반복한다.
6. left랑 right가 바뀌면 피봇과 right를 교환한다.
7. 위 과정을 거치고 나면 피봇의 왼쪽에는 피봇보다 작은 수가, 피봇의 오른쪽에는 피봇보다 큰 수가 위치한다.
8. 피봇을 기준으로 왼쪽과 오른쪽 리스트 두 개로 나눠 과정을 반복 수행한다.
9. 해당 과정을 순환하면 분할된 리스트의 크기가 0이나 1이 되면 수행을 종료한다.

<br>

## 퀵정렬 장단점

### 장점

- 속도가 빠르다. 특정 상태가 아닌 이상 평균 시간 복잡도는 nlogn이며 다른 알고리즘에 비해 속도가 매우 빠르다. (분할 정복 알고리즘인 merge sort에 비해서 2~3배 빠르다)
- 추가적인 별도의 메모리를 필요로 하지 않으며 재귀 호출 스택 프레임에 의한 공간 복잡도는 logn으로 메모리를 적게 소비한다.

### 단점

- 불안정 정렬이다.
- 최악의 경우 시간복잡도가 O(n^2)이다.
- 특정 조건하에 성능이 급격하게 떨어진다.
    - 내림차순으로 정렬되어있는데 피봇 값을 배열의 첫 index로 잡는 경우..등 정렬된 배열에서 시간복잡도가 O(n^2)이다.
    - 
<br>

## 소스코드

```bash
import java.io.*;
import java.util.*;
public class Main {
   static int number = 10;  //데이터의 개수
   static int[] arr = {1, 10, 5, 8, 7, 6, 4, 3, 2, 9};  //정렬할 배열

   public static void main(String[] args) throws IOException {
       quickSort(arr, 0, number - 1);
   }

   static void quickSort(int[] arr, int start, int end) {
       if (start >= end) {
           return;  //원소가 1개인 경우
       }

       int pivot = start;  //pivot값은 첫번째 원소
       int i = start + 1;  //시작점 - 왼쪽부터 큰 값을 찾을 때 시작하는 인덱스
       int j = end;  //도착점 - 오른쪽부터 작은 값을 찾을 때 시작하는 인덱스
       int temp;  //수를 바꿀 때 사용하는 임시 변수

       while (i <= j) { // 엇갈릴 때까지 반복
           while (arr[i] <= arr[pivot]) { //pivot보다 더 큰 값을 만날 때까지
               i++;
           }
           while (arr[j] >= arr[pivot] && j > start) { //pivot보다 더 작은 값을 만날 때까지
               j--;
           }
           if (i > j) { //현재 엇갈린 상태면 pivot값과 교체
               temp = arr[j];
               arr[j] = arr[pivot];
               arr[pivot] = temp;
           } else { //엇갈리지 않았다면 i와 j를 교체
               temp = arr[i];
               arr[i] = arr[j];
               arr[j] = temp;
           }
      }

       //분할된 왼쪽과 오른쪽 둘다 퀵 정렬 수행
       quickSort(arr, start, j - 1);
       quickSort(arr, j + 1, end);
   }
}
```

만약 내림차순으로 정렬하려면

```bash
import java.io.*;
import java.util.*;
public class Main {
    static int number = 10;  //데이터의 개수
    static int[] arr = {1, 10, 5, 8, 7, 6, 4, 3, 2, 9};  //정렬할 배열

    public static void main(String[] args) throws IOException {
        quickSort(arr, 0, number - 1);
    }

    static void quickSort(int[] arr, int start, int end) {
        if (start >= end) {
            return;  //원소가 1개인 경우
        }

        int pivot = start;  //pivot값은 첫번째 원소
        int i = start + 1;  //시작점 - 왼쪽부터 큰 값을 찾을 때 시작하는 인덱스
        int j = end;  //도착점 - 오른쪽부터 작은 값을 찾을 때 시작하는 인덱스
        int temp;  //수를 바꿀 때 임시 변수

        while (i <= j) { // 엇갈릴 때까지 반복
            while (arr[i] >= arr[pivot]) { //pivot보다 더 큰 값을 만날 때까지
                i++;
            }
            while (arr[j] <= arr[pivot] && j > start) { //pivot보다 더 작은 값을 만날 때까지
                j--;
            }
            if (i > j) { //현재 엇갈린 상태면 pivot값과 교체
                temp = arr[j];
                arr[j] = arr[pivot];
                arr[pivot] = temp;
            } else { //엇갈리지 않았다면 i와 j를 교체
                temp = arr[i];
                arr[i] = arr[j];
                arr[j] = temp;
            }
       }

        //분할된 왼쪽과 오른쪽 둘다 퀵 정렬 수행
        quickSort(arr, start, j - 1);
        quickSort(arr, j + 1, end);
    }
}
```

중간부분 피봇과 비교하는 코드만 바꾸면 된다.

<br>
<br>


**reference**

https://propercoding.tistory.com/195
