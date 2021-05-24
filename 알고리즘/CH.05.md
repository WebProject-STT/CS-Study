# CH.05 재귀 알고리즘

## 1. 재귀의 기본

### 재귀란?
어떤 사건이 자기 자신을 포함하고 다시 자기 자신을 사용하여 정의될 때 `재귀적` 이라고 함

### 팩토리얼 구하기
- 음이 아닌 정수의 `팩토리얼`을 구하는 프로그램
- 규칙
  1. 0! = 1
  2. n > 0이면 n! = n * n(n-1)!

```java
class Factorial {
    static int factorial(int n) {
        if (n > 0) return n * factorial(n - 1); // 재귀 호출
        else return 1;
    }

    public static void main(String[] args) {
        Scanner stdIn = new Scanner(System.in);
        int x = stdIn.nextInt(); // 팩토리얼을 구할 정수
        System.out.println(x + "의 팩토리얼은 " + factorial(x) + "입니다." );
    }
}
```

위의 코드와 같이 factorial 메서드 내부에서 factorial 메서드를 호출하는 형태는 `직접(direct) 재귀` 라고 함.

**참고**
```
- 직접(direct) 재귀 : 메서드 a가 자신과 같은 메서드 a를 호출하는 구조
- 간접(indirect) 재귀 : 메서드 a가 메서드 b를 호출하고, 메서드 b가 메서드 a를 호출하는 구조
```

### 유클리드 호제법
- 두 정수의 `최대공약수`를 재귀적으로 구하는 방법

```java
import java.util.Scanner;

class EulidGCD() {

    // 정수 x, y의 최대공약수를 구하여 반환
    static int gcd (int x, int y) {
        if (y == 0) return x;
        else return gcd(y, x % y);
    }

    public static void main(String[] args) {
        Scanner stdIn = new Scanner(System.in);

        int x = stdIn.nextInt();
        int y = stdIn.nextIn();

        System.out.println("최대공약수는 " + gcd(x, y) + "입니다.");
    }
}
```

<br/>

## 2. 재귀 알고리즘 분석

### 하향식 분석
- 가장 위쪽에 위치한 메서드 호출부터 시작해 계단식으로 자세히 조사하는 분석 기법
- 꼭대기(top)부터 분석하면 같은 메서드의 호출이 여러 번 나올 수 있기 때문에 반드시 효율적이라고 할 수는 없음

### 상향식 분석
- 아래쪽부터 쌓아 올리며 분석하는 방법

<br/>

## 3. 하노이의 탑

작은 원반이 위에, 큰 원반이 아래에 위치할 수 있도록 원반을 3개의 기둥 사이에서 옮기는 문제

```java
import java.util.Scanner;

class Hanoi {
    // no개의 원반을 x번 기둥에서 y번 기둥으로 옮김
    static void move(int no, int x, int y) {
        if (no > 1) move(no-1, x, 6-x-y); // 1 -> 2

        if (no > 1) move(no-1, 6-x-y, y); // 2 -> 3
    }

    public static void main(String[] args) {
        Scanner stdIn = new Scanner(System.in);

        int n = stdIn.nextInt(); // 원반 개수

        move(n, 1, 3); // 1번 기둥의 n개의 원반을 3번 기둥으로 올김
    }
}
```

<br/>

## 4. 8퀸 문제

### 8퀸 문제란?
- 서로 공격하여 잡을 수 없도록 8개의 퀸을 8 x 8 체스판이 놓는 문제
- 92가지의 조합이 존재함

<<생략>>
