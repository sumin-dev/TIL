# 항해99 8기 5주차 <Node.js 심화> TIL_20220808 #
## 주특기 심화 주차 키워드 과제 ##
### Q2) Class는 var 변수처럼 호이스팅이 일어날까요? ###
#### 호이스팅 hoisting 개념 이해 ####
- 호이스팅은 코드를 실행하기 전 변수선언/함수선언을 해당 스코프의 최상단으로 **끌어 올려지는 현상
(할당이 끌어올려지는 것이 아니다)**

  - 자바스크립트 엔진은 코드를 실행하기 전 실행 가능한 코드를 형상화하고 구분하는 과정(실행 컨텍스트 과정)에서
모든 선언(var, let, const, function, class)을 스코프에 등록
  - 자바스크립트의 모든 선언에는 호이스팅이 일어나지만, let/const/class를 이용한 선언문은 호이스팅이 발생하지 않는 것 처럼 동작
(참조에러가 발생 ReferenceError)
  - 이는 스코프의 시작지점에서 초기화 시작지점까지 **일시적 사각지대(Temporal Dead Zone, TDZ)** 에 빠지기 때문
  - **var 키워드는 선언과 함께 동시에 undefined로 초기화**되어 메모리에 저장되는데, **나머지는 선언단계와 초기화단계가 분리되어 진행**된다. 
  즉, 스코프에 변수를 등록하지만 초기화 단계는 변수 선언문에 도달했을 때(코드 실행 후) 이뤄지므로 초기화되지 않은 상태로
선언만 메모리에 저장. 초기화되지 않으면 변수를 참조할 수 없다.
  - var로 선언된 대상은 undefined로 초기화/ **let이나 const, class와 같은 여타 호이스팅 대상들은 undefined가 아니라 uninitialized로 초기화**. undefined는 Javascript의 Primitive Type중에 하나이고, 실질적인 '값'으로써 인식하기 때문에 에러가 발생하지 않는다. 반면에, <uninitialized>로 선언된 변수/함수는 초기화되지 않았다는 에러를 발생하며 이와같이 <uninitialized>로 Lexical Environment의 값(variable)이 정의된 때를 Temporal Dead Zone이라고 한다.
  - 결국 호이스팅이 일어나지 않은게 아니라, 호이스팅되었기 때문에 참조에러가 발생하는 것.

  ```
  var foo = new Foo(1, 2); //ReferenceError
  class Foo {
   constructor(x, y) {
      this.x = x;
      this.y = y;
   }
  }
  
  
  console.log(str); // undefined
  var str = "hello";
