# java-lotto-precourse

# 로또

## 구현 목표

- 관련 함수들을 묶어 클래스를 만들고, 객체들이 협력하여 하나의 큰 기능을 수행하도록 한다.
- 클래스와 함수에 대한 단위 테스트를 통해 의도한 대로 정확하게 작동하는 영역을 확보한다.
- Enum을 적극 사용한다.

## 요구사항 정리

간단한 로또 발매기를 구현한다.

- 로또 번호의 숫자 범위는 1~45까지이다.
- 1개의 로또를 발행할 때 중복되지 않는 6개의 숫자를 뽑는다.
- 당첨 번호 추첨 시 중복되지 않는 숫자 6개와 보너스 번호 1개를 뽑는다.
- 당첨은 1등부터 5등까지 있다. 당첨 기준과 금액은 아래와 같다.
- 1등: 6개 번호 일치 / 2,000,000,000원
- 2등: 5개 번호 + 보너스 번호 일치 / 30,000,000원
- 3등: 5개 번호 일치 / 1,500,000원
- 4등: 4개 번호 일치 / 50,000원
- 5등: 3개 번호 일치 / 5,000원
- 로또 구입 금액을 입력하면 구입 금액에 해당하는 만큼 로또를 발행해야 한다.
- 로또 1장의 가격은 1,000원이다.
- 당첨 번호와 보너스 번호를 입력받는다.
- 사용자가 구매한 로또 번호와 당첨 번호를 비교하여 당첨 내역 및 수익률을 출력하고 로또 게임을 종료한다.
- 사용자가 잘못된 값을 입력할 경우 IllegalArgumentException을 발생시키고, "[ERROR]"로 시작하는 에러 메시지를 출력 후 그 부분부터 입력을 다시 받는다.
- Exception이 아닌 IllegalArgumentException, IllegalStateException 등과 같은 명확한 유형을 처리한다.

## 실행 예시

```
구입금액을 입력해 주세요.
8000

8개를 구매했습니다.
[8, 21, 23, 41, 42, 43]
[3, 5, 11, 16, 32, 38]
[7, 11, 16, 35, 36, 44]
[1, 8, 11, 31, 41, 42]
[13, 14, 16, 38, 42, 45]
[7, 11, 30, 40, 42, 43]
[2, 13, 22, 32, 38, 45]
[1, 3, 5, 14, 22, 45]

당첨 번호를 입력해 주세요.
1,2,3,4,5,6

보너스 번호를 입력해 주세요.
7

당첨 통계
---
3개 일치 (5,000원) - 1개
4개 일치 (50,000원) - 0개
5개 일치 (1,500,000원) - 0개
5개 일치, 보너스 볼 일치 (30,000,000원) - 0개
6개 일치 (2,000,000,000원) - 0개
총 수익률은 62.5%입니다.
```

## 구현할 기능 목록

- 입력기능
    - 구입금액 입력받는 기능
    - 당첨번호 입력받는 기능
    - 보너스번호 입력받는 기능

- 출력기능
    - 로또 목록 출력 기능
        - 구입한 로또는 오름차순으로 정렬하여 출력
    - 통계 출력 기능

- 핵심기능
    - 로또 관련 기능
        - 로또 생성 기능
            - 로또의 번호가 중복이면 예외 발생
            - 로또의 번호 개수가 6 이상이면 예외 발생
            - 로또 번호가 범위 내 (1 ~ 45)가 아니면 예외 발생
        - 돈으로 로또 사는 기능
            - 돈이 0 이상이 아니면 예외 발생
            - 돈이 1000원 단위가 아니면 예외 발생
        - 당첨 번호 저장 기능
            - 당첨 번호 개수가 6 이상이면 예외 발생
            - 보너스 번호 개수가 1 이상이면 예외 발생
            - 당첨 번호와 보너스 번호가 범위 내 (1 ~ 45)가 아니면 예외 발생
            - 당첨 번호와 보너스 번호가 중복이면 예외 발생
    - 당첨 통계 관련 기능
        - 몇 개 당첨 인지 계산 기능
        - 수익률 계산 기능
            - 소수점 둘째자리에서 반올림 할
            - 숫자 세자리마다 "," 표시
    - 문자열 분석 기능
        - ","로 구분된 문자열 받아서 정수 리스트로 반환하는 기능
            - 형식에 맞지 않으면 예외 발생
        - 문자열 받아서 정수로 반환하는 기능
            - 숫자 문자열이 오지 않으면 예외 발생

## 구현 방식

- MVC 패턴을 이용해 프로젝트 설계
    - Controller 파트에서는 서비스들을 이용해 실행흐름을 제어 역할
    - Service 파트에서는 주요 비즈니스 로직 관리 역할
    - View 파트에서는 사용자의 화면을 그리거나 입력받는 역할
        - Model에 데이터들을 담아 서비스와 컨트롤러 계층을 거쳐 View에 전달

- 예외처리 관련
    - BaseErrorCode를 구현한 ErrorStatus Enum에서 모든 예외처리 메세지를 관리
    - 각 예외 종류마다 **ErrorHandler를 생성하고 GeneralException을 상속받게 함.
    - Handler를 이용해 예외 던짐
    - 예외가 전달된 상위 계층에서는 try-cath 문을 사용해 사용자의 재 입력을 처리

- AppConfig
    - AppConfig에서 의존성을 관리
    - 생성자 주입을 통해 의존성 주입을 구현
- Controller
    - LottoGameController에서 나머지 세 컨트롤러들을 호출해 실행 흐름 제어
