# CH.08 문자열 검색

## 1. 브루트-포스법

- 다른 문자를 만나면 패턴을 1칸씩 옮긴 다음 다시 패턴의 처음부터 검사
- 선형 검색을 확장한 알고리즘
- 단순법, 소박법이라고도 함
- 텍스트 "ABABCDEFGHA"에서 패턴 "ABC"를 `브루트-포스법`을 사용해 검색하는 순서

    ![brute force](./img/08.brute_force_method.png)
- 문자열 검색 프로그램
    ``` java
    import java.util.Scanner;

    class BFmath {
        // 브루트-포스법으로 문자열을 검색하는 메서드
        static int bfMatch(String txt, String pat) {
            int pt = 0; // txt 커서
            int pp = 0; // pat 커서

            while (pt != txt.length() && pp != pat.length()) {
                if (txt.charAt(pt) == pat.charAt(pp)) {
                    pt++;
                    pp++;
                } else {
                    pt = pt - pp + 1;
                    pp = 0;
                }
            }

            if (pp == pat.length()) // 검색 성공!
                return pt - pp;
            return -1; // 검색 실패!
        }

        public static void main(String[] args) {
            Scanner stdIn = new Scanner(System.in);

            String s1 = stdIn.next(); // 텍스트용 문자열
            String s2 = stdIn.next(); // 패턴용 문자열

            int idx = bfMatch(s1, s2); // 문자열 s1에서 문자열 s2 검색

            if (idx == -1)
                System.out.println("텍스트 안에 패턴 없음");
            else
                System.out.println((idx + 1) + "번째 문자부터 일치합니다.");
        }
    }
    ```

<br/>

## 2. KMP법

- 검사했던 위치 결과를 버리지 않고 이를 효율적으로 활용하는 알고리즘
- 

<br/>

## 3. Boyer-Moore법

- KMP법보다 효율이 더 우수하기 때문에 실제로 문자열 검색에 널리 사용하는 알고리즘