# `getOne()`과 `getRawOne()` 메소드의 차이점

- `getOne()`을 사용하면 TypeORM의 내부에서 반환된 결과를 ClientFile 클래스의 인스턴스로 변환하여 반환한다. 
- 다른 테이블과 join했을 경우 값을 가공하기가 쉽지 않다.

  ![Untitled](https://user-images.githubusercontent.com/109029407/215793759-be0008d8-2fd4-4469-b75e-1a5230bdab4f.png)
  ![Untitled](https://user-images.githubusercontent.com/109029407/215794044-79826041-24e9-410a-94a6-8360d8799d91.png)
  
- 인스턴스로 변환하지 않기 위해서는 `getRawOne()` 메소드를 사용할 수 있다. 
- raw data로 반환 시 boolean 값은 숫자형으로 변환된다.
  
  ![Untitled](https://user-images.githubusercontent.com/109029407/215794349-e56b5dda-dfee-4428-a0ba-494cccd77273.png)
  ![Untitled](https://user-images.githubusercontent.com/109029407/215794459-0c29147c-b58f-4b1f-8113-03a97ab41a7f.png)
  
- 결과값의 alias를 직접 명시해주면 프로퍼티명을 더 직관적으로 바꿀 수 있다.
- `getOne()`에서는 alias 지정 불가능

  ![Untitled](https://user-images.githubusercontent.com/109029407/215794812-8684c12a-5046-4e93-b2c6-d7783571cc0d.png)
  ![Untitled](https://user-images.githubusercontent.com/109029407/215794872-8f1143ec-1527-46ef-b80a-4bb59bfab620.png)
