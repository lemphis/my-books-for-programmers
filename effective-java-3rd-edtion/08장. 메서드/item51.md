# 51. 메서드 시그니처를 신중히 설계하라

다음은 배우기 쉽고, 쓰기 쉬우며, 오류 가능성이 적은 API를 설계하는 요령이다.

1. 메서드 이름을 신중히 지을 것
   - 항상 표준 명명 규칙을 사용해야 한다
   - 이해할 수 있고, 같은 패키지에 속한 다른 이름들과 일관되게 짓는 게 최우선
   - 개발자 커뮤니티에서 널리 받아들여지는 이름을 사용해야 한다
2. 편의 메서드를 너무 많이 만들지 말 것
   - 메서드가 너무 많은 클래스는 익히고, 사용하고, 문서화하고, 테스트하고, 유지보수하기 어려움
   - 메서드가 너무 많은 인터페이스는 이를 구현하는 사람고 사용하는 사람 모두를 고통스럽게 만듦
3. 매개변수 목록은 짧게 유지할 것
   - 4개 이하가 좋음 (4개가 넘어가면 매개변수를 전부 기억하기가 쉽지 않음)
   - 긴 매개변수 목록을 짧게 줄여주는 기술 세 가지
     1. 여러 메서드로 쪼갠다
     2. 매개변수 여러 개를 묶어주는 도우미 클래스 작성
     3. 위 두 기법을 혼합한 것으로, 객체 생성에 사용한 빌더 패턴을 메서드 호출에 응용
4. 매개변수 타입으로는 클래스보다 인터페이스를 사용할 것
   - 인터페이스 대신 클래스를 사용하면 클라이언트에게 특정 구현체만 사용하도록 제한하는 꼴임
   - 입력 데이터가 다른 형태로 존재한다면 명시한 특정 구현체의 객체로 옮겨 담느라 비싼 복사 비용을 치러야 함
5. boolean 보다는 원소 2개짜리 열거 타입을 사용할 것 (메서드 이름상 boolean을 받아야 의미가 명확한 경우 제외)
   - 코드를 읽고 쓰기가 더 쉬워짐
   - 나중에 선택지를 추가하기도 쉬움
