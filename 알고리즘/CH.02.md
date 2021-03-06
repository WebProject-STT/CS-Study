# CH.02 기본 자료구조

> 💡 자료구조란 데이터 단위와 데이터 자체 사이의 물리적 또는 논리적 관계를 의미함

## 1. 배열
같은 자료형의 구성 요소가 직선 모양으로 연속하여 줄지어 있는 단순한 자료구조

```java
int[] a; // int형 자료형을 가진 배열 a 선언
a = new int[5]; // a는 길이가 5인 배열 참조
```

### 구성요소
```java
배열 변수 이름 [인덱스] // 배열 안의 임의의 구성 요소
```

### 구성 요소 길이
```java
배열 변수 이름.length
```

### 배열의 요솟값을 초기화하며 배열 선언하기
```java
int[] a = {1, 2, 3, 4, 5};
```

### 배열의 복제 (클론)
```java
배열 이름.clone()
```

### 다차원 배열
아래는 `int형을 구성 자료형으로 하는 배열`을 구성 자료형으로 하는 배열
```java
int[][] x = new int[2][4];
```



## 2. 클래스
임의의 데이터형을 자유로이 조합하여 만들 수 있는 자료구조

### 클래스 선언
```java
class XYZ {
    int x;
    long y;
    double z;
}
```

### 클래스형 변수 선언 및 인스턴스 생성
```java
XYZ a; // XYZ형의 클래스형 변수 a 선언
a = new XYZ(); // XYZ형의 클래스 인스턴스(실체) 생성
// 위의 두 가지 한 번에 수행
XYZ a = new XYZ();
```

> 💡 아래 내용은 클래스에 대해 알아야 할 최소 사항

### 클래스 본체와 멤버
1. 클래스 본체에서는 `멤버, 클래스 초기화/인스턴스 초기화, 생성자` 선언 가능
2. 필드/메서드/생성자를 선언할 때 `public/protected/private` 지정 가능
3. 메서드/생성자는 다중으로 정의(오버로드) 할 수 있음
4. final로 선언한 필드는 한 번만 값을 대입할 수 있음
5. 생성자는 새로 생성한 인스턴스의 초기화를 위해 사용됨

### 공개 클래스
1. 클래스 접근 제한자 public을 붙여 선언한 클래스
2. 다른 패키지에서 사용 가능

### final 클래스
1. 클래스 접근 제한자 final을 붙여 선언한 클래스
2. 서브 클래스를 가질 수 없음

### 파생 클래스
1. 클래스 A를 직접 상위 클래스로 하려면 선언할 때 extends A를 추가해야 함
2. 이 때 선언한 클래스는 클래스 A의 직접 서브 클래스가 됨
3. 클래스 선언에 extends가 없는 클래스의 상위 클래스는 Object 클래스

### 인터페이스 구현
인터페이스 X를 구현하려면 선언에 implements X를 추가해야 함

### 추상 클래스
1. 클래스 접근 제한자 abstract를 붙여 클래스를 선언하면 추상 메서드를 가질 수 있는 추상 클래스가 됨
2. 추상 클래스형은 불완전한 클래스이므로 인스턴스 생성 불가

### 중첩 클래스
클래스 또는 인터페이스 안에 선언한 클래스
- 멤버 클래스 : 선언이 다른 클래스 또는 인터페이스 선언에 둘러싸인 클래스
- 내부 클래스 : 
  1. 명시적으로도 암묵적으로도 정적(static)으로 선언되지 않은 중첩 클래스
  2. 정척 초기화나 멤버 인터페이스 선언 불가
  3. 컴파일 시 상수 필드가 아닌 한 정적 멤버 선언 불가
- 지역 클래스 : 이름이 주어진 중첩 클래스인 내부 클래스로, 어떤 클래스의 멤버도 될 수 없음