# Javascript 객체 숫자 property의 자동 정렬
  
- 고객조회API와 문서목록조회API에 동일한 정렬 로직을 적용했는데, response에서 어떤 기준을 적용하든 문서목록조회API는 `documentId` 기준으로만 정렬되어 반환되었다. 
- **이유는 자바스크립트 객체의 특성때문이었다!** 
- **객체의 프로퍼티로 숫자를 넣게되면 자동으로 오름차순 순서로 반환되어 출력된다.**

  
    
## 결과값이 의도하지 않은 정렬이 되어 나오네!
- querybuilder로 가져온 raw data는 최신순인데 너는 왜 이렇게 정렬된거야? 😡
  
  ![Untitled](https://user-images.githubusercontent.com/109029407/213502960-97209464-45e0-42cf-8bb7-a4daf45d4290.png)
  
  
## 실험을 해보자~!
- 객체에 key와 value를 `1 → 4 → 2` 순서로 넣어보자. 
- 콘솔창에는 `1 → 2 → 4` 로 출력된다. 
- 혹시나싶어 string 형태의 숫자를 넣어도 동일한 결과가 나온다! (객체 프로퍼티는 숫자가 들어오면 문자열로 암묵적 타입 변환이 일어나기 때문에 차이가 없음)
  
- <객체 프로퍼티 숫자로 넣기>

  ![1](https://user-images.githubusercontent.com/109029407/213504210-bbd46f52-4874-4912-aa6f-7d3eba446be4.png)
  
- <객체 프로퍼티 string 형태의 숫자로 넣기>

  ![2](https://user-images.githubusercontent.com/109029407/213504344-12808436-2b24-4c93-afb0-9ce268eeba1c.png)

- <2가지 케이스가 동일하게 정렬되어 나오는 콘솔창>
  
  ![3](https://user-images.githubusercontent.com/109029407/213504539-2214536a-51c1-43fb-b096-a34cc9a4dda0.png)
  
## 그럼 어떻게 해결하지?
  
- **객체 프로퍼티가 숫자일 때는 숫자형이든 문자형이든 자바스크립트가 자동 변환하여 오름차순 정렬**을 해버리니까 프로퍼티에 숫자가 아닌 문자를 추가해주었더니 넣은 순서대로 값이 잘 나왔다!
- 프로퍼티 값은 response에서 보이지 않아서 아무렇게나 변형했지만, 만약 프로퍼티까지 보여준다면 방법을 고민해봐야 할 것 같다.
  
  
  ![1](https://user-images.githubusercontent.com/109029407/213505567-40824e92-0ee8-4ea7-986a-49ecd3314254.png)

  
     
- 비슷한 상황의 [참고문서](https://myung-ho.tistory.com/90)를 찾았는데, `Object.value()` 메소드와 상관없이 자바스크립트 객체의 특성인 듯하다
