# TypeORM 시간 설정하기(KST vs UTC)

## 첫 번째 시도(실패).
- **entity에 `type: timestamp` 을 지정**
  - 해당 열이 추가된 시각을 자동으로 기록하는 옵션
  - 만일 MySQL의 time zone이 'UTC'라면 UTC 기준으로 출력하고 'Asia/Seoul'라면 서울 기준으로 출력
 
- **MySQL에서 타임존 별도 설정**
  - global time zone은 권한이 없어서 바꾸지 못했음
  - session time zone 변경만으로도 KST로 시간대 표시됨  
    <img width="816" alt="Untitled (1)" src="https://user-images.githubusercontent.com/109029407/212912768-8970d1a6-60d1-45ee-af60-792862d0e496.png">

- 참고문서: [mysql에서 9시간 차이날때(GCP)](https://velog.io/@taelee/mysql%EC%97%90%EC%84%9C-9%EC%8B%9C%EA%B0%84-%EC%B0%A8%EC%9D%B4%EB%82%A0%EB%95%8CGCP)    
    
- 성공했다고 박수치고 좋아했는데, **서버를 끄고 다시 켜니까 새로운 session이 시작되면서 'Asia/Seoul'에서 'UTC'로 돌아감**
  - 황당 그 자체(?) 
  - Tip: global time zone을 설정하면 계속 유지가 된다고 함
    <img width="816" alt="Untitled (2)" src="https://user-images.githubusercontent.com/109029407/212913856-90318c88-87b2-4c8a-80f0-a267f508a8b2.png">

## 두 번째 시도(성공).      
- **DB는 ‘UTC’ 기준으로 저장하고 typeorm.config에 ‘timezone’을 별도 설정**
  ```
  export const typeORMConfig: TypeOrmModuleOptions = {
    type: 'mysql',
    host: '비밀입니다',
    port: 3306,
    username: '이것도',
    password: '비밀입니다',
    database: 'pairy',
    timezone: '-09:00',
    entities: [Document, User, Client, Tag],
    synchronize: false,
    logging: true
  };
  ```
- trouble shooting🤬
  - 분명 MySQL에서도 타임존을 지원한다고 공식문서에 나와있는데 왜 안 되는 건지? 테스트를 20번을 했다
  - 그리고 테스트 결과 찾아낸 되는 케이스 단 하나는 ‘+00:00’. ‘-00:00’ 등 시간차를 직접 설정 하는 것
  - [같은 문제를 겪은 git issue](https://github.com/typeorm/typeorm/issues/5895)를 찾았는데 누군가가 내 심정을 대변함 => `실제 작동하지 않습니다. 나는 이것 때문에 3시간을 잃었다.`
       
    - *timezone: 'asia/seoul' ⇒ 작동X*
    - *timezone: 'Asia/Seoul' ⇒ 작동X*
    - *timezone: 'kst' ⇒ 작동X*
    - *timezone: 'KST' ⇒ 작동X*
    - ***timezone: '-09:00' ⇒ 작동O***
