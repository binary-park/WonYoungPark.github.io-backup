# Mrblue Java Style Guide

|     일자     | 작성자  |  비고  |
| :--------: | :--: | :--: |
| 2018.01.10 | 박원영  | 최초작성 |
| 2018.01.17 | 박원영  |  수정  |
| 2018.01.18 | 박원영  |  수정  |
| 2018.01.25 | 박원영  |  수정  |



## 1. 소개

이 문서는 Java ™ 프로그래밍 언어의 소스 코드에 대한 Mrblue의 코딩 표준을 정의한 것입니다. Java 소스 파일은 여기에있는 규칙을 준수하는 경우에만 *Mrblue 스타일*이라 할 수 있습니다.

다른 프로그래밍 스타일 가이드와 마찬가지로 해당 코딩 표준은 코드의 가독성 뿐만 아니라 일관된 코드 스타일을 통해 코드의 **품질 상향과 코드의 유지보수 비용을 절감하고 업무 효율**을 높이는데에 목적 의미를 가지고 있습니다.



### 1.1 전문 용어 참고 사항

이 문서에서는 달리 명확히하지 않는 한 :

1. *클래스* 라는 용어 는 "일반"클래스, enum 클래스, 인터페이스 또는 주석 유형 ( `@interface`) 을 의미하기 위해 포괄적으로 사용됩니다 .
2. *멤버* (클래스) 라는 용어 는 중첩 된 클래스, 필드, 메서드 *또는 생성자* 를 의미하는 데 포괄적으로 사용됩니다 . 즉 초기화 자 및 주석을 제외한 클래스의 모든 최상위 내용
3. *주석* 이라는 용어는 항상 *구현* 주석을 나타냅니다 . 공통적인 용어 "Javadoc" 대신 "documentation comments"라는 구를 사용하지 않습니다.

다른 "용어 노트"가 문서 전체에서 간혹 나타납니다.



### 1.2 안내 사항

이 문서의 내용은 Google의 코딩 표준을 기준으로 작성된 Mrblue의 코딩 표준입니다.



## 2 소스 파일 기본 사항

### 2.1 파일 이름

소스 파일 이름은 포함하고있는 최상위 클래스의 대소 문자를 구분하는 이름과`.java`확장자로 구성됩니다.



### 2.2 파일 인코딩 : UTF-8

파일 인코딩은 **UTF-8** 로 지정합니다.



### 2.3 특수 문자

#### 2.3.1 공백 문자

줄 종결 자 시퀀스와는 별도로 **ASCII 수평 공백 문자** ( **0x20** )는 소스 파일의 아무 곳에 나 나타나는 유일한 공백 문자입니다. 이는 다음을 의미합니다.

1. 문자열 및 문자 리터럴의 다른 모든 공백 문자는 이스케이프 처리됩니다.
2. 탭 문자는 들여 쓰기에 사용 **하지 않습니다** .

#### 2.3.2 특수 이스케이프 시퀀스

있는 모든 문자를 [이스케이프 시퀀스](http://docs.oracle.com/javase/tutorial/java/data/characters.html) ( `\b`, `\t`, `\n`, `\f`, `\r`, `\"`, `\'`과 `\\`), 그 순서는 해당 진수 (예를 들면보다는 사용  `\012`) 또는 유니 코드 (예를 들어  `\u000a`) 탈출.

#### 2.3.3 비 ASCII 문자

코드 가독성이 좋다면 비 ASCII문자(\u221e)를 사용해도 무방하다.

나머지 비 ASCII 문자의 경우, 실제 유니 코드 문자 (예 :) `∞`또는 동등한 유니 코드 이스케이프 문자 (예  :)  가 사용됩니다. 유니 코드는 문자열 리터럴 밖에서 이스케이프 처리되고 주석은 강력하게 권장되지 않지만 코드를 **더 쉽게 읽고 이해할 수** 있게하는 선택에 따라 다릅니다 .`\u221e`****

**팁 :** 유니 코드 이스케이프의 경우, 때로는 실제 유니 코드 문자가 사용되는 경우에도 설명 주석이 매우 유용 할 수 있습니다.

예 :

| 예                                        | 토론                                       |
| ---------------------------------------- | ---------------------------------------- |
| `String unitAbbrev = "μs";`              | 최고 : 주석 없이도 명확하다.                        |
| `String unitAbbrev = "\u03bcs"; // "μs"` | 허용: 수행 할 이유가 없다.                         |
| `String unitAbbrev = "\u03bcs"; // Greek letter mu, "s"` | 허용: 어색하고 실수하기 쉽다.                        |
| `String unitAbbrev = "\u03bcs";`         | 안좋음: 개발자가 이해할 수 없다.                      |
| `return '\ufeff' + content; // byte order mark` | 좋음 : 인쇄 할 수없는 문자에 대해서는 이스케이프를 사용하고, 필요한 경우 주석으로 처리해야 한다. |

**팁 :** 일부 프로그램이 비 ASCII 문자를 제대로 처리하지 못할 수 있다는 두려움에서 코드를 쉽게 읽을 수 없게 만들지 마십시오. 그런 일이 일어나면 그 프로그램은 **고장 났고** 반드시 **수정** 해야합니다 .



## 3 소스 파일 구조

소스 파일은 다음 **순서** 로 구성됩니다 .

1. 라이센스 또는 저작권 정보 (있을 경우)
2. package 문
3. import 문
4. 정확히 하나의 최상위 클래스

**정확히 하나의 공백 행** 은 존재하는 각 섹션을 구분합니다.



### 3.1 라이센스 또는 저작권 정보 (있는 경우)

라이센스 또는 저작권 정보가 파일에 속하면 여기에 속합니다.



### 3.2 package 문

package 문은 **줄 바꿈되지 않습니다** . 열 제한 (4.4 절, [열 제한 : 100](https://google.github.io/styleguide/javaguide.html#s4.4-column-limit) )은 패키지 문에 적용되지 않습니다.



### 3.3 import 문

#### 3.3.1 줄 바꾸기 없음

import 문은 **줄 바꿈하지 않습니다** . 열 제한 (4.4 절, [열 제한 : 100](https://google.github.io/styleguide/javaguide.html#s4.4-column-limit) )은 가져 오기 문에 적용되지 않습니다.

#### 3.3.2 순서 및 간격

import문은 다음과 같이 정렬합니다 :

1. 모든 static import는 단일 블록으로 이루어집니다.
2. 모든 non-static import는 단일 블록으로 이루어집니다.

static 및 non-static import가 모두있는 경우 빈 줄 하나가 두 블록을 구분합니다.



### 3.4 클래스 선언

#### 3.4.1 하나의 최상위 클래스 선언

소스파일에는 정확히 하나의 최상의 클래스를 선언합니다.

#### 3.4.2 클래스 내용의 순서

클래스 내부의 변수와 메소드 순서는 코드 분석에 큰 영향을 줄 수 있습니다. 그러나 이에 대한  올바른 방법은 없습니다. 새로운 메소드는 클래스의 끝에 추가하는 것이 아니라 로직의 순서에 맡게 정렬해야 합니다.

##### 3.4.2.1 Overloads : 분할하지 않는다.

클래스에 여러 생성자가 있거나 동일한 이름을 가진 여러 메소드가있는 경우 이러한 클래스는 순차적으로 나타나며 사이에 다른 코드를 작성하지 않습니다.

```java
/*
 * Copyright (c) 1997, 2014, 저작권
 */
 
package 패키지;
 
import 참조 클래스;
 
public class 클래스명  {
}
```





## 4 서식 지정

**용어 참고 :** *블록과 유사한 구조* 는 클래스, 메서드 또는 생성자의 본문을 참조합니다. [배열 이니셜 라이저](https://google.github.io/styleguide/javaguide.html#s4.8.3.1-array-initializers) 에 대한 4.8.3.1 절 에서 모든 배열 이니셜 라이저 *는* 선택적으로 블록과 유사한 구조로 처리 *될 수* 있습니다.



### 4.1 교정기

#### 4.1.1 중괄호

 `if`, `else`, `for`, `do`및 `while`문에 사용되며, 본문이 비어 있거나 단 하나의 문이 포함 된 경우에 사용됩니다.

#### 4.1.2 비어 있지 않는 구획 : K & R style

중괄호는 Kernighan and Ritchie 스타일 ( " [Egyptian brackets](http://www.codinghorror.com/blog/2012/07/new-programming-jargon.html) ")을 따라 *비어 있지 않은* 블록과 블록과 같은 구조 를 *만듭니다* .

- 여는 중괄호 전에 줄 바꿈이 없습니다.
- 여는 중괄호 후에 줄 바꿈
- 닫는 중괄호 전에 줄 바꿈을 하십시오.
- 줄에 닫는 중괄호만 있는 경우는 메소드, 생성자, 클래스가 종료될 때이다.

예 :

```java
return () -> {
    while (condition()) {
    	method();
    }
};

// 여는 중괄호 전에는 줄바꿈이 없다.
return new MyClass() {
    @Override public void method() {
        if (condition()) {
            try {
              	something();
            } catch (ProblemException e) {
              r	ecover();
            }
        } else if (otherCondition()) {
          	somethingElse();
        } else {
          	lastThing();
          	// 닫는 중괄호 전에는 줄바꿈이 있다.
        }
    }
};
```

#### 4.1.3 빈 블록 : 간결함

중괄호 사이에 아무 문자가 없으면 즉시 닫을 수 있습니다.

예 :

```java
// 허용 가능
void doNothing () {}

// 허용 가능
void doNothingElse () {
}
```

```java
// 허용되지 않습니다 : 다중 블록 문에서 간결한 빈 블럭을 사용할 수 없습니다.
try {
    doSomething();
} catch (Exception e) {}    
```



### 4.2 블록 들여 쓰기 : +4 공백

새로운 블록이나 블록과 같은 구조가 열릴 때마다 들여 쓰기가 네 칸 증가합니다. 블록이 끝나면 들여 쓰기가 이전 들여 쓰기 수준으로 돌아갑니다. 들여 쓰기 수준은 블록 전체에서 코드와 주석 모두에 적용됩니다. (4.1.2 [비어 있지 않은 블록 : K & R 스타일](https://google.github.io/styleguide/javaguide.html#s4.1.2-blocks-k-r-style) 의 예를 참조하십시오 .)



### 4.3 한 줄에 하나의 문장

각 명령문 다음에 줄 바꿈이옵니다.



### 4.4 열 제한 : 200

Java 코드의 컬럼 제한은 200 자입니다. "문자"는 모든 유니 코드 코드 포인트를 의미합니다. 아래 명시된 경우를 제외하고,이 제한을 초과하는 모든 행은 4.5 절, [줄 바꿈](https://google.github.io/styleguide/javaguide.html#s4.5-line-wrapping) 에서 설명한대로 줄 바꿈되어야합니다 .

**예외 :**

1. 열 한계를 따르는 라인은 불가능합니다. (예 : Javadoc의 긴 URL)
2. `package`, `import`문
3. 쉘에 잘라 붙일 수있는 주석의 명령 행.



### 4.5 줄 바꿈

모든 상황에서 정확히 줄 바꿈하는 방법을 정확하게 보여주는 공식은 없고, 줄 바꿈은 일반적으로 열 제한을 방지하기 위해 적용됩니다.

#### 4.5.1 열이 바뀌는 지점

1. 일반 연산자  앞

   - 다음 **연산자와 같은**기호에도 적용됩니다.
     - 점 분리 기호  `.`
     - 메소드 참조의 두 콜론  `::` 
     - 앰퍼센트(&) `<T extends Foo & Bar>`
     - catch 블록의 파이프 라인(|) `catch (FooException | BarException e)`

2. 대입 연산자의 뒤

   - 이는 확장 `for`( "foreach") 문 에서 "할당 연산자와 유사한"콜론에도 적용됩니다 .

3. 메서드 또는 생성자 이름은 뒤에 오는 여는 괄호 ( `(`)에 연결 된 상태로 유지 됩니다.

4. 쉼표 ( `,`)는 앞에 토큰에 연결된 상태를 유지해야 합니다.

   ```java
   //일반 연산자 앞에서 줄바꿈이 일어난다.
   Class cls =
       new Class();
   //쉼표는 앞에 토큰에 연결된 상태를 유지한다.
   cls.method("a",
       "b");
   //메소드, 생성자의 이름에는 괄호「(」를 부착한 상태로 유지한다.
   cls.method(
       "aaa"
   //일반 연산자 앞에서 줄바꿈이 일어난다.
       +"bbb");
   ```

   ​

5. 람다의 몸체가 하나의 표현되지 않은 표현식으로 구성되어 있다면, 화살표 이전에 줄바꿈을 하지 않습니다.

   ```java
   MyLambda<String, Long, Object> lambda =
       (String label, Long value, Object obj) -> {
           ...
       };

   Predicate<String> predicate = str ->
       longExpressionInvolving(str);
   ```

#### 4.5.2 들여쓰기는 적어도 +4 공백으로 들여 씁니다.



### 4.6 공백

#### 4.6.1 수직 공백

1. 필드, 생성자, 메소드, 중첩 클래스, static initializers, instance initializers
   - **예외 :** 두 개의 연속 된 필드 사이에 빈 줄 (다른 코드가없는)은 선택 사항입니다. 이러한 공백 라인은 필요에 따라 필드의 *논리적 그룹* 을 만드는 데 사용됩니다 .
   - **예외 :** 열거 형 상수 사이의 빈 줄은 [4.8.1 절](https://google.github.io/styleguide/javaguide.html#s4.8.1-enum-classes) 에서 다룹니다 .
2. 코드를 논리적 하위 섹션으로 구성하는데 필요한만큼

여러 개의 연속적인 공백 행은 허용되지만 반드시 필요하지는 않습니다.

```java
public class MyClass {
    int a;
    int b;
    //수직공백
    public MyClass(){
        //if, for, catch 와 그 다음에 오는 '(' 사이에 공백문자
        //연산자 앞 뒤로는 공백문자 삽입
        for ( int i = 0; i < 10; i++) {
            //',', ':', ';'의 다음 이나 타입 캐스트시의 ')'  다음에 공백문자
            if ( i < 10 ) {
            //else, catch와 그 이전에 오는 '}' 사이에 공백문자
            } else {

            }
	    }
    }
  	//수직공백
    public void run() {
    }
}
```

#### 4.6.2 수형 공백

1. `if`, `for`, `catch`와 그다음에 오는 `(` 사이에 공백 문자

2. `else`, `catch`와 그다음에 오는 `}` 사이에 공백 문자

3. 같은 임의 예약어 분리 `if`, `for`또는 `catch`열린 괄호 (에서, `(`그 라인을 다음)

4. 같은 임의 예약어 분리 `else`나 `catch`닫는 중괄호 (에서, `}`그 라인에 선행)

5. ​

   ```
   {
   ```

   두 가지 예외가 있는 열린 중괄호 ( ) 전에 :

   - `@SomeAnnotation({a, b})` (사용 공간 없음)
   - `String[][] x = {{"foo"}};`( `{{`아래 항목 8에 의해 공간이 필요하지 않음 )

6. 임의의 2 항 또는 3 항 연산자의 양쪽 모두. 다음 "연산자와 같은"기호에도 적용됩니다.

   - 결합 형 바인드의 앰퍼샌드 : `<T extends Foo & Bar>`
   - 여러 예외를 처리하는 catch 블록 용 파이프`catch (FooException | BarException e)`
   - `:`향상된 `for`( "foreach") 문 에서 콜론 ( )
   - 람다 식의 화살표 : `(String str) -> str.length()`

   하지만

   - `::`메소드 참조 의 두 개의 콜론 ( )은 다음과 같이 작성됩니다.`Object::toString`
   - 점 분리 기호 ( `.`)는 다음과 같이 작성됩니다. `object.toString()`

7. 캐스팅 후 `,:;`또는 닫는 괄호 ( `)`)

8. `//`줄 끝 주석을 시작하는 이중 슬래시 ( ) 의 양면에 . 여기에는 여러 개의 공백이 허용되지만 필수는 아닙니다.

9. 선언의 유형과 변수 사이 : `List<String> list`

10. ​

  배열 이니셜 라이저의 양쪽 중괄호 안의 *선택 사항*

  - `new int[] {5, 6}`그리고 모두 유효`new int[] { 5, 6 }`

#### 4.6.3 수평 정렬 : 필요 없음

**용어 참고 : **가로 맞춤* 은 이전 줄의 특정 토큰 아래에 특정 토큰을 직접 표시하려는 목적으로 코드에 가변 개수의 추가 공백을 추가하는 연습입니다.

이 방법은 허용되지만 Google 스타일 에서는 **절대로 필요하지 않습니다** . 이미 사용 된 위치에서 수평 정렬 을 *유지할* 필요가 없습니다 .

다음은 정렬이없는 예입니다. 그런 다음 정렬을 사용합니다.

```java
private int x; // this is fine
private Color color; // this too

private int   x;      // 나중에 변수명을 편집했을 경우
private Color color;  // 정렬되지 않은 상태일 수 있습니다.
```

**팁 :** 정렬은 가독성을 높일 수 있지만 향후 유지 관리에 문제가됩니다. 한 줄을 만져야하는 미래의 변화를 고려하십시오. 이 변경으로 인해 이전에 기쁘게하는 서식이 엉망으로 남을 수 있으며 이는 **허용** 됩니다. 더 자주 그것은 코더 (아마도 당신)에게 주변 선상의 공백을 조정할 것을 촉구합니다. 아마도 계단식 재 포맷을 유발할 수 있습니다. 그 한줄짜리 변경은 이제 "폭발 반경"을 갖습니다. 이것은 최악의 경우 혼잡을 일으킬 수 있지만 결과적으로 여전히 버전 기록 정보가 손상되고 검토자가 느려지고 충돌을 악화시킵니다.



### 4.7 그룹 괄호 : 권장

선택적 그룹 괄호는 작성자와 검토자가 코드가없이 잘못 해석 될 가능성이 없으며 코드를 더 읽기 쉽게 만들지 않을 것에 동의하는 경우에만 생략됩니다. 이다 *없는* 모든 독자가 기억 전체 자바 연산자 우선 순위 테이블이 있다고 가정하는 것이 합리적.

```java
public class MyClass {
    //수직공백
    public void run() {
        int a = 0;
        int b = 0;
        //그룹화
        {
            a += 10;
            a -= 5;
            System.out.println(a);
        }
        //그룹화
        {
            b += 62;
            b -= 3;
            System.out.println(b);
        }
    }
}
```



### 4.8 특정 구성

#### 4.8.1 열거 형 클래스

enum 상수 뒤에 오는 각 쉼표 뒤에 줄 바꿈은 선택 사항입니다. 추가 공백 행 (대개 하나만 허용)이 허용됩니다. 이것은 한 가지 가능성입니다.

```java
private enum Answer {
  YES {
    @Override public String toString() {
      return "yes";
    }
  },

  NO,
  MAYBE
}
```

이 배열 이니셜 (4.8.3.1 절에 참조 것처럼없는 방법 및 상수 없음 서적 열거 클래스 임의로 포맷 될 수 [어레이를 초기화](https://google.github.io/styleguide/javaguide.html#s4.8.3.1-array-initializers) ).

```java
private enum Suit { CLUBS, HEARTS, SPADES, DIAMONDS }
```

enum 클래스 *는 클래스* 이므로 서식 지정 클래스의 다른 모든 규칙이 적용됩니다.



#### 4.8.2 변수 선언

##### 4.8.2.1 선언 당 하나의 변수

모든 변수 선언 (필드 또는 로컬)은 `int a, b;`사용되지 않는 선언과 같은 하나의 변수 만 선언합니다 .

**예외 :** 여러 변수 선언은 `for`루프 의 헤더에서 허용됩니다 .

##### 4.8.2.2 필요시 선언

지역 변수는 포함하는 블록이나 블록과 같은 구조의 시작에서 습관적으로 선언 **되지 않습니다** . 대신 로컬 변수는 범위를 최소화하기 위해 처음 사용 된 시점 (이유 내에서) 가까이에 선언됩니다. 로컬 변수 선언에는 일반적으로 초기화 프로그램이 있거나 선언 직후에 초기화됩니다.

```java
public class ExText() {
    //맴버 변수
    private String item;
    public void test() {
        int c, d; //이런 식으로 사용되지 않는다.
        //지역 변수는 상단 초기화
        String item2 = "테스트2"
        //맴버 변수는 사용될 때 선언한다.
        item = "테스트";
        System.our.println(item + item2);
    }
}
```



#### 4.8.3 배열

##### 4.8.3.1 배열 이니셜 라이저 : "블록과 같은"

임의의 배열 이니셜 라이저는 "블록 형 구조"처럼 *선택적* 으로 포맷팅 될 수 있습니다. 예를 들어, 다음 (모든 유효 **하지** 완전한 목록) :

```java
new int[] {           new int[] {
  0, 1, 2, 3            0,
}                       1,
                        2,
new int[] {             3,
  0, 1,               }
  2, 3
}                     new int[]
                          {0, 1, 2, 3}
```

##### 4.8.3.2 C 스타일 배열 선언을 하지 않는다.

배열 선언은 `String[] args`형식으로 선언하며 C 스타일 배열선언은 하지 않는다. 예:  `String args[]`



#### 4.8.4 스위치 문

##### 4.8.4.1 들여 쓰기

다른 블록과 마찬가지로 스위치 블록의 내용은 +4로 들여 쓰기됩니다.

switch문 다음에 줄 바꿈이 있고 들여 쓰기 수준은 블록이 열리는 것과 똑같이 +4 증가합니다. 다음 switch문 블록이 닫힌 것처럼 이전 들여 쓰기 수준으로 되돌아갑니다.

##### 4.8.4.2 통과 주석

switch ~ case문은 기본적으로 `break`, `continue`, `return`, `throw`로 switch의 블록 구분을 나타냅니다.

switch문을 블록하지 않고 계속 실행을 나타낸다면 `// 계속진행`과 같은 주석을 작성합니다.

```java
switch (input) {
    case 1:
    case 2:
        prepareOneOrTwo();
        // 계속 진행
    case 3:
        handleOneTwoOrThree();
        break;
    default:
        handleLargeNumber(input);
}
```

#### 4.8.5 Annotations

클래스, 메소드 또는 생성자에 적용되는 annotation은 고유 한 행 (즉, 한 행에 하나의 주석)에 작성해야 합니다. 예:

```java
@Override
@Nullable
public String getNameIfPresent() { ... }
```

**예외 : **단일 annotation의 경우 클래스, 메소드 또는 생성자와 함께 하나의 행으로 작성할 수 있습니다.

```java
@Override public int hashCode () { ... }
```

**필드의 경우에 한해서** 복수의 annotation도 필드와 함께 하나의 행으로 작성할 수 있습니다. 예 :

```java
@Partial @Mock DataLoader loader;
```

매개 변수, 지역 변수 또는 유형에 대한 annotation 서식 지정에 대한 특정 규칙은 없습니다.

#### 4.8.6 주석

이 섹션에서는 주석을 다룹니다. Javadoc은 섹션 7, [Javadoc](#s7-javadoc) 에서 별도로 다루어 집니다.

##### 4.8.6.1 블록 주석 스타일

주석은 주변 코드와 같은 레벨의 들여쓰기를 사용해야 합니다.

블록 주석 예:

```java
/*
 * This is
 * okay.
 */
 
// And so
// is this.
   
/* Or you can
 * even do this. */
```

주석은 별표 또는 기타 문자로 그려진 상자에 포함되지 않습니다.

**팁 :** 여러 줄로 된 주석을 쓸 때는 `/* ... */`자동 코드 작성자가 필요에 따라 줄을 다시 감싸는 스타일을 사용 하려는 경우 스타일을 사용하십시오 (단락 스타일). 대부분의 포맷터는 `// ...`스타일 주석 블록 에서 줄 바꿈을하지 않습니다 .

#### 4.8.7 숫자 리터럴

`long`값이 큰 정수 리터럴은 대문자 `L`접미사를 사용하고 소문자는 사용 하지 않습니다 (`1`과 같은 숫자와 혼동하지 않기 위해 ). 예를 들어, `3000000000L` 보다는 `3000000000l`.



## 5 이름 지정

### 5.1 식별자 유형별 규칙

#### 5.1.1 패키지 이름

패키지 이름은 모두 소문자이며, 연속된 단어는 이어서 작성합니다.(밑줄 사용하지 않음).

예:

- 옳은 예 : `com.example.deepspace`
- 틀린 예 : `com.example.deepSpace`, `com.example.deep_space`



#### 5.1.2 클래스 이름

클래스 이름은 PascalCase 로 작성해야 합니다.

- 클래스 : 일반적으로 명사 또는 명사구입니다. 예를 들어, `Character`또는 `ImmutableList`.
- 인터페이스 : 명사 또는 명사구 (예 : `List`) 일 수도 있지만 때로는 형용사 또는 형용사구로 작성할 수도 있습니다(예 : `Readable`).
- 테스트 : 테스트 할 클래스의 이름으로 시작하며, `Test`로 끝납니다. 예를 들어, `HashTest`또는 `HashIntegrationTest`.

#### 5.1.3 메소드 이름

메서드 이름은 CamelCase로 작성해야 합니다.

메소드 이름은 일반적으로 동사 또는 동사구입니다. 예를 들어, `sendMessage` 또는 `stop`.

#### 5.1.4 상수 이름

상수 이름에는 `CONSTANT_CASE` 대문자를 사용 하며 각 단어는 밑줄로 구분됩니다.

```java
// Constants
static final int NUMBER = 5;
static final ImmutableList<String> NAMES = ImmutableList.of("Ed", "Ann");
static final ImmutableMap<String, Integer> AGES = ImmutableMap.of("Ed", 35, "Ann", 32);
static final Joiner COMMA_JOINER = Joiner.on(','); // because Joiner is immutable
static final SomeMutableType[] EMPTY_ARRAY = {};
enum SomeEnum { ENUM_CONSTANT }

// Not constants
static String nonFinal = "non-final";
final String nonStatic = "non-static";
static final Set<String> mutableCollection = new HashSet<String>();
static final ImmutableSet<SomeMutableType> mutableElements = ImmutableSet.of(mutable);
static final ImmutableMap<String, SomeMutableType> mutableValues =
    ImmutableMap.of("Ed", mutableInstance, "Ann", mutableInstance2);
static final Logger logger = Logger.getLogger(MyClass.getName());
static final String[] nonEmptyArray = {"these", "can", "change"};     
```

이 이름은 일반적으로 명사 또는 명사구입니다.

#### 5.1.5 변수 필드 이름

변수 필드 이름 (static 또는 기타)은 CamelCase로 작성해야 합니다.

이 이름은 일반적으로 명사 또는 명사구입니다. 예를 들어, `computedValues`또는 `index`.

#### 5.1.6 매개 변수 이름

매개 변수 이름은 CamelCase로 작성해야 합니다.

공용 메서드에서 한 문자 매개 변수 이름은 피해야합니다.

#### 5.1.7 지역 변수 이름

지역 변수 이름은 CamelCase로 작성해야 합니다.

~~`final`과 불변인 지역변수는 상수로 선언되지 않아야 합니다.~~


### 5.2 CamelCase

때로는 영어 문구를 낙타의 경우로 변환하는 합리적인 방법이 있습니다 (예 : "IPv6"또는 "iOS"와 같은 두문자어 또는 비정상적인 구문이있는 경우). Mrblue 스타일은 다음과 같은 작성방법을 제시합니다.

산문 형태의 이름으로 시작 :

1. 구문을 일반 ASCII로 변환하고 apostrophes를 제거하십시오. 예를 들어, "Müller 's algorithm"은 "Muellers algorithm"이 될 수 있습니다.
2. 이 결과를 단어, 공백 및 나머지 구두점 (일반적으로 하이픈)으로 나눕니다.

   - 권장 단어 : 일반적인 단어로 된 경우라면 해당 부분으로 나눕니다 (예 : "AdWords"는 "ad words"). 반면 "iOS"와 같은 단어는 CamelCase에 적합하지 않습니다.
3. 이제 모든 것을 소문자로(약어 포함) 만들며, 대문자는 첫번째 문자만을 허용합니다.

4. 마지막으로 모든 단어를 하나의 식별자로 결합하십시오.

원래 단어의 대소 문자는 거의 전적으로 무시됩니다. 예 :

| 산문 형태                   | 옳은                                   | 잘못된                 |
| ----------------------- | ------------------------------------ | ------------------- |
| "XML HTTP  request"     | `XmlHttpRequest`                     | `XMLHTTPRequest`    |
| "new customer ID"       | `newCustomerId`                      | `newCustomerID`     |
| "inner stopwatch"       | `innerStopwatch`                     | `innerStopWatch`    |
| "supports IPv6 on iOS?" | `supportsIpv6OnIos`                  | `supportsIPv6OnIOS` |
| "YouTube importer"      | `YouTubeImporter`  `YoutubeImporter` |                     |

```java
/*
 * Copyright (c) 1997, 2014, 저작권
 */
// 패키지 이름은 모두 소문자입니다.
package com.example.deepspace;
 
// 클래스 이름은 형용사구(형용사+명사) 또는 명사구(명사+명사)로 이루어져 있으며 구분은 대문자로 한다.(파스칼 표기법)
public class CodingTestClass {
    // 상수는 모두 대문자로 표시되며 구분자는 밑줄로 구분한다.
    public final static String KEY_NUMBER1 = "1";
    // 맴버변수는 명사구(명사+명사)로 구분된다.
    private String key = "abcde";
 
    // 메소드는 동사구(동사+명사) 또는 명사구(명사+명사)로 구분된다.
    public String convertKey(String keyType) {
        // 지역변수는 자유롭게 단축이 가능하지만 축약은 대도록 지양하도록 한다. returnVal -> retVal
        String retVal = this.key + keyType
        return retVal;
    }
}
```



## 6 프로그래밍 실습

### 6.1 `@Override`: 항상 사용

인터페이스를 구현하거나, 부모 클래스로 부터 상속된 메소드를 재정의 할 경우는 `@Override` 주석을 반드시 선언해야 합니다.

**예외 :** 부모 메소드에 `@Deprecated`주석이 선언되어 있을 경우  `@Override`를 생략 할 수 있습니다.

### 6.2 예외 : 무시하지 않음

아래 명시된 경우를 제외하고는 catch 된 예외처리를 반드시 작성하여야 합니다. 하지만 간혹 아무런 조치를 취하지 않는 것이 적절할 때는 주석을 통해 정당화 되는 설명을 작성하여야 합니다.

```java
try {
    int i = Integer.parseInt(response);
    return handleNumericResponse(i);
} catch (NumberFormatException ok) {
    // it's not numeric; that's fine, just continue
}
return handleTextResponse(response);
```

### 6.3 정적(static) 멤버

직접적인 참조로 정적 메소드를 실행하여야 한다.

```java
Foo foo = new Foo();
Foo.staticMethod(); // good
foo.staticMethod(); // bad
```

### 6.4 Finalizers : 사용하지 않음

`Object.finalize`는 가비지 컬렉션(이하 GC)이 수행될 때 더 이상 사용하지 않는 리소스를 정리하는 작업을 진행하기 위해 호출 되는 종료자 메소드입니다. Java의 경우 메모리 관리를 JVM(Java virtual Machine)이 하는것이 원칙이며, 해당 명령문을 임의로 수행될 경우 시스템에 심각한 오류를 유발할 수 있습니다. `Object.finalize` **절대 사용하지 않습니다.**



```java
public class CodingTestClass extends CodingParentsClass {
    // 상속 받는 메소드는 반드시 Override를 선언한다.
    @Override
    public String transData(String data) {
        data += "trans";
        return data;
    }
    public static String getTransData() {
        return "exam";
    }
    public CodingTestClass() {
        // 정적 메소드는 반드시 직접참조를 한다.
        // 올바르지 못한 예:
        // CodingTestClass codingTest = new CodingTestClass();
        // codingTest.getTransData()
        try {
            String temp = transData(CodingTestClass.getTransData());
        } catch (Exception e) {
            // catch 안에 아무런 내용이 없으면 안된다.
            e.printStackTrace();
        }
    }
}
```



## 7 Java doc

### 7.1 포맷팅

#### 7.1.1 일반 서식

Javadoc 블록 의 *기본적인* 형식은 다음 예제와 같습니다.

```java
/**
 * 자바독에 대한 예제입니다.
 * 일반 적인 서식은 이런 형태입니다.
 */
public int method(String p1) { ... }
    
```

... 또는 한 줄짜리 예 :

```java
/ ** Javadoc의 짧은 예. * /
```

#### 7.1.2 단락

한 줄의 빈 라인입니다.

#### 7.1.3 절

`@param`, `@return`, `@throws`, `@deprecated`이 네 가지 유형은 반드시 설명이 있어야 합니다.

```java
/**
 * 자바독에 대한 예제입니다.
 * 일반 적인 서식은 이런 형태입니다.
 *
 * 단락은 빈라인으로 표시됩니다.
 * 주석의 절은 반드시 하나의 문장으로 표시합니다.
 * @param p1 테스트 데이터를 입력합니다.
 * @return  테스트 데이터를 1 가산한 값으로 반환합니다.
 * @throws String을 1의 정수로 변환시에 수가 아닌 값이 문자열에 존재하면 에러를 발생합니다.
 * @deprecated Java 1.7 이전 버전에서는 사용하지 않습니다.
 */
public int method(String p1) { ... }
```



### 8. Reference
- https://google.github.io/styleguide/javaguide.html
- http://nowonbun.tistory.com/378