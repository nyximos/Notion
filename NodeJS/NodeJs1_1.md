# 1장 노드 시작하기

### 1.1.1 서버

**Node.js?**
Chrome V8 Javascript 엔진으로 빌드된 Javascript 런타임 (JS 실행기)

- **런타임** : 실행환경, jvm과 유사
    - 특정 언어로 만든 프로그램들을 **실행**할 수 있는 환경
    - 컴퓨터 과학에서 컴퓨터 프로그램이 실행되고 있는 동안의 환경
    - 자바스크립트를 웹 브라우저 밖 환경에서도 쓸 수 있게 해주는 프로그램

- **서버** : 네트워크를 통해 클라이언트에 정보나 서비스를 제공 하는 컴퓨터 · 프로그램

                클라이언트의 요청에 대해 응답 Response 

- **클라이언트** :  요청 Request 을 보내는 주체
- **엔진** : 해석기

- **노드의 핵심개념**
    1. 이벤트 기반
    2. 논블로킹 I/O
    3. 싱글 스레드

### 1.1.2 자바 스크립트 런타임

- **노드** : Chrome V8 Javascript 엔진으로 빌드된 ** Javascript 런타임**

    → 자바 스크립트 실행기

    → 브라우저,html이 없어도 JS를 돌릴 수 있음 

- **기존** :  JS를 웹 브라우저 위 에서만 실행 (브라우저에 JS 런타임 내장)
- **크롬 출시** : V8 엔진을 사용하여 다른 자바스크립트 엔진과 달리 매우 빠름, 오픈 소스로 코드 공개

                    → **속도 문제 해결**

- Ryan Dahl이 V8 엔진 기반의 노드 프로젝트 시작
- 노드 구조

    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5da1815c-6746-4cb4-ba8f-089191484f18/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5da1815c-6746-4cb4-ba8f-089191484f18/Untitled.png)

    - libuv 라이브러리 : 노드의 특성인 이벤트 기반, 논 블로킹 I/O 모델 구현
    - V8, libuv : C와 C++로 구현
    - 노드가 알아서 자바스크립트 코드와 V8, libuv에 자동 연결

### 1.1.3 이벤트 기반 Event Driven

- **이벤트 기반 event-driven**

    이벤트 발생할 때 미리 지정해둔 작업을 수행하는 방식

- **이벤트 = 사건**

    시스템의 상태 변화가 있는 것

    ex) 클릭, 화면전환, 키보드 입력, 네트워크 요청 등등

- **이벤트 기반 순서**
    1. 이벤트 리스너에 콜백 함수 등록
    2. 이벤트 발생                       (시스템 → 이벤트 리스너)
    3. 등록된 콜백 함수 호출      (이벤트 리스너 → 시스템)
- **이벤트 루프 event loop**

      여러 이벤트가 동시에 발생했을 때 어떤 순서로 콜백 함수를 호출할지를 판단

1. 노드가 JS 코드의 맨 위부터 한 줄씩 실행

 2. 함수 호출 부분을 발견했다면 호출한 함수를  호출 스택 call stack 에 넣음

- **call stack (LIFO) - queue (FIFO)**
- **호출 스택**

    호출 순서대로 입력되어 맨 마지막 호출된 함수 호출

    호출된 순서와 반대로 실행 완료

- **호출 스택 예시**

    ```jsx
    function first() {
    second();
    console.log('첫번째');
    }
    function second() {
    third();
    console.log('두번째');
    }
    function third() {
    second();
    }
    first;
    ```

    호출 스택 그려보면 쉽게 어떤 로그를 남길지 파악 가능

    - 호출 스택
    - 백그라운드
    - 태스크 큐
    - 콘솔창
    - 메모리
- **anonymous 함수**
    - 전역 컨텍스트 global context
    - 처음 실행시 호출
    - 호출 스택에 맨 처음 입력
    - 전역 컨택스트 ↔ 지역 컨택스트
- **컨택스트 context**
    - 함수가 호출되었을때 **생성되는 환경**
    - 함수가 호출되었을 때 생성되는 환경 함수는 실행되는 동안 호출 스택에 머물러 있다가
    - 실행이 완료되면 호출 스택에서 지워짐
    - 함수는 context를 가진다.
    - third , second, first, anonymous 순으로 지워짐
- **setTimeout**

    콜백 함수, 특정 밀리초 (1초/1,000분) 이후에 코드 실행

- **3초 뒤에 run 함수를 실행하는 코드**

    ```jsx
    function run() {
    	console.log('3초 후 실행');
    }
    console.log('시작');
    setTimeout(run,3000);
    console.log('끝');
    ```

- **이벤트 루프**
    - 이벤트 발생 시 호출할 콜백 함수를 관리
    - 호출된 콜백 함수의 실행 순서 결정
    - 테스크 큐에 있는 콜백을 호출 스택에 입력
    - 호출 스택이 비어 있으면 태스크 큐에서 함수를 하나씩 가져와 호출 스택에 넣고 실행
- **백그라운드**
    - setTime 같은 타이머나 이벤트 리스너가 대기하는 곳
    - JS가 아닌 다른 언어로 작성된 프로그램
    - 여러 작업이 동시에 실행될 수 있음
- **태스크 큐 = 콜백 큐**
    - 콜백 함수들이 대기하는 큐
    - FIFO 형태의 배열
    - 이벤트가 발생하면 백그라운드에 있는 콜백 함수를 가져와서 입력
- **콜백 함수**

    이름 그대로 나중에 호출되는 함수

    어떤 이벤트가 발생했거나 특정 시점에 도달했을 때 시스템에서 호출하는 함수

    Non-Block, 비동기 Asynchronous

```jsx
function run() { 
        console.log('3초 후 실행');
    }

console.log('시작');
setTimeout(run,3000); // 3초 뒤에 run 함수를 실행
console.log('끝');
```

- **이벤트 루프1**

    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7ffbcc2c-0d76-43a2-bb81-a0546238e77b/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7ffbcc2c-0d76-43a2-bb81-a0546238e77b/Untitled.png)

    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f5ecd861-39f3-471e-9495-f246df0900c4/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f5ecd861-39f3-471e-9495-f246df0900c4/Untitled.png)

    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0db041e3-83f4-437c-b946-22de6b7b2d0a/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0db041e3-83f4-437c-b946-22de6b7b2d0a/Untitled.png)

    1. 먼저 전역 컨택스트인 anonymous 함수 호출 스택 입력
    2. console.log('시작')이 호출스택 입력 후 실행되고 아웃
    3. setTimeout(run,3000)가 호출스택에 입력되고 실행된 후 아웃
    3-1. run()이 백그라운드로 타이머와 함께 입력
    4. console.log('끝')이 호출스택 입력 후 실행되고 아웃
    5. anonymous 함수가 아웃
    6. 백그라운드의 타이머가 3초가 끝나는 이벤트가 발생하면
    run에 태스크 큐를 입력
    7. 이벤트루프는 태스크큐에 있는 run을 호출스택에 입력
    8. 호출스택의 run이 실행되고 아웃
    - **태스크 큐**는 여러 개의 큐로 이루어짐

### 1.1.4 논블로킹 I/O

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/284932bb-546c-408a-8649-26089f1707a0/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/284932bb-546c-408a-8649-26089f1707a0/Untitled.png)

- **Blocking**
    - **순서대로 실행**
    - 호출된 함수가 바로 리턴 x
    - 이전 작업이 끝나면 다음 작업 수행
    - 함수가 실행되는 동안 다른 작업 못함
    - 실행 context를 알아야 한다.
- **Non-Blocking**
    - **순서대로 실행되지 않는다.**
    - 호출된 함수가 바로 리턴
    - 이전 작업이 완료될 때까지 대기하지 않고 다음 작업 수행
    - 함수가 실행되는 동안 다른 작업을 수행할 수 없음
    - 같은 작업을 더 짧은 시간에 처리할 수 있음
    - 논블로킹 ≠ 동시
    - 이벤트 루프를 알아야한다.
- **동시성**

     동시 처리가 가능한 작업을 논블로킹 처리해야 얻을 수 있다.

- **Synchronous (동기) 同期**

    호출되는 함수의 작업 완료를 **호출한 함수**가 신경 씀

- **Asynchronous (비동기) : 非同期**

    호출되는 함수의 작업 완료를 **호출된 함수**가 신경 씀

블로킹은 동기고 논블로킹은 비동기이다.

- **블로킹 수업 예제**

    ```jsx
    function longRunningTast() {
    //오래 걸리는 작업
    var sum = 0;
    for(var i=1;i<=1000000000;i++)
    sum+=i;

    console.log('작업끝');
    }

    console.log('시작');
    longRunningTask();
    console.log('다음 작업');
    ```

    시작

    작업 끝

    다음 작업

- **논블로킹 수업 예제**

    ```jsx
    function longRunningTast() {
    //오래 걸리는 작업
    var sum = 0;
    for(var i=1;i<=1000000000;i++)
    sum+=i;

    console.log('작업끝');
    }
    console.log('시작');
    longRunningTask(longRunningTask,0);
    // 논블로킹 만들기 기법, setImmediate
    console.log('다음 작업');
    ```

    시작

    다음 작업

    작업 끝

    - **setTimeout(콜백, 0)** : 코드를 논 블로킹으로 만들기 위해 사용하는 기법중 하나
    - **longRunningTask**가 태스크큐로 보내지므로 순서대로 실행되지 않는다.
    - 다음 작업이 먼저 실행된 후, 오래 걸리는 작업이 완료.

[http://homoefficio.github.io/2017/02/19/Blocking-NonBlocking-Synchronous-Asynchronous/](http://homoefficio.github.io/2017/02/19/Blocking-NonBlocking-Synchronous-Asynchronous/)

### 1.1.5 싱글 스레드 Single Thread

- 노드 프로세스 : node.exe
- **노드**
    - 여러 스레드를 가짐
    - but 프로그래머인 우리가 제어할 수 있는 스레드는 하나

       * 노드 12버전 이후부터는 멀티 스레드 가능

- **프로세스 vs 스레드**
    - **프로세스 process**
        - 실행 중인 하나의 애플리케이션
        - 운영체제 OS에서 할당하는 작업의 단위
        - 메인 메모리의 로드된 실행 파일
        - 애플리케이션을 실행하면 운영체제는 메모리를 할당하여 애플리케이션의 코드를 실행시킨다
        - 하나의 애플리케이션은 다중 프로세스를 만들기도 한다.
        - 프로세스 간 메모리 등 자원 공유 x

        ex) 노드나 웹브라우저

    - **스레드**
        - 프로세스 내에서 실행되는 작업의 단위
        - 하나의 코드 실행 흐름
        - 부모 프로세스의 자원을 공유
        - 같은 주소의 메모리에 접근 가능 → 데이터 공유 가능

        *** 프로세스**는 스레드를 여러 개 생성해 여러 작업을 동시에 처리

- **메인 스레드 Main Thread**
    - 모든 자바 애플리케이션은 반드시 하나의 메인 스레드를 가진다.
    - main() 메소드를 실행하면서 시작
    - 여러 개의 작업 스레드 생성 가능
- **싱글 스레드**
    - 스레드가 하나
    - 자바스크립트가 동시에 실행될 수 없는 이유
- **멀티 스레드 Multi Thread**
    - 애플리케이션 내부에서의 멀티 태스킹
- **워커 스레드 Worker Thread**
    - 노드 12 버전에서 안정화된 기능으로 이제 노드에서도 직접 멀티 스레드를 사용할 수 있다.
    - CPU 작업 (연산이 많은 작업) 이 많은 경우 사용

- 노드 프로세스는 멀티 스레드이지만 직접 다룰 수 있는 스레드는 하나이기 때문에 싱글 스레드라고 표현
- 노드는 멀티 스레드 대신 멀티 프로세스 활용

- **제일 중요!!**
    - 실행 컨텍스트
        - this
        - scope
    - 이벤트 루프
    - 프로토타입
