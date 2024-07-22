## 12장 함수

- **함수의 정의 (155p)**
  - 일련의 과정을 문으로 구현하고 코드 블록으로 감싸서 하나의 실행 단위로 저장한 것
  ```jsx
  function add(x, y) => {
  	return x+y;
  }
  add(1,2); // 3

  /*
  add : 함수 이름
  (x,y): x,y는 매개 변수
  return x+y: 반환 값
  add(1,2) : 함수 호출 / 1,2는 인수
  const ~ {} : 함수 몸체
  */
  ```
  - 함수 사용 이유 : 코드 재 사용이 유용해서
  - **함수 선언문 (159p)**
    - function add(x, y) { …
    - 함수 이름 생략 불가능
    - 표현식이 아닌 **문**
    - 함수 객체를 가리키는 식별자로 호출한다 ex) add(1, 2)
  - **함수 표현식 (163p)**
    - let add = function (x, y) { …
    - 값의 성질을 갖는 일급 객체
  - **함수 생성 시점과 함수 호이스팅 (164p)**
    ```jsx
    // 함수 참조
    console.log(dir(add)); // f add(x,y)
    console.log(dir(sub)); // undefined
    // 함수 호출
    console.log(add(2, 5)); // 7
    console.log(sub(2, 5)); // TypeError: sub is not a function
    // 함수 선언문
    function add(x, y) {
      return x + y;
    }

    // 함수 표현식
    let sub = function (x, y) {
      return x - y;
    };
    ```
    - 함수 선언문과 함수 표현식으로 생성한 함수들은 생성 시점이 다르다
    - **호이스팅 : 함수 선언문이 코드의 선두로 끌어 올려진 것처럼 동작하는 js의 고유의 특징**
      - 변수 호이스팅과 미묘한 차이가 있다
      - 변수는 undefined로 초기화, 함수 선언문은 함수 객체로 초기화
      - 변수 할당문의 값은 런타임에 평가되므로, 함수 표현식의 함수 리터럴도 할당문이 실행되는 시점에서 평가되어 함수 객체가 된다
      - **함수 표현식으로 함수를 정의하면 함수 호이스팅이 아닌 변수 호이스팅이 발생**
  - **Function 생성자 함수 (166p)**
    - new 연산자와 함께 호출
    - new 없이 호출해도 결과는 동일
    ```jsx
    let add = new Function("x", "y", "return x + y");
    console.log(2, 5); // 7
    ```
  - **화살표 함수 (167p)**
    - ⇒ 를 사용해 더 간략한 방법으로 함수를 선언
    - 생성자 함수로 사용 불가, 항상 익명 함수로 정의
    - 기존 함수 선언문 or 함수 표현식을 표현만 간략하게 한 것이 아닌 내부 동작도 간략화 되어있다
    - 기존 함수와 this 바인딩 방식이 다르다
    - prototype 프로퍼티가 없다
    - arguments 객체를 생성하지 않는다 (더 자세한 것은 26.3장 에서)
    ```jsx
    const add = () => x + y;
    console.log(2, 5); // 7
    ```
  - **즉시 실행 함수(177p)**
    - 정의와 동시에 즉시 호출되는 함수
    - 단 한 번만 호출되며 재호출 불가
    - 익명 즉시 실행 함수를 사용하는 것이 일반적
    ```jsx
    // 익명 즉시 실행 함수
    (function () {
      let a = 3;
      let b = 5;
      return a * b;
    })();

    // 기명 즉시 실행 함수
    (function foo() {
      let a = 3;
      let b = 5;
      return a * b;
    })();
    ```
  - **재귀 함수 (179p)**
    - 함수가 자기 자신을 호출 하는 것
    ```jsx
    // ! : 팩토리얼 구현
    function factorial(n) {
      if (n <= 1) return 1;
      return n * factorial(n - 1);
    }
    ```
    - 탈출 조건이 필수, 없으면 무한 호출이 되어 스택 오버플로 에러가 발생한다
    - 대부분 for, while문으로 구현이 가능하다
  - **콜백 함수 (183p)**
    - 함수의 매개변수를 통해 다른 함수의 내부로 전달되는 함수
    - 매개변수를 통해 함수의 외부에서 콜백 함수를 전달 받은 함수를 고차 함수라 한다
    - 콜백 함수는 고차 함수에 의해 호출되며 이때 고차 함수는 필요에 따라 콜백 함수에 인수를 전달 가능
    ```jsx
    // 익명 함수 리터럴을 콜백 함수로 고차 함수에 전달한다
    // 익명 함수 리터럴은 repeat 함수를 호출할 때마다 평가되어 함수 객체를 생성한다
    repeat(5, function (i) {
      if (i % 2) console.log(i);
    }); // 1 3

    // logOdds 함수는 단 한번만 생성
    var logOdds = function (i) {
      if (i % 2) console.log(i);
    };

    // 고차 함수에 함수 참조를 전달
    repeat(5, logOdds); // 1 3
    ```
  - **순수 함수와 비 순수 함수**
    - 외부 상태 : 전역 변수, 서버 데이터, 파일, Console, DOM 등
    - 순수 함수 : 어떤 외부 상태에 의존하지 않고 변경하지도 않는(부수 효과가 없는)함수
      - 최소 하나 이상의 인수를 전달 받음
      - 인수의 불변성을 유지(인수 변경x)
      ```jsx
      let count = 0;

      // 순수 함수 increase는 동일한 인수 전달 시, 언제나 동일 값을 반환
      function increase(n) {
        return ++n;
      }

      // 순수 함수가 반환한 결과값을 변수에 재할당하여 상태 변경
      count = increase(count);
      console.log(count); // 1

      count = increase(count);
      console.log(count); // 2
      ```
    - 비 순수 함수 : 외부 상태에 의존하거나 외부 상태를 변경하는(부수 효과가 있는)함수
      - 함수의 외부 상태에 따라 반환 값이 달라짐
      - 함수의 외부 상태를 변경하는 부수 효과가 있다
    ```jsx
    let count = 0;

    // 비 순수 함수
    function increase() {
      return ++count; // 외부 상태에 의존하며 외부 상태를 변경
    }

    // 비 순수 함수는 외부 상태(count)를 변경하므로 상태 변화를 추적하기 어려워진다
    increase();
    console.log(count); // 1

    increase();
    console.log(count); // 2
    ```

## 13장 스코프

- **식별자가 유효한 범위**
  - 모든 식별자(변수 이름, 함수 이름, 클래스 이름 등)는 자신이 선언된 위치에 의해 다른 코드가 식별자 자신을 참조할 수 있는 유효 범위가 결정 됨
  - **식별자를 검색할 때 사용하는 규칙**이라고 할 수 있다
  - 스코프 개념이 **존재하는 이유?** → 같은 이름을 갖는 변수가 충돌을 일으키므로 프로그램 전체에서 하나밖에 사용할 수 없기 때문에
  ```jsx
  // 전역 스코프
  let x = "global";
  function foo() {
    // foo 함수 스코프
    let x = "local";
    console.log(x); // local
  }
  foo();
  console.log(x); // global
  ```
  - 스코프는 **네임 스페이스**
  - var를 사용하지 않는 이유? → var로 선언된 변수는 같은 스코프 내 중복 선언이 허용된다. 즉, 의도치 않게 변수 값이 재할당 되어 변경되는 부작용이 있다
  - let이나 const는 같은 스코프 내에서 중복 선언을 허용하지 않는다
  - 이름이 동일한 식별자이지만 스코프가 다른 별개의 변수로 인식되는 이유이다
- **렉시컬 환경**
  - 코드가 어디서 실행되며 주변에 어떤 코드가 있는지
- **스코프의 종류**

  - **전역 :** 코드의 가장 바깥 영역, 전역 스코프, 전역 변수, **어디서나 참조 가능**
  - **지역 :** 함수 몸체 내부, 지역 스코프, 지역 변수, **자신의 지역 + 하위 지역 스코프에서 유효**

  ```jsx
  // 전역 스코프
  var x = "global x";
  var y = "global y";

  function outer() {
    // 지역스코프
    var z = "outer local z";

    console.log(x); // global x
    console.log(y); // global y
    console.log(z); // outer local z

    function inner() {
      // 지역스코프
      var x = "inner local x";

      console.log(x); // inner local x
      console.log(y); // global y
      console.log(z); // outer local z
    }

    inner();
  }

  outer();

  console.log(x); // global x
  console.log(z); // ReferenceError : z is not defined
  ```

- **스코프 체인**
  - 함수의 중첩 : 함수 몸체 내부에서 함수가 정의된 것
    - 중첩 함수 : 함수 몸체 내부에서 정의한 함수 ex) inner()
    - 외부 함수 : 중첩 함수를 포함하는 함수 ex) outer()
  - 스코프 체인은 **스코프가 계층적으로 연결된 것**을 의미
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/8f3a5e44-ef14-4ca6-bdbe-b63b228a2d6d/01393536-06f4-4b00-a5dc-72861df57fcd/Untitled.png)
  - 스코프 체인은 물리적인 실체로 존재
    - js엔진은 코드 실행에 앞서 위의 그림과 유사한 자료구조인 렉시컬 환경을 실제로 생성
    - 변수 선언 실행 → 식별자가 렉시컬 환경에 key로 등록 → 변수 할당이 발생 → 자료구조의 변수 식별자에 해당하는 값을 변경
    - 변수의 검색도 이 자료구조 상에서 이뤄진다
    - 스코프 체인은 실행 컨텍스트의 렉시컬 환경을 단방향으로 연결한 것
    - 전역 레시컬 환경은 코드가 로드 시 생성, 함수 렉시컬 환경은 함수 호출 시 생성
- **스코프 체인에 의한 변수, 함수 검색**
  - **변수를 참조할 때 js엔진은 스코프 체인을 통해 변수를 참조하는 코드의 스코프에서 시작하여 상위 스코프 방향으로 이동하며 선언된 변수를 검색**
  - 상위 스코프의 변수는 하위 스코프도 참조가 가능하지만, 하위 스코프 변수는 상위 스코프에서 참조할 수 없다
  ```jsx
  // 전역함수
  function foo() {
    console.log("global function foo");
  }
  function bar() {
    // 중첩 함수
    function foo() {
      console.log("local function foo");
    }
    foo();
  }
  bar(); // local function foo
  foo(); //global function foo
  ```
- **함수 레벨 스코프**
  - **함수에 의해서만 지역 스코프가 생성된다**
  - **블록 레벨 스코프**
    - C, Java 등의 대부분의 언어는 함수 몸체만이 아닌 모든 코드 블록(if, for, while, try/catch 등)이 지역 스코프를 만드는 특성
  - **함수 레벨 스코프**
    - var 키워드로 선언된 변수는 오로지 함수의 코드 블록(함수 몸체)만을 지역 스코프로 인정하는 특성

```jsx
var x = 1;

if (true) {
  // var 키워드로 선언된 변수는 함수의 코드 블록만을 지역스코프로 인정
  // 함수 밖에서 var 키워드로 선언된 변수는 코드 블록 내에서 선언되었다 할지라도 모두 전역 변수
  // 즉, x는 전역 변수, 이미 선언된 변수x가 있으므로 x는 중복 선언이 된다
  // 이는 의도치 않게 변수 값이 변경되는 부작용을 발생시킨다(앞에서 나온 이야기)
  var x = 10;
}

console.log(x); // 10
```

- **렉시컬 스코프**
  ```jsx
  var x = 1;

  function foo() {
    var x = 10;
    bar();
  }

  function bar() {
    console.log(x);
  }

  foo(); // 1
  bar(); // 1
  // bar 함수는 전역 코드가 실행되기 전, 먼저 평가되어 함수 객체를 생성
  // 이때 생성된 bar함수 객체는 자신이 정의된, 전역 스코프를 기억
  // 때문에 어디서 호출이 되던지 간에 전역 스코프의 변수 x값을 호출하게 된다
  ```
  - **동적 스코프** : **함수를 어디서 호출 했는지**에 따라 함수의 상위 스코프를 결정
  - **렉시컬 스코프(정적 스코프) : 함수를 어디서 정의 했는지**에 따라 함수의 상위 스코프를 결정
  - j**s는 렉시컬 스코프를 따르므로** **함수를 어디서 정의 했는지에 따라 상위 스코프를 결정**
    - **함수가 호출된 위치는 상위 스코프 결정에 어떠한 영향을 주지 않는다**
    - 즉, **함수의 상위 스코프는 언제나 자신이 정의된 스코프**이다
    - 함수의 상위 스코프는 함수 정의가 실행될 때 정적으로 결정된다
    - 함수 정의가 실행되어 생성된 함수 객체는 이렇게 결정된 상위 스코프를 기억하는데, 함수가 호출될 때마다 함수의 상위 스코프를 참조할 필요가 있기 때문이다

## 14장 전역 변수의 문제점

- 변수의 생명 주기
  - 지역 변수 : 함수의 생명주기와 일치
  ```jsx
  function foo() {
    // 변수x의 생명 주기 ----
    // 변수 x 생성
    var x = "local"; // 변수 x에 값 할당
    console.log(x); // local
    return x;
    // 변수 x 소멸
    // 변수x의 생명 주기 ----
  }
  ```
  - 호이스팅은 스코프 단위로 동작
  ```jsx
  var x = "global";

  function foo() {
    console.log(x); // undefined
    var x = "local";
  }

  foo();
  console.log(x); // global
  ```
  - 전역 변수 : var 키워드로 선언한 전역 변수의 생명주기 = 전역 객체의 생명 주기
    - 명시적 호출 없이 실행(코드가 로드 되자마자 곧바로 해석, 실행)
    - **전역 객체 - 코드 실행 이전 단계에서 js엔진에 의해 먼저 생성되는 특수한 객체**
      - 클라이언트 사이드 환경(브라우저) - window
      - 서버 사이드 환경(Node.js) - global
      - 환경에 따라 전역 객체를 가리키는 다양한 식별자는 ES11에서 **globalThis**로 통일
      - 표준 빌트인 객체(Object, String, Number, Function, Array…)
        환경에 따른 호스트 객체(클라이언트 Web API 또는 Node.js의 호스트 API)
        var키워드로 선언한 전역 변수와 전역 함수를 프로퍼티로 받음
- **전역 변수의 문제점**
  - 암묵적 결합
    - 코드 어디서든 전역 변수를 참조하고 변경할 수 있는 것
    - 변수의 유효 범위가 크면 클수록 코드의 가독성이 나빠지고 의도치 않게 상태가 변경될 수 있는 위험성이 증가
  - 긴 생명 주기
    - 중복 가능성이 있어 의도치 않는 재할당이 이뤄질 수 있음
  - 스코프 체인 상에서 종점에 존재
    - 변수 검색 시 가장 마지막에 전역 변수가 검사가 되어 검색 속도가 가장 느리다
  - 네임 스페이스 오염
    - 다른 파일 내 동일한 이름으로 명명된 변수나 함수가 같은 스코프 내에 존재할 경우 예상 못한 결과가 나온다
- **전역 변수의 사용 억제 방법**
  - 즉시 실행 함수
    - 모든 코드를 즉시 시행 함수로 감싸면 모든 변수는 즉시 실행 함수의 지역 변수가 됨
    ```jsx
    (function foo() {
      var foo = 10;
      // ...
    })();

    console.log(foo); // ReferenceError : foo is not defined
    ```
  - 네임스페이스 객체
    - 전역에 네임스페이스 역할을 담당할 객체를 생성하고 전역 변수처럼 사용하고 싶은 변수를 프로퍼티로 추가하는 방법
    ```jsx
    var MYAPP = {}; // 전역 네임스페이스 객체

    MYAPP.name = "Lee";

    console.log(MYAPP.name); // Lee
    ```
    - 네임스페이스 객체에 또 다른 네임스페이스 객체를 프로퍼티로 추가해서 네임스페이스를 계층적으로 구성 가능
    ```jsx
    var MYAPP = {}; // 전역 네임스페이스 객체

    MYAPP.person = {
      name: "Lee",
      address: "Seoul",
    };

    console.log(MYAPP.person.name); // Lee
    ```
    - 그러나 그닥 유용하지는…
  - 모듈 패턴
    - 전역 변수 억제 및 캡슐화 까지 구현 가능
    ```jsx
    var Counter = (function () {
      // private 변수
      var num = 0;

      // 외부로 공개할 데이터나 메서드를 프로퍼티로 추가한 객체를 반환
      return {
        increase() {
          return ++num;
        },
        decrease() {
          return --num;
        },
      };
    })();

    // private 변수는 외부로 노출되지 않음
    console.log(Counter.num); // undefined

    cosole.log(Counter.increase()); // 1
    cosole.log(Counter.increase()); // 2
    cosole.log(Counter.decrease()); // 1
    cosole.log(Counter.decrease()); // 0
    ```
  - ES6모듈
    - 전역 변수 사용 불가
    - 대신 ES6 모듈은 파일 자체의 독자적인 모듈 스코프를 제공

## 15장 let, const 키워드와 블록 레벨 스코프

- **var 키워드로 선언한 변수의 문제점**
  - 변수 중복 선언 허용
  - 함수 레벨 스코프
    - 함수 외부에서 선언한 var 변수는 코드 블록 내에서 선언해도 모두 전역변수가 됨
  - 변수 호이스팅
    - 변수 선언문 이전에 참조 가능, 할당문 이전에 변수를 참조하면 언제나 undefined 반환
      ```jsx
      // 이 시점에는 변수 호이스팅에 의해 이미 foo 변수가 선언됨(1. 선언단계)
      // 변수 foo 는 undefined로 초기화(2. 초기화단계)
      console.log(foo); // undefined
      // 변수에 값 할당(3. 할당 단계)
      foo = 123;

      console.log(foo); // 123

      // 변수 선언은 런 타임 이전에 js엔진에 의해 암묵적으로 실행
      var foo;
      ```
      ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/8f3a5e44-ef14-4ca6-bdbe-b63b228a2d6d/09358b93-c4d3-4b5a-98de-6c37fa41ee41/Untitled.png)
- **let 키워드**
  - 변수 중복 선언 금지
  - 블록 레벨 스코프
  ```jsx
  let foo = 1; // 전역 변수
  {
  	let foo = 2; // 지역 변수
  	let var = 3; // 지역 변수
  }
  console.log(foo); // 1
  console.log(bar); // ReferenceError : bar is not defined
  ```
  - 변수 호이스팅
    - 변수 호이스팅이 발생하지 않는 것처럼 보인다
    - let은 선언 단계와 초기화 단계가 분리되어 진행된다
    - **일시적 사각지대(TDZ)** - 스코프의 시작 지점부터 초기화 지점 까지 변수를 참조할 수 없는 구간
    ```jsx
    // 런타임 이전에 선언 단계가 실행, 변수 초기화 x
    // 초기화 이전의 일시적 사각지대에서는 변수 참조 불가
    console.log(foo); // ReferenceError: foo is not defined
    let foo; // 변수 선언문에서 초기화 단계가 실행
    console.log(foo); // undefined

    foo = 1; // 할당문에서 할당 단계 실행
    console.log(foo); // 1
    ```
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/8f3a5e44-ef14-4ca6-bdbe-b63b228a2d6d/353f0347-cca2-439b-9965-ab2219efe276/Untitled.png)
  - 전역 객체와 let
    - var키워드로 선언한 전역 변수, 전역 함수, 선언하지 않은 변수에 값을 할당한 암묵적 전역은 전역 객체 window의 프로퍼티가 되며, 참조시 window 생략 가능
    ```jsx
    // 이 예제는 브라우저 환경에서 실행해야 한다.
    // 전역 변수
    var x = 1;
    // 암묵적 전역
    y = 2;
    // 전역 함수
    function foo() {}
    // var 키워드로 선언한 전역 변수는 전역 객처/ window의 프로퍼티다.
    console.log(window.x)； // 1
    // 전역 객처/ window의 프로퍼티는 전역 변수처럼 사용할 수 있다.
    console.logCx); // 1
    // 암묵적 전역은 전역 객체 window의 프로퍼티다.
    console.log(window.y); // 2
    console.log(y); // 2
    // 함수 선언문으로 정의한 전역 함수는 전역 객처/ window의 프로퍼티다.
    console.log(window.foo); // / foo() {}
    // 전역 객체 window으! 프로퍼티는 전역 변수처럼 사용할 수 있다.
    console.log(foo); // / foo() {}
    ```
    - let은 전역 객체의 프로퍼티가 아니므로 window.foo같이 접근이 불가
    - let 전역 변수는 보이지 않는 개념적인 블록(전역 렉시컬 환경의 선언적 환경 레코드)내에 존재
- **const 키워드**
  - 선언과 초기화 - const로 선언한 변수는 반드시 선언과 동시에 초기화 해야한다
  - 재할당 금지(상수)
  - const 키워드와 객체
    - const로 선언된 변수에 **원시 값을 할당 시 변경 불가**
    - const로 선언된 변수에 **객체를 할당한 경우 값 변경 가능**
    - 재할당을 금지할 뿐, “**불변”을 의미하지는 않음**
    ```jsx
    const person = {
    name： 'Lee'
    }；
    // 객체는 변경 가능한 값이다. 따라서 자/할당 없이 변경이 가능하다.
    person.name = 'Kim’;
    console.log(person); // {name： "Kim"}
    ```
- **var vs. let vs. const**
  - 기본적으로는 const를 사용, 원시 값과 객체에 사용
  - let은 재할당이 필요한 경우 한정해서 사용, 변수의 스코프는 최대한 좁게

## 16장 프로퍼티 어트리뷰트
