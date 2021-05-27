# CH.06 정렬

## 1. 정렬

- 대소 관계에 따라 데이터 집합을 일정한 순서로 줄지어 늘어서도록 바꾸는 작업
- 데이터를 정렬하면 검색을 더 쉽게 할 수 있음
- 아래 그림처럼 키 값이 작은 데이터를 앞쪽에 놓으면 `오름차순 정렬`, 그 반대로 놓으면 `내림차순 정렬` 이라고 함

    ![정렬](./img/06.sorting.png)

### 정렬 알고리즘의 안정성
- 안정된 정렬 : 같은 값의 키를 가진 요소의 순서가 정렬 전후에도 유지되는 것 <br/> ![안정된 정렬](./img/06.stable.png)
- 안정되지 않은 정렬 : 같은 값의 키를 가진 요소의 순서가 정렬 전후에 유지되지 않는 것 <br/> ![안정되지 않은 정렬](./img/06.unstable.png)

### 내부 정렬과 외부 정렬
- 내부 정렬 : 정렬할 모든 데이터를 하나의 배열에 저장할 수 있는 경우에 사용하는 알고리즘
- 외부 정렬 : 정렬할 데이터가 너무 많아서 하나의 배열에 저장할 수 없는 경우에 사용하는 알고리즘

### 정렬 알고리즘의 핵심 요소
```
👉 교환, 선택, 삽입
```
<br/>

## 2. 버블 정렬

이웃한 두 요소의 대소 관계를 비교하여 교환을 반복

![bubble sort](./img/06.bubble_sort.png)

위의 그림을 코드로 표현하면 다음과 같음

```java
import java.util.Scanner;

class BubbleSort {
    static void swap(int[] a, int idx1, int idx2) { // a[idx1]과 a[idx2] 값 변경
        int t = a[idx1]; 
        a[idx1] = a[idx2];
        a[idx2] = t;
    }

    static void bubbleSort(int[] a, int n) { // 버블 정렬
        for (int i = 0; i < n-1; i++)
        for (int j = n-1; j > i; j--)
            if (a[j-1] > a[j])
                swap(a, j-1, j);
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        int nf = sc.nextInt(); // 요솟수
        int[] x = new int[n];

        for (int i = 0; i < n; i++) {
            x[i] = sc.nextInt(); // 정렬해야하는 배열의 숫자를 입력 받음
        }

        bubbleSort(x, n); // 배열 x 버블 정렬
    }
}
```

이러한 형태로 코드를 작성하면 중간에 정렬이 끝난 경우에도 계속 검사를 진행하게 되어 불필요한 비교 연산 시간이 발생하게 됨

이러한 문제를 해결하려면 다음과 같은 형태로 버블 정렬을 구현

```java
static void bubbleSort(int[] a, int n) {
    for (int i = 0; i < n-1; i++) {
        int exchg = 0; // 교환 횟수 기록
        for (int j = n-1; j > i; j--)
            if (a[j-1] > a[j]) {
                swap(a, j-1, j);
                exchg++;
            }
        if (exchg == 0) break; // 교환이 이루어지지 않았으므로 종료
    }
}
```

하지만 위의 코드는 일부분이 이미 정렬이 되어 있는 경우에도 코드 검사를 진행하게 됨

일부분이 이미 정렬되어 있는 상황에서 불필요한 검사를 줄이기 위해서는 마지막 교환 위치부터 정렬을 진행하면 됨

위와 같은 형태로 코드를 수정하면 다음과 같음

```java
static void bubbleSort(int[] a, int n) {
    int k = 0;
    while(k < n-1) {
        int last = n-1;
        for (int j = n-1; j > k; j--)
        if (a[j-1] > a[j]) {
            swap(a, j-1, j);
            last = j;
        }
        k = last;
    }
}
```

<br/>

## 3. 단순 선택 정렬

 - 가장 작은 요소부터 정렬하는 알고리즘
 - 교환 과정
   1. 아직 정렬하지 않은 부분에서 가장 작은 키의 값(a[min]) 선택
   2. a[min]과 아직 정렬하지 않은 부분의 첫 번째 요소 교환
- n-1회 반복

![straight selection sort](./img/06.straight_selection_sort.png)

```java
static void selectionSort(int[] a, int n) {
    for (int i = 0; i < n-1; i++) {
        int min = i; // 아직 정렬되지 않은 부분에서 가장 작은 요소의 인덱스 기록
        for (int j = i+1; i < n; j++) 
            if (a[j] < a[min]) min = j;
        swap(a, i, min); // 아직 정렬되지 않은 부분의 첫 요소와 가장 작은 요소 교환
    }
}
```

<br/>

## 4. 단순 삽입 정렬

- 선택한 요소를 그보다 더 앞쪽의 알맞은 위치에 `삽입하는` 작업을 반복하여 정렬하는 알고리즘
- n-1회 반복하면 정렬을 마치게 됨

![straight insertion sort](./img/06.straight_insertion_sort.png)

```java
static void insertionSort(int[] a, int n) {
    for (int i = 1; i < n; i++) {
        int j;
        int tmp = a[i]; // a[i] 위치의 값을 저장
        for (j = i; j > 0 && a[j-1] > tmp; j--) // tmp 값이 더 작으면 앞의 인덱스로 이동
            a[j] = a[j-1]; // 값이 뒤로 1칸씩 이동
        a[j] = tmp; // j에 기존의 a[i] 값 대입
    }
}
```

<br/>

```
💡 지금까지 나온 버블, 선택, 삽입 정렬과 같은 단순 정렬의 시간 복잡도는 모두 O(n^2) (효율이 좋지 않음)
```

## 5. 셸 정렬

<br/>

## 6. 퀵 정렬

<br/>

## 7. 병합 정렬

<br/>

## 8. 힙 정렬

<br/>

## 9. 도수 정렬