## 프로세스 동기화
<code>⚙️협력적 프로세스: 시스템 내에서 실행 중인 다른 프로세스의 실행에 영향을 주거나 영향을 받는 프로세스      
=> 코드와 데이터를 다른 프로세스들과 공유한다
</code>

### 프로세스 동기화의 배경
1. 프로세스들이 병렬 또는 병행 실행될 때 여러 프로세스가 공유하는 데이터의 무결성에는 문제가 없을까?        
    <code>💡무결성: 데이터의 정확성, 일관성, 유효성이 유지되는 것</code>
2. 데이터의 무결성이 망가지는 예시      
    <code>두 개의 프로세스가 있고, counter라는 변수가 있음.     
    counter의 값은 5이고, 두 프로세스는 "counter++"와 "counter--"를 병행하게 실행한다 가정      
    ❗️ counter의 올바른 최종값은 5      
    "counter++"의 실행 과정은 다음과 같음       
    1. register1 = counter
    2. register1 = register1 + 1
    3. counter = register1

    "counter--"의 실행 과정은 다음과 같음       
    1. register2 = counter
    2. register2 = register2 - 1
    3. counter = register2

    위의 두 연산을 병행하게 실행한다면 다음과 같은 순서로 실행될 수 있음
    1. register1 = counter 수행 (register1 = 5)
    2. register1 = register1 + 1 수행 (register1 = 6)
    3. register2 = counter 수행 (register2 = 5)
    4. register2 = register2 - 1 수행 (register2 = 4)
    5. counter = register1 수행 (counter = 6) 
    6. counter = register2 수행 (counter = 4)     
    
    ❗️ 5번과 6번의 순서가 바뀐다면 counter는 6이 됨     
    </code>
3. 프로세스를 병행, 병렬 실행할 경우 데이터의 무결성이 왜 망가질까?     
    => 여러 프로세스가 동시에 같은 데이터를 조작하도록 허용했기 때문에
4. 경쟁 상태(Race Condition): 여러 프로세스가 공유 자원에 동시에 접근할 때, 공유 자원에 대한 접근 순서에 따라 실행 결과가 달라질 수 있는 상황
5. 경쟁 상태를 방지하기 위해 프로세스 동기화가 필요

### 임계영역 문제(The Critical-Section Problem)
1. 각 프로세스는 임계영역(Critical Section)이라 불리는 코드 영역을 포함
2. 임계영역: 다른 프로세스와 자원을 공유하는 코드 영역
3. 한 프로세스가 자신의 임계영역에서 수행하는 동안에는 다른 프로세스가 해당 영역에 들어갈 수 없음
4. 임계 영역을 기준으로 코드 영역은 다음과 같이 나눠짐      
    <code>
    1. 진입 영역(Entry Section): 임계 영역으로 진입하기 전에 진입 허가를 요청하는 영역
    2. 퇴출 영역(Exit Section): 임계 영역이 끝난 직후 따라오는 코드 영역
    3. 나머지 영역(Remainder Section): 진입 영역, 퇴출 영역, 임계 영역을 제외한 나머지 코드 부분
    </code>
5. 임계영역 문제에 대한 해결책은 세 가지 요구조건을 충족해야 함
    <code>
    1. 상호 배제(Mutual Exclusion): 한 프로세스가 자신의 임계영역에서 실행되고 있다면, 다른 프로세스들은 해당 임계영역에 진입할 수 없음
    2. 진행(Progress): 임계영역에서 실행되는 프로세스가 없다면 임계영역으로 진입하려는 프로세스의 진입결정은 유한시간 내에 결정돼야 한다
    3. 한정된 대기(Bounded Waiting): 한 프로세스가 임계 영역에 대한 진입을 요청한 후에는 다른 프로세스가 해당 임계 영역에 진입하는 횟수는 제한되어야 한다
    </code>

### 피터슨의 해결안(Peterson's Solution)
1. 임계영역 문제를 해결하기 위한 알고리즘
2. 프로세스들이 int형 변수 turn과 boolean형 배열 flag를 공유        
    - turn: 임계영역으로 진입할 순번
    - flag: 프로세스가 임계영역으로 진입할 준비가 되어있음을 나타냄
3. 피터슨의 해결안 과정
    - 프로세스 i와 j가 있다고 가정
    - 프로세스 i의 구조     
        <code>
        do{
            flag[i] = true;
            turn = j;
            while(flag[j] && turn == j);

            /* Critical Section  */

            flag[i] = false;

            /* Remainder Section */
        }while(true);
        </code>
    - 프로세스 j의 구조     
        <code>
        do{
            flag[j] = true;
            turn = i;
            while(flag[i] && turn == i);

            /* Critical Section  */

            flag[j] = false;

            /* Remainder Section */
        }while(true);
        </code>
    - 핵심은 `양보`에 있음