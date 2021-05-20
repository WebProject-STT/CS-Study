# CH.03 검색

## 1. 검색 알고리즘

```
💡 검색 알고리즘은 데이터 집합에서 원하는 값을 가진 요소를 찾아내는 알고리즘
```

### 검색 종류
1. 배열 검색 <br/> ![Array Search](./img/03.array_search.PNG)
2. 선형 리스트 검색 - 9장 <br/> ![Linear List Search](./img/03.linear_list_search.PNG)
3. 이진검색트리 검색 - 10장 <br/> ![Binary Search Tree Search](./img/03.binary_search_tree_search.PNG)

> 이 장에서는 `배열 검색`에 대해 정리되어 있음

### 배열 검색 알고리즘
1. 선형 검색 : 무작위로 늘어놓은 데이터 모임에서 검색 수행
2. 이진 검색 : 일정한 규칙으로 늘어놓은 데이터 모임에서 아주 빠른 검색 수행
3. 해시법 : 추가, 삭제가 자주 일어나는 데이터 모임에서 아주 빠른 검색 수행
   - 체인법 : 같은 해시 값의 데이터를 선형 리스트로 연결하는 방법
   - 오픈 주소법 : 데이터를 위한 해시 값이 충돌할 때 재해시하는 방법

<br/>

## 2. 선형 검색

### 선형 검색
- 요소가 직선으로 늘어선 배열에서의 검색으로,
- 원하는 키 값을 갖는 요소를 만날 때까지 맨 앞부터 `순서대로` 요소 검색
- `순차 검색`이라고도 함

![Linear Search](./img/03.linear_search.png)

- 선형 검색의 검색 종료 조건
  1. 검색할 값을 발견하지 못하고 배열의 끝이 지나간 경우
  2. 검색할 값과 같은 요소를 발견한 경우

- 배열의 요솟수가 n개일 때 조건1, 2를 판단하는 횟수는 평균 `n/2회`

### 보초법
- 선형 검색의 **종료 조건을 검사하는 비용**을 `반으로 줄이는 방법`
- 배열의 맨 끝 요소에 `보초`로 키 값과 동일한 값을 저장한 후 검색 진행 <br/> => `종료 조건2`만으로 수행 가능하기 때문에 종료 판단 횟수를 2회에서 1회로 줄여줌

```java
import java.util.Scanner;

class SeqSearchSen {

    static int seqSearchSen(int[] a, int n, int key) {
        int i = 0;

        a[n] = key; // 보초 추가

        while (true) { // 순서대로 검색
            if (a[i] == key) break; // 검색 성공
            i++;
        }

        return i == n ? -1 : i; // 보초인지 아닌지 판단
    }

    public static void main(String[] args) {
        Scanner stdIn = new Scanner(System.in);

        int num = stdIn.nextInt(); // 요솟수
        int[] x = new int[num+1]; // 보초를 저장하기 위해 요솟수 + 1만큼 배열 생성

        for (int i = 0; i < num; i++) {
            x[i] = stdIn.nextInt(); // 요소 추가
        }

        int ky = stdIn.nextInt(); // 검색할 값

        int idx = seqSearchSen(x, num, ky); // 배열 x에서 값이 ky인 요소 검색
        
        if (idx == -1)
            System.out.println("요소가 존재하지 않음")
        else
            System.out.println("요소는 x["+dix"]에 존재함")
    }
}
```

<br/>

## 3. 이진검색

### 이진 검색
### 복잡도
### Arrays.binarySearch에 의한 이진 

---
### 🎨 이미지 출처
- https://senalyst.com/algo/34/
- https://rumor1993.tistory.com/38