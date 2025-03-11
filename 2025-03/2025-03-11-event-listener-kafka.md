## 테스트 컨테이너 블로그 포스팅

[블로그](https://velog.io/@wda067/Docker-Testcontainers%EB%A1%9C-%ED%85%8C%EC%8A%A4%ED%8A%B8-%ED%99%98%EA%B2%BD-%EA%B5%AC%EC%B6%95%ED%95%98%EA%B8%B0)

## 이벤트 리스너 vs. 카프카 

쿠폰 사용 알림 -> 스프링 이벤트 리스너 vs. 카프카
- 이벤트 리스너
  - 간단한 구현
  - 낮은 트래픽에 적합
- 카프카
  - 확장성
  - 장애 시 재처리 가능
  - 이벤트 활용 가능
  - 초기 설정 비용

이미 기존에 쿠폰 발급 로직에서 카프카를 사용하고 있는데 쿠폰 발급과 다르게 쿠폰 사용은 높은 트래픽이 발생하지 않을거 같고, 사용 알림 외 다른 요구사항은 없는데 카프카는 오버 엔지니어링일까?

***

[#kafka](https://github.com/wda067/TIL/search?q=%23kafka&type=code) 
[#spring](https://github.com/wda067/TIL/search?q=%23spring&type=code)