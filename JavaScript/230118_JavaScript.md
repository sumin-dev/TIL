# Javascript time format 변환하기
- 조회 API를 만드는 중, response에 `created_at` 칼럼이 추가되었고, 정해진 시간 형식이 있었다! => `2023-01-15T13:10:38`
- 우선 DB에 저장되는 데이터의 형태(UTC 기준으로 저장하고 가져올 때 typeORM으로 시간차 보완)
  ![ㅇㅇㅇㅇㅇ](https://user-images.githubusercontent.com/109029407/213197815-f5d62219-936d-451a-9ba1-cfe0f0da70f1.jpg)


## 첫 번째 시도(실패).
- **TypeORM을 이용해 DB에 저장되는 포맷을 바꾸기**
  
  - entity에 `@transform` 데코레이터를 이용해 포맷바꾸기(실패) 
  - column의 type option에 `timestamp without time zone`, `timestamptz` 등 설정하기(실패)
    - Z로 표기되는 줄루시간을 없애고 싶어 이것저것 시도해보았지만, MySQL이 `timestamp` 외에는 옵션 지원하지 않음


  - **무엇보다 DB 저장 포맷 자체를 바꾸려고 하는 것은 좋지 못한 방법이었다.** 
    - **첫번째 이유**) `Date` 형식으로 저장되고 있는데, 포맷을 바꾸려면 String 타입으로 변환해야하고 그렇게되면 Javascript에서 기본적으로 제공하는 시간 관련 기능들을 이용하지 못함
    - **두번째 이유**) 만약 이후에 response 케이스가 추가되어 time format을 또 바꿔야한다면 매우 비효율적인 상황이 됨
    - **결론**) DB에는 raw data 그대로 저장하고 상황에 맞게 가공해서 쓰자!  


## 두 번째 시도(성공).
- **코드 단에서 time format 가공하기**
  - `created_at: c.t_created_at.toString().split('.')[0]`(실패) 
    <img width="657" alt="실패리스폰스" src="https://user-images.githubusercontent.com/109029407/213247399-2ee5e465-438b-4892-933f-2482acc5cb00.png">

  - `created_at: c.t_created_at.toISOString().split('.')[0]`(성공) 
    <img width="494" alt="성공리스폰스" src="https://user-images.githubusercontent.com/109029407/213247424-e75f421e-de9d-4e8d-96df-e82ee7665b5f.png">

- **`toString()` 과  `toISOString()` 은 어떤 차이가 있길래? 🧐**
  
    - `toString()` 을 쓰면서 British Summer Time 형식으로 변환되었고 `split('.')` 메소드에 영향을 받지 않았음(time format에 `.`이 없음)
    - `toISOString()` 을 쓰면 ISO시간 형식의 문자열로 변환되고 `split('.')` 메소드로 줄루시간 제거 가능
    
- **javascript `Date` 객체에는 기본적으로 제공하는 7개의 날짜 지정 형식이 있음.**
          
  - [JavaScript에서 코드 한 줄로 날짜 형식 지정하기](https://www.freecodecamp.org/korean/news/how-to-format-dates-in-javascript/)
      - `toString()` - **Fri Jul 02 2021 14:03:54 GMT+0100 (British Summer Time)**을 반환합니다
      - `toDateString()` - **Fri Jul 02 2021**을 반환합니다
      - `toLocaleString()` - **7/2/2021, 2:05:07 PM**을 반환합니다
      - `toLocaleDateString()` - **7/2/2021**을 반환합니다
      - `toGMTString()` - **Fri, 02 Jul 2021 13:06:02 GMT**을 반환합니다
      - `toUTCString()` - **Fri, 02 Jul 2021 13:06:28 GMT**을 반환합니다
      - `toISOString()` - **2021-07-02T13:06:53.422Z**을 반환합니다
