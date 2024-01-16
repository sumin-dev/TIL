# 항해99 8기 5주차 <Node.js 심화> TIL_20220806 #
## 주특기 심화 주차 키워드 과제 ##
### Q1) Class는 대체로 추상화를 위해 사용됩니다. ES5 자바스크립트에서 Class는 어떻게 동작할까요? ###
#### 클래스와 인스턴스의 개념 이해 ####
- **클래스**: 인스턴스의 공통 속성을 모은 추상적인 개념
  - 과일은 음식이라는 집단에 포함. 따라서 과일 클래스는 음식 클래스의 하위 클래스.

- **인스턴스**: 클래스의 속성을 지니는 실존하는 객체
  - 자몽, 사과, 귤 등은 과일이라는 집단에 속함. 여기서 과일은 ‘클래스’이고 자몽, 사과 귤 등은 클래스에 속하는 인스턴스들.
과일, 음식은 추상적인 개념이고 자몽, 사과, 귤 등은 눈으로 볼 수도 있고 만질 수도 있고 먹을 수도 있는 구체적인 물체.

- 현실에서는 인스턴스로 부터 공통점을 찾아 클래스를 정의되는 것과 달리 **프로그래밍에서는 클래스가 먼저 정의되어야 공통적인 요소를 지니는 인스턴스를 생성**할 수 있음.
![인스턴스](https://user-images.githubusercontent.com/109029407/183239604-a177152f-33c7-4cbd-a56c-940071962025.png)
---
#### 붕어빵 만들기로 클래스 접근하기 ####
- 먼저 object형식으로 붕어빵 하나를  만들어보자
```
var potBung = {
   base : '밀가루';
   flavor : '팥';
}
```

- 이 같은 형식으로 다양한 맛의 '새로운 붕어빵 object'를 만들어 볼까?
```
var potBung = {
   base : '밀가루';
   flavor : '팥';
}

var shooBung = {
   base : '쌀가루';
   flavor : '슈크림';
}

var pizzaBung = {
   base : '튀김가루';
   flavor : '피자';
}
```
- 여기서 potBung라는 객체에서 flavor를 선택하면 '팥'이 나온다. shooBung라는 객체에서 flavor를 선택하면 '슈크림'이 나간다. 
이 같은 방식으로라면 데이터가 많아지면 많아지는 대로 재료를 일일이 적어줘야한다. (=하드코딩) **이렇게 비슷한 object를 많이 만들어야할 때, 
class문법을 사용**하면 수고로움을 덜 수 있다.
- class라는 멋진 붕어빵 틀을 만들면 '밀가루와 팥'을 넣든, '쌀가루와 슈크림'을 넣든 붕어빵 모양으로 나온다. **class는 object를 뽑아쓰는 틀!**
<img width="349" alt="붕어빵" src="https://user-images.githubusercontent.com/109029407/183240119-50c1bd5f-bf4e-4bd2-b1f3-1c36b7addf10.png">

---

#### 클래스로 붕어빵 틀 만들기 ####

:: ES5
```
function FishBunFrame(){
  this.base = "밀가루";
  this.flavor = "팥";
}
```

- class 생성자 함수는 일반함수와의 구분을 위해 보통 대문자로 시작
- this가 존재함으로해서 function은 class역할을 해줄 수 있음
- 여기서 this = 기계로 부터 새로 생성되는 object들. 즉 생성자 함수 자신. (= instance=인스턴스)


```
this.x = "밀가루";
// this.x = 새로 생성되는 object에 {x = '밀가루'} 를 추가해 줘
```

- 어떤 주문이 들어오든 같은 모양의 붕어빵을 뽑아내는 FishBunFrame가 만들어 졌다.

- fishBunFrame를 통해 새로운 obect를 생성하고 싶을 때 주문법은 아래와 같다.
```
/* instance 생성 */
new FishBunFrame();
```

- new를 통해 this에 저장된 내용물을 객체에 적용한다.

 
- 다양한 변수에 새로운 객체를 할당 해보자.

```
var potBung = new FishBunFrame();
console.log(potBung);    // FishBunFrame {base: "밀가루", flavor: "팥"}

var chouxBung = new FishBunFrame();
console.log(chouxBung)   // FishBunFrame {base: "밀가루", flavor: "팥"}

```

- new 붕어빵 (object)가 만들어 졌지만, 재료가 전부 같다.
 
 
- 여기서 new 붕어빵(object)에 각기 다른 재료 (value)를 넣고 싶다면?

```
function FishBunFrame(a, b){
  this.base = a;
  this.flavor = b;
}
```


- FishBunFrame함수에 parameter를 사용하여 값을 받는다.
- parameter가 a, b 값을 받으면 원하는 value를 넣어 생성할 수 있다.

``` 
var potBung = new FishBunFrame('밀가루', '팥')
console.log(potBung)      // fishBunFrame {base: "밀가루", flavor: "팥"}

var chouxBung = new FishBunFrame('쌀가루', '슈크림')
console.log(chouxBung)    // fishBunFrame {base: "쌀가루", flavor: "슈크림"}
```


- 참고 : https://sambalim.tistory.com/157
- 참고 : https://kangdanne.tistory.com/93
