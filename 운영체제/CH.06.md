## 프로세스 동기화
<pre>⚙️협력적 프로세스: 시스템 내에서 실행 중인 다른 프로세스의 실행에 영향을 주거나 영향을 받는 프로세스      
=> 코드와 데이터를 다른 프로세스들과 공유한다
</pre>

### 프로세스 동기화의 배경
1. 프로세스들이 병렬 또는 병행 실행될 때 여러 프로세스가 공유하는 데이터의 무결성에는 문제가 없을까?        
    <pre>💡무결성: 데이터의 정확성, 일관성, 유효성이 유지되는 것</pre>
2. 데이터의 무결성이 망가지는 예시      
    <pre>두 개의 프로세스가 있고, counter라는 변수가 있음.     
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
    
    ❗️ 5번과 6번의 순서가 바뀐다면 counter는 6이 됨</pre>
3. 프로세스를 병행, 병렬 실행할 경우 데이터의 무결성이 왜 망가질까?     
    => 여러 프로세스가 동시에 같은 데이터를 조작하도록 허용했기 때문에
4. 경쟁 상태(Race Condition): 여러 프로세스가 공유 자원에 동시에 접근할 때, 공유 자원에 대한 접근 순서에 따라 실행 결과가 달라질 수 있는 상황
5. 경쟁 상태를 방지하기 위해 프로세스 동기화가 필요

### 임계영역 문제(The Critical-Section Problem)
1. 각 프로세스는 임계영역(Critical Section)이라 불리는 코드 영역을 포함
2. 임계영역: 다른 프로세스와 자원을 공유하는 코드 영역
3. 한 프로세스가 자신의 임계영역에서 수행하는 동안에는 다른 프로세스가 해당 영역에 들어갈 수 없음
4. 임계 영역을 기준으로 코드 영역은 다음과 같이 나눠짐      
    <pre>
    1. 진입 영역(Entry Section): 임계 영역으로 진입하기 전에 진입 허가를 요청하는 영역
    2. 퇴출 영역(Exit Section): 임계 영역이 끝난 직후 따라오는 코드 영역
    3. 나머지 영역(Remainder Section): 진입 영역, 퇴출 영역, 임계 영역을 제외한 나머지 코드 부분</pre>
5. 임계영역 문제에 대한 해결책은 세 가지 요구조건을 충족해야 함
    <pre>
    1. 상호 배제(Mutual Exclusion): 한 프로세스가 자신의 임계영역에서 실행되고 있다면, 다른 프로세스들은 해당 임계영역에 진입할 수 없음
    2. 진행(Progress): 임계영역에서 실행되는 프로세스가 없다면 임계영역으로 진입하려는 프로세스의 진입결정은 유한시간 내에 결정돼야 한다
    3. 한정된 대기(Bounded Waiting): 한 프로세스가 임계 영역에 대한 진입을 요청한 후에는 다른 프로세스가 해당 임계 영역에 진입하는 횟수는 제한되어야 한다</pre>

### 피터슨의 해결안(Peterson's Solution)
1. 임계영역 문제를 해결하기 위한 알고리즘 (소프트웨어 기반 해결책)
2. 프로세스들이 int형 변수 turn과 boolean형 배열 flag를 공유        
    - turn: 임계영역으로 진입할 순번
    - flag: 프로세스가 임계영역으로 진입할 준비가 되어있음을 나타냄
3. 피터슨의 해결안 과정
    - 프로세스 i와 j가 있다고 가정
    - 프로세스 i의 구조     
        <pre><code>
        do{
            flag[i] = true;
            turn = j;
            while(flag[j] && turn == j);

            /* Critical Section  */

            flag[i] = false;

            /* Remainder Section */
        }while(true);</code></pre>
    - 프로세스 j의 구조     
        <pre><code>
        do{
            flag[j] = true;
            turn = i;
            while(flag[i] && turn == i);

            /* Critical Section  */

            flag[j] = false;

            /* Remainder Section */
        }while(true);</code></pre>
    - 핵심은 `양보`에 있음
    <pre>
    1. 프로세스 i는 자신이 임계구역에 들어갈 준비가 되면 자신에 대한 flag 값을 true 로 만듬
    2. 동시에 turn 은 자신이 아닌 프로세스 j를 가리키게 함. 프로세스 j의 준비상태가 true 라고 한다면 프로세스 i는 4번 라인의 while 문의 모든 조건이 true가 되므로 무한루프를 돌게됨
    3. 이때 프로세스 j가 임계구역에 들어갈 준비가 되면 자신의 flag 를 true로 만들고 turn은 프로세스 i를 가리키게 됨
    4. 프로세스 j가 turn 을 i로 바꾸었기 때문에 프로세스 i는 무한루프에서 빠져나올 수 있음
    5. 무한루프에서 빠져나온 프로세스 i는 임계구역에 진입해서 작업을 수행한다. 이때 flag[i] == true, turn == i 가 모두 참이기 때문에 프로세스 j는 무한루프를 돌며 대기
    6. 프로세스 i가 임계구역을 빠져나오면서 flag[i]를 false 로 만듬
    7. 프로세스 j는 이제 flag[i] 가 false 가 되었기 때문에 무한루프를 빠져나와 임계구역으로 진입
    8. 위 과정을 반복</pre>
4. 피터슨의 해결안 분석
    - 상호 배제, 진행, 한정된 대기 요구조건 충족하는지 확인
    1. Mutual Exclusion(상호 배제) : 동시에 여러 프로세스가 임계구역에 진입하는 것을 허용하면 안된다. 이 조건은 while(flag[i] && turn == i) 에 의해 만족된다. 두 프로세스가 모두 준비상태에 있어 flag[i]와 flag[j]가 모두 true 를 가진다고 해도 turn 값은 언제나 i 혹은 j의 값을 하나만 가질 수 있다. 따라서 동시에 두 프로세스가 임계구역에 진입하는 것을 제한한다.
    2. Progress(진행) : 임계구역이 비어있을 때, 임계구역의 진입여부가 한정된 시간안에 결정되어야 한다. 이 조건은 임계구역을 빠져나오면서 실행되는 flag[i] = false 와 while(flag[i] && turn == i) 에 의해 만족된다. 프로세스 i 는 임계구역을 빠져나온 직후에 flag[i]를 false 로 만드는데, 이 코드 덕분에 무한루프를 돌며 대기중이던 프로세스 j가 임계구역으로 진입하는 것이 가능해진다. 즉, 임계구역이 비게되자마자 곧바로 다른 준비된 프로세스의 임계구역 진입을 허락하는 것이다.
    3. flag[i] = false 와 while(flag[i] && turn == i) 는 2번 조건을 만족시킴과 동시에 3번 조건도 만족시킨다. 이 코드를 통해서 두 프로세스는 무한정하게 임계구역 진입을 위해 기다리지 않고, 다른 한 프로세스가 임계구역을 빠져나오게 되면 곧바로 임계구역으로의 진입이 가능해진다.
5. 현대 컴퓨팅 시스템에서는 피터슨 해결책이 임계구역 문제를 항상 해결해주지는 않음

### 동기화 하드웨어(Synchronization Hardware)
1. 임계구역을 보호하기 위해 LOCK을 사용 (하드웨어적 해결책)
2. 프로세스는 LOCK을 획득해야만 임계영역에 진입할 수 있고, 임계영역 벗어나면 LOCK 반납
3. 코드로 표현한 결과
    <pre><code>while(true){
        록 획득
        //임계영역
        록 방출
        //나머지 영역
    }</code></pre>
4. 하드웨어 특징에 따라 임계영역 문제의 해결법이 간단하거나 복잡해질 수 있음 
    - 단일 처리기 환경
        => 공유 변수 변경되는 동안에는 인터럽트 발생을 허용하지 않음으로써 간단히 해결
    - 다중 처리기 환경
        1. 인터럽트 금지 해결책을 사용할 수 없음
        2. 모든 처리기에 인터럽트를 금지시키도록 해야 하므로 시간이 많이 소요됨     
        => 현재는 한 워드(word)의 내용을 검사하고 변경하거나, 두 워드의 내용을 원자적으로 교환할 수 있는, 즉 인터럽트 되지 않는 하나의 단위로서, 특별한 하드웨어 명령어들을 제공 (무슨 소린지 모르겠음)     
        => swap을 통해 여러 처리기의 공유 변수를 원자적으로 변경시켜 LOCK을 획득

### Mutex(Mutual Exclusion) Locks
1. lock 기법을 사용하는 하드웨어적 해결책
2. `임계구역을 보호`하고 `경쟁 상태를 방지`하기 위해 `mutex lock` 사용
3. 프로세스는 임계구역 들어가기 전에 반드시 `lock을 획득`해야 하고 빠져 나올 땐 반드시 `반환`해야 함
4. 전체적인 구조
    <pre><code>do {
        // lock을 획득
        /* 임계 구역 */
        // lock을 반환
        /* 나머지 구역 */
    } while(true);</code></pre>
4. 구현에 사용되는 함수와 변수
    - available: lock의 가용 여부를 표시하는 boolean형 변수
    - acquire() 함수: lock을 획득하는데 사용
        <pre><code>acquire() {
            while(!available) 
                ; /* busy wait */
            available = false;
        }</code></pre>
        => lock이 가용하면 acquire() 호출 성공하고 lock은 사용불가 상태가 됨        
        => 사용불가 상태의 lock을 획득하려고 시도하는 프로세스는 lock이 반환될 때까지 `봉쇄`됨
    - release() 함수: lock을 반환하는데 사용
        <pre><code>release() {
            available = true;
        }</code></pre>
        => lock은 사용가능 상태가 됨
5. acquire()와 release() 함수 호출은 원자적으로 수행돼야 함
    <pre>💡원자적 수행: 수행 도중에 중단되면 안된다는 뜻</pre>
6. `바쁜 대기(busy waiting)`를 해야 하기 때문에 `CPU 자원이 낭비`됨
    <pre><code>💡바쁜 대기(busy waiting): 원하는 자원을 얻기 위해 권한을 얻을 때까지 확인하는 것을 의미</code></pre>
    - 한 프로세스가 임계 구역에 있는 동안 임계구역에 들어가고 싶은 프로세스들은 acquire() 함수내의 반복문을 계속 실행        
    => 이런 유형의 mutex lock을 `spinlock`이라고 함
7. `spinlock`: 다른 프로세스가 lock을 소유하고 있다면 그 lock이 반환될 때까지 계속 확인하며 기다리는 것
    - lock을 기다리는 동안 많은 시간이 소요되는 문맥 교환이 이뤄지지 않는다는 장점 존재
    - 프로세스들이 짧은 시간 동안만 락을 소유할 경우 유용함
    - 다중 처리기에서 많이 사용됨       
        => 한 처리기에서 스레드가 임계구역 실행하는 동안, 다른 스레드는 다른 처리기에서 회전 수행

### 세마포(Semaphores)
1. 세마포 S는 `공유자원의 개수`를 나타내는 정수 변수!!
2. 초기화 제외하고 `wait()`과 `signal()` 함수로만 접근 가능
3. wait(): 세마포 S를 `검사`하고 `감소`하는 함수
    <pre><code>wait(S) {
        while(S <= 0)
            ; // 바쁜 대기
        S--;
    }</code></pre>
4. signal(): 세마포 S를 `증가`하는 함수
    <pre><code>signal(S) {
        S++;
    }</code></pre>
5. 한 스레드가 세마포 값을 변경할 때, 다른 스레드들은 동시에 동일한 세마포 값을 변경할 수 없다!!
6. 각 함수는 원자적으로 실행돼야 함
7. mutex lock과 마찬가지로 `바쁜 대기`를 해야함

#### 세마포 사용법
1. 세마포는 `카운팅 세마포`와 `이진 세마포`로 구분
2. 카운팅 세마포: 값의 범위가 정해져있지 않음
    - 가용한 자원의 개수가 `여러개`인 경우 쓰임
    - 초기에 세마포는 가용한 자원의 개수로 초기화
3. 이진 세마포: 0 or 1 값만 가짐
    - 가용한 자원의 개수가 `1개`일 때 쓰임
    - mutex lock과 유사하게 동작(mutex lock도 true or false)
    - 몇몇 시스템에서는 mutex lock 대신 이진 세마포가 사용됨
    - 초기에 세마포는 1로 초기화
4. 세마포 사용 과정
    - 자원을 사용하려는 프로세스는 세마포에 wait()연산을 수행       
        => 세마포 값 감소(가용 자원 수 감소)
    - 프로세스는 자원 방출할 때 signal() 연산 수행      
        => 세마포 값 증가(가용 자원 수 증가)
    - 세마포 값이 0이 되면 모든 자원이 사용 중임을 나타냄       
        => 이후에 자원 사용하려는 프로세스는 세마포 값이 0보다 커질 때까지 `봉쇄`됨
5. 동기화 문제 해결도 `가능`
    - P1은 S1 명령문을 P2는 S2 명령문을 병행하게 수행한다고 가정
    - S2는 S1이 끝난 뒤에만 수행돼야 하는 상황
    - 세마포 synch를 공유하고 초기값은 0
    - 전체 코드는 다음과 같음
        <pre><code>  S1;
        signal(synch);
        wait(synch);
        S2;</code></pre>
        => synch이 0이므로 P2는 P1이 signal(synch)를 호출한 후에 S2를 수행할 수 있음..!!

#### 구현
1. `바쁜 대기`의 문제점을 해결하기 위해 프로세스는 자기 자신을 `봉쇄`시킬 수 있음
2. 봉쇄 연산 과정
    <pre><code> 1. 봉쇄 연산은 프로세스를 세마포에 연관된 대기 큐에 넣고 프로세스의 상태를 `대기 상태`로 전환
    2. 이후 제어는 CPU 스케줄러로 넘어가고 스케줄러는 다른 프로세스를 실행하기 위해 선택</code></pre>
3. 세마포를 대기하며 봉쇄된 프로세스는 다른 프로세스가 signal() 연산을 실행하면 재시작 되어야 함
4. 프로세스는 wakeup() 연산에 의해 재시작됨
    <pre><code>wakeup() 연산: 프로세스의 상태를 대기상태에서 준비 완료 상태로 변경시킴</code></pre>
5. 프로세스는 준비 완료 큐에 넣어짐
6. 세마포의 구조
    <pre><code>typedef struct {
        int value;
        struct process *list;
    } semaphore;
    => 정수 value와 프로세스 리스트 list를 가짐
    - 프로세스가 세마포를 기다리게 될 경우, 해당 프로세스를 세마포의 프로세스 리스트에 추가
    - signal() 연산은 프로세스 리스트에서 한 프로세스를 꺼내서 그 프로세스를 깨워줌</code></pre>
7. 세마포의 구조에 따른 wait() 연산과 signal() 연산 코드
    - wait() 연산
        <pre><code>void wait(semaphore *S) {
            S->value--;
            if (S->value < 0) {
            이 프로세스를 S->list에 넣는다;
            block();
            }
        }</code></pre>
        => 이전의 wait() 연산에서 세마포 값의 감소와 검사의 순서를 바꿈!!       
        => 세마포 값이 음수 값을 가질 수 있게 되며 그 때 세마포의 값의 `절대 값`은 세마포를 대기하고 있는 프로세스들의 수가 됨
    - signal() 연산
        <pre><code>void signal(semaphore *S) {
            S->value++;
            if (S->value <= 0) {
                S->list로부터 하나의 프로세스 P를 꺼낸다;
                wakeup(P);
            }
        }</code></pre>
    - 같은 세마포에 대해 두 프로세스가 동시에 wait()과 signal() 연산들을 실행할 수 없다는 것을 명심     
    => 동시에 실행되면 당연히 임계구역 문제가 발생하게 되겠죠??
8. block() 연산은 자기를 호출한 프로세스를 `중지`시키는 역할
9. wakeup(P) 연산은 봉쇄된 프로세스 P의 실행을 재개시킴
10. block과 wakeup은 운영체제의 기본적인 시스템 호출로 제공됨

#### 교착 상태와 기아(Deadlock And Starvation)
1. 세마포를 사용하게 되면 `교착 상태`가 발생할 수 있음        
    <pre><code>💡교착 상태: 두 개 이상의 프로세스들이 각각 자원을 점유한 상태에서 서로 다른 프로세스가 점유하고 있는 자원을 요구하며 무한정 기다리는 상황</code></pre>
2. 교착 상태의 예시
    <pre><code>프로세스: P0, P1 / 세마포: S, Q => 1로 지정되어 있음     
    
    P0          P1
    wait(S);    wait(Q);
    wait(Q);    wait(S);
    .           .
    .           .
    signal(S);  signal(Q);
    signal(Q);  signal(S);      
    
    위와 같은 순서로 연산이 동시에 실행된다고 하면
    P0은 P1이 Q를 방출해주기를 기다려야 하고,
    P1은 P0이 S를 방출해주기를 기다려야 함
    둘 다 다음 연산은 실행될 수 없으니 교착 상태가 발생하게 되겠죠?!
    </code></pre>
3. 교착 상태의 핵심은 자원의 `획득`과 `방출`
4. 교착 상태와 연관된 다른 문제점: `무한 봉쇄`또는 `기아`      
    <details>
    <summary>Q. 무한 봉쇄와 기아는 무엇일까요?</summary>
    <pre><code>💡 무한 봉쇄: 실행 준비는 되어 있으나 자원을 할당받지 못하는 프로세스들이 자원을 기다리면서 봉쇄된 것으로 간주되는 것
    💡 기아: 우선 순위가 높은 프로세스에 의해 우선 순위 낮은 프로세스들이 자원을 할당받지 못하고 무한정 대기하는 상태</code></pre>
    </details>

#### 우선순위 역전(Priority Inversion)


