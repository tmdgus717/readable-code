# Readable Code

<img src="img.png" width="300" alt="이미지 설명">

[Readable Code: 읽기 좋은 코드를 작성하는 사고법](https://www.inflearn.com/course/readable-code-%EC%9D%BD%EA%B8%B0%EC%A2%8B%EC%9D%80%EC%BD%94%EB%93%9C-%EC%9E%91%EC%84%B1%EC%82%AC%EA%B3%A0%EB%B2%95/dashboard)

## 도메인?
도메인 : 해결하고자 하는 문제 영역

도메인 지식 : 도메인을 이해하고 해결하는데 필요한 지식

## 추상
코드를 잘 짠다? <span style= "color:red">읽기가 좋다~ </span>

**클린 코드**의 본질은 : 가독성

### 추상과 구체
추상 : 중요한 정보는 **가려내어** 남기고, 덜 중요한 정보는 생략하여 **버린다**

**적절한 추상화**는 복잡한 데이터와 복잡한 로직을 단순화하여 이해하기 쉽도록 돕는다 -> 이해하기 쉽다

**잘못된 추상화**는 구체를 유추하지 못한다
1. 추상화 과정에서 중요한 정보를 부각시키지 못함
2. 해석자가 동일하게 공유하는 문맥이 없다

정리 : 

'적잘한 추상화'는 해당 도메인의 문맥 안에서, 중요한 핵심 개념만 남겨서 표현하는 것

### 이름 짓기
추상화의 대표적인 행위!! 이름 짓기

1. **단수, 복수 표현하기**
2. **이름 줄이지 않기**
   - 줄임말 : 가독성이 나빠진다
   - 관용적으로 사용하는 줄임말 정도는 사용해도 괜찮다
   - Ex) column -> col, latitude -> lat, longitude -> lon
3. **은어/방언 사용하지 않기**
   - 일부만 이해하는 용어 금지X (새로운 팀원을 위하여)
   - 도메인 용어 사용하기 (도메인 : 해결하고자 하는 문제 영역)
4. 좋은 코드를 보고 습득하기
   - 좋은 프레임워크, 라이브러리들의 좋은 설계나 코드를 보고 학습하자!
   - pool(모여있는 것), candidate(후보들), threshold(한계점)

### 코드 분석 (레거시)
MinesweeperGame(지뢰찾기 게임)
 
```java
private static String[][] board = new String[8][10]; //게임판 정보
private static Integer[][] landMineCounts = new Integer[8][10];//지뢰 숫자
private static boolean[][] landMines = new boolean[8][10]; //지뢰 유무
private static int gameStatus = 0; // 0: 게임 중, 1: 승리, -1: 패배
```

- 보드 초기화
```java
        for (int i = 0; i < 8; i++) {
            for (int j = 0; j < 10; j++) {
                board[i][j] = "□";
            }
        }
```

- 지뢰 생성 (10개)
```java
        for (int i = 0; i < 10; i++) {
            int col = new Random().nextInt(10);
            int row = new Random().nextInt(8);
            landMines[row][col] = true;
        }
```

- 주변 지뢰 갯수 설정
```java
        for (int row = 0; row < 8; row++) {
            for (int col = 0; col < 10; col++) {
                int count = 0;
                if (!landMines[row][col]) { //지뢰가 아니라면
                    if (row - 1 >= 0 && col - 1 >= 0 && landMines[row - 1][col - 1]) {
                        count++;
                    }
                    if (row - 1 >= 0 && landMines[row - 1][col]) {
                        count++;
                    }
                    if (row - 1 >= 0 && col + 1 < 10 && landMines[row - 1][col + 1]) {
                        count++;
                    }
                    if (col - 1 >= 0 && landMines[row][col - 1]) {
                        count++;
                    }
                    if (col + 1 < 10 && landMines[row][col + 1]) {
                        count++;
                    }
                    if (row + 1 < 8 && col - 1 >= 0 && landMines[row + 1][col - 1]) {
                        count++;
                    }
                    if (row + 1 < 8 && landMines[row + 1][col]) {
                        count++;
                    }
                    if (row + 1 < 8 && col + 1 < 10 && landMines[row + 1][col + 1]) {
                        count++;
                    }
                    landMineCounts[row][col] = count;
                    continue;
                }
                landMineCounts[row][col] = 0; //지뢰라면 카운트가 필요없으므로 0
            }
        }
```

- 유저 입력값을 row,col 값으로 변경
```java
            System.out.println();
            System.out.println("선택할 좌표를 입력하세요. (예: a1)");
            String cellInput = scanner.nextLine();
            System.out.println("선택한 셀에 대한 행위를 선택하세요. (1: 오픈, 2: 깃발 꽂기)");
            String userActionInput = scanner.nextLine();
            char cellInputCol = cellInput.charAt(0);
            char cellInputRow = cellInput.charAt(1);
            int col;
            switch (cellInputCol) {
                case 'a':
                    col = 0;
                    break;
                case 'b':
                    col = 1;
                    break;
                case 'c':
                    col = 2;
                    break;
                case 'd':
                    col = 3;
                    break;
                case 'e':
                    col = 4;
                    break;
                case 'f':
                    col = 5;
                    break;
                case 'g':
                    col = 6;
                    break;
                case 'h':
                    col = 7;
                    break;
                case 'i':
                    col = 8;
                    break;
                case 'j':
                    col = 9;
                    break;
                default:
                    col = -1;
                    break;
            }
            int row = Character.getNumericValue(cellInputRow) - 1; //0부터 시작하는 값으로 변경
```

- 2번 깃발 선택
```java
          if (userActionInput.equals("2")) {
                board[selecterRowIndex][selectedColIndex] = "⚑";
                boolean isAllOpened = true;
                for (int row = 0; row < 8; row++) {
                    for (int col = 0; col < 10; col++) {
                        if (board[row][col].equals("□")) {
                            isAllOpened = false;
                        }
                    }
                }
                if (isAllOpened) {
                    gameStatus = 1;
                }
```

- 1번 셀 열기 선택
```java
 else if (userActionInput.equals("1")) {
                if (landMines[selecterRowIndex][selectedColIndex]) { //지뢰셀이 true면
                    board[selecterRowIndex][selectedColIndex] = "☼";
                    gameStatus = -1;
                    continue;
                } else {
                    open(selecterRowIndex, selectedColIndex);
                }
                boolean isAllOpened = true;
                for (int row = 0; row < 8; row++) {
                    for (int col = 0; col < 10; col++) {
                        if (board[row][col].equals("□")) {
                            isAllOpened = false;
                        }
                    }
                }
                if (isAllOpened) {
                    gameStatus = 1;
                }
            } else {
                System.out.println("잘못된 번호를 선택하셨습니다.");
            }
        }
```

- open 메서드
```java
    private static void open(int row, int col) {
        if (row < 0 || row >= 8 || col < 0 || col >= 10) { //보드 칸 벗어나면 패스
            return;
        }
        if (!board[row][col].equals("□")) { //이미 열렸으면 패스
            return;
        }
        if (landMines[row][col]) { //지뢰칸이면 패스
            return;
        }
        if (landMineCounts[row][col] != 0) { //숫자셀이라면 숫자표시
            board[row][col] = String.valueOf(landMineCounts[row][col]);
            return;
        } else { //아무셀도 아니라면 열림표시
            board[row][col] = "■";
        }
        //자기 주변의 셀을 돈다
        open(row - 1, col - 1);
        open(row - 1, col);
        open(row - 1, col + 1);
        open(row, col - 1);
        open(row, col + 1);
        open(row + 1, col - 1);
        open(row + 1, col);
        open(row + 1, col + 1);
    }

```

### 메서드와 추상화

<span style="color:blue">잘 쓰여진 코드라면, 한 메서드의 주제(기능)는 반드시 하나!</span>

메서드의 이름으로 구체적인 내용을 추상화 + 의미를 담을 수 있는 메서드로 쪼개기!!

```
void 산책하면서_돈쓰기(){
   Money 은행에서_현금_인출();
   Book 서점에서_책_구입하기();
}
```

### 메서드 선언부 ⭐
반환타입 메서드명 (파라미터) : **메서드 선언부**

메서드명 (파라미터) : 메서드 시그니처

메서드 선언부
   - 추상화된 구체를 유추할 수 있는, 적절한 의미가 담긴 이름
   - 파라미터와 연결지어 더 풍부한 의미를 전달

파라미터
   - **파라미터의 타입, 개수, 순서를 통해 의미를 전달**
   - 파라미터는 외부 세계와 소통하는 창

반환타입
   - 메서드 시그니처에 납득이 가는, 적절한 타입의 반환값 돌려주기
   - void 대신 반환할 만한 값이 있는지 고민
     - 반환값을 통해 테스트 용이
   
```java
//cellInputCol로부터 Col을 변환할꺼야
int selectedColIndex = convertColFrom(cellInputCol);
```

### 추상화 레벨 ⭐
메서드로 추출 -> 경계가 발생한다 (마치 책 제목을 본 것)

외부 세계 -> 추상화 레벨이 높다

내부 세계 -> 추상화 레벨이 낮다

```java
showBoard();
if (gameStatus == 1) { // 주변과 추상화 레벨이 다르다
    System.out.println("지뢰를 모두 찾았습니다. GAME CLEAR!");
    break;
}
```

변경
```java
if (doesUserWinTheGame()) {
    System.out.println("지뢰를 모두 찾았습니다. GAME CLEAR!");
    break;
}
```

### 매직 넘버, 매직 스트링
상수로 추출한다 -> 이름을 부여한다!! (추상화 행위)

의미를 갖고 있으나, 상수로 추출되지 않은 숫자, 문자열

상수 추출로 이름을 부여함으로써 <span style= "color:red">**가독성, 유지보수성**</span> 향상

---
## 논리, 사고의 흐름

### Early return
else 문을 사용하면 전체 로직을 **기억해야** 코드를 이해할 수 있다

메서드로 추출 후 if 문 실행 시 **return** 을 활용하여 실행을 종료해 버리면 그 분기만 이해하면 된다

<span style= "color:lime">결론 : Early return으로 else의 사용을 지양</span>

### 사고의 depth 줄이기
- 중첩 분기문, 중첩 반목문
- 사용할 변수는 가깝게 선언하기

이중 for문을 method로 추출하여 표현하는 것이 이해하는데 도움이 된다면 추출

스스로 판단하여 코드를 작성

### 공백 라인
공백 라인도 의미를 가진다
- 복잡한 로직을 의미 단위로 나누자!

### 부정어
부정연산자는 사고를 한번 더 해야하므로 비효율적일 수 있다

1. 부정어구를 쓰지 않아도 되는 상황인지 체크
2. 부정의 의미를 담은 다른 단어가 존재하는지
3. 부정어구로 메서드명 구성

ex) <span style= "color:red">!</span>왼쪽일때() -> **오른쪽일때()** || 왼쪽이아닐때()

### 해피 케이스 + 예외 처리

- **검증이 필요한 부분은 주로 클라이언트의 데이터 (외부 세계와의 접점)**
  - **사용자 입력**, 객체 생성자, 외부 서버의 요청 ...
- <span style= "color:lime">의도한 예외와 예상하지 못한 예외를 구분하기</span>
  - 사용자에게 보여줄 예외와, 개발자가 처리할 예외 구분!!!

⭐ Null을 대하는 자세
- NullPointException을 방지하는 방향으로 설계하기
- 메서드 설계 시 return null 자제 -> Optional을 사용

⭐ Optional
- 꼭 필요한 상황에서 반환 타입에 사용
- Optional을 파라미터로 받지 않는다
  - 분기 케이스가 3개나 된다 
  - Optional이 가진 데이터가 null인지 아닌지 + Optional 그 자체가 null 인지
- Optional을 반환받았다면 최대한 빠르게 해소한다

Optional을 해소하는 방법
- ifPresent()-get() 대신 API 사용
  - ex)orElseGet(), orElseThrow(), ifPresent(), ifPresentOrElse()
  - ex) orElseGet(), orElseGet(), orElseThrow() 의 차이
    - orElse() -> 항상 실행
    - orElseGet() -> null인 경우 실행

```java
    public T orElse(T other) {
        return value != null ? value : other;
    }
    
    public T orElseGet(Supplier<? extends T> supplier) {
        return value != null ? value : supplier.get();
    }

    public T orElseThrow() {
        if (value == null) {
            throw new NoSuchElementException("No value present");
        }
        return value;
    }
```

