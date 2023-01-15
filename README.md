# SpringMVC-2


# 1. 프로젝트 생성

Spring MVC 을 공부하기 위해 다시 프로젝트를 생성합시다!



추가로 인텔리제이에서 lombok 플러그인을 설치하고 Annotation 으로 Processing 이 가능하도록 합니다. (Enable annotation processing 체크)



원활한 학습을 위한 홈화면을 만듭니다.

`*src/main/resources/static/index.html*`

```jsx
<html>
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<ul>
    <li>텍스트
        <ul>
            <li><a href="/basic/text-basic">텍스트 출력 기본</a></li>
            <li><a href="/basic/text-unescaped">텍스트 text, utext</a></li>
        </ul>
    </li>
    <li>표준 표현식 구문
        <ul>
            <li><a href="/basic/variable">변수 - SpringEL</a></li>
            <li><a href="/basic/basic-objects?paramData=HelloParam">기본 객체들</a>
            </li>
            <li><a href="/basic/date">유틸리티 객체와 날짜</a></li>
            <li><a href="/basic/link">링크 URL</a></li>
            <li><a href="/basic/literal">리터럴</a></li>
            <li><a href="/basic/operation">연산</a></li>
        </ul>
    </li>
    <li>속성 값 설정
        <ul>
            <li><a href="/basic/attribute">속성 값 설정</a></li>
        </ul>
    </li>
    <li>반복
        <ul>
            <li><a href="/basic/each">반복</a></li>
        </ul>
    </li>
    <li>조건부 평가
        <ul>
            <li><a href="/basic/condition">조건부 평가</a></li>
        </ul>
    </li>
    <li>주석 및 블록
        <ul>
            <li><a href="/basic/comments">주석</a></li>
            <li><a href="/basic/block">블록</a></li>
        </ul>
    </li>
    <li>자바스크립트 인라인
        <ul>
            <li><a href="/basic/javascript">자바스크립트 인라인</a></li>
        </ul>
    </li>
    <li>템플릿 레이아웃
        <ul>
            <li><a href="/template/fragment">템플릿 조각</a></li>
            <li><a href="/template/layout">유연한 레이아웃</a></li>
            <li><a href="/template/layoutExtend">레이아웃 상속</a></li>
        </ul>
    </li>
</ul>
</body>
</html>
```



이렇게 세팅이 되었다면 본격적으로 학습을 진행해봅니다.

# 2. 타임리프 소개

먼저 타임리프에 대해 알아볼 것입니다.

- 공식 사이트: https://www.thymeleaf.org/

- 공식 메뉴얼 - 기본 기능: https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html

- 공식 메뉴얼 - 스프링 통합: https://www.thymeleaf.org/doc/tutorials/3.0/thymeleafspring.html

시간이 날 때 조금씩 읽어봅시다.

이전 시리즈에서 타임리프를 간단히 사용해보고 그 특징도 알아 보았죠. 

이번에는 간단히 타임리프의 개념을 보고, 실제 동작하는 기본 기능 위주로 알아봅시다.

## A. 타임리프 특징

- 서버 사이드 HTML 렌더링 (SSR)
- Natural Template
- Spring 통합 지원

### SSR

타임 리프는 백엔드 서버에서 HTML 을 동적으로 렌더링하는 용도로 사용됩니다.

### Natural Template

이전 글에서도 언급했었죠. 타임리프는 **순수 HTML 을 최대한 유지**합니다!

즉, 웹브라우저에서 파일을 직접 열어도 내용을 확인할 수 있고, 
서버를 통해 **뷰 템플릿**을 거치면 동적으로 변경된 결과를 확인할 수도 있습니다. 이게 큰 장점인지 잘 와닿지 않을 수 있어요.

JSP을 포함한 다른 뷰 템플릿들은 해당 파일을 열면, 예를 들어 JSP 파일 자체를 그대로 웹 브라우저에서 열면 JSP 코드와 HTML 코드가 섞여서 정상적인 HTML 결과를 확인할 수 없습니다. 오직 서버를 통해 JSP 가 렌더링되고 응답 결과를 받아야 화면을 확인할 수 있어요.

하지만 타임리프로 작성된 파일은 해당 파일을 그대로 웹 브라우저에서 열어도 정상적인 HTML 결과를 확인할 수 있습니다. 물론, 이 경우에는 동적으로 결과가 렌더링 되지 않죠~ 

하지만! HTML 마크업(Mock-up) 결과가 어떻게 되는지 서버 없이 파일만 열어도 바로 확인할 수 있습니다.

저번 시리즈에서 보았듯 순수 HTML 을 그대로 유지하면서 뷰 템플릿도 사용할 수 있는 타임리프의 특징을 Natural Templates 이라고 합니다.

### 스프링 통합 지원

Thymeleaf 는 스프링과 자연스럽게 통합되고, 스프링의 다양한 기능을 편리하게 사용할 수 있도록 지원합니다. 이 부분은 추후에 **‘스프링 통합과 폼’** 에서 알아봅시다.

## B. 타입리프 기본 기능

타임리프를 사용하기 위해 html 파일 상단에 아래 코드를 선언합니다.

```html
<html xmlns:th="http://www.thymeleaf.org">
```

### a. 기본 표현식

**간단한 표현**

- 변수 표현식: ${...}
- 선택 변수 표현식: *{...}
- 메시지 표현식: #{...}
- 링크 URL 표현식: @{...}
- 조각 표현식: ~{...}

**리터럴**

- 텍스트: 'one text', 'Another one!',…
- 숫자: 0, 34, 3.0, 12.3,…
- Boolean: true, false
- 널: null
- 리터럴 토큰: one, sometext, main,…

**문자 연산:**

- 문자 합치기: +
- 리터럴 대체: |The name is ${name}|

**산술 연산:**

- Binary operators: +, -, *, /, %
- Minus sign (unary operator): -

**Boolean 연산:**

- Binary operators: and, or
- Boolean negation (unary operator): !, not

**조건연산:**

- 비교: >, <, >=, <= (`gt`, `lt`, `ge`, `le`) → greater than 과 greater than or equal 약자인 듯.
- 동등 연산: ==, != (`eq`, `ne`) → equal 과 not equal 약자인 듯.
- 조건 연산:
    - If-then: `(if) ? (then)`
    - If-then-else: `(if) ? (then) : (else)`
    - Default: `(value) ?: (defaultvalue)`

- 특별한 토큰
    - No-Operation: `_`

더 자세한 내용은 아래 사이트에서 참조하세요!

[https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html#standardexpression-syntax](https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html#standardexpression-syntax)

# 3. 텍스트 - text, utext

타임 리프의 가장 기본 기능인 말 그대로 텍스트를 출력하는 기능을 알아봅시다. 

HTML 태그의 속성에 기능을 정의해서 동작하는데 HTML의 콘텐트(content)에 데이터를 출력할 때는 `th:text` 을 사용하면 됩니다.

```html
<span th:text="${data}">
```

HTML 태그의 속성이 아닌 HTML 콘텐트 영역 안에 직접 데이터를 출력하려면 대괄호를 두번 감싸면 됩니다.

```html
컨텐트 안에 직접 출력하기 = **[[ ${data}]]**
```

`*BasicController*`

```java
package hello.thymeleaf.basic;

@Controller
@RequestMapping("/basic")
public class BasicController {
    
    @GetMapping("text-basic")
    public String textBasic(Model model){
        model.addAttribute("data", "Hello Spring!");
        return "/basic/text-basic";
    }
}
```

*`/resources/templates/basic/text-basic.html`*

```html
<!DOCTYPE html>
<!--타임리프 사용 설정-->
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1>컨텐츠에 데이터 출력하기</h1>
<ul>
    **<li>th:text 사용 <span th:text="${data}"></span></li>
    <li>컨텐츠 안에서 직접 출력하기 = [[${data}]]</li>**
</ul>

</body>
</html>
```

[http://localhost:8080/basic/text-basic](http://localhost:8080/basic/text-basic) 결과



### Escape

HTML 문서는 `<,` `>` 같은 특수 문자를 기반으로 정의된다. 따라서 뷰 템플릿으로 HTML 화면을 생성할 때는 출력하는 데이터에 이러한 특수 문자가 있는지 주의해야 함. 

앞서 만든 데이터에서 Spring! 을 Bold 체로 바꿔봅시다.

```html
“Hello <b>Spring!</b>”
```

```java
@GetMapping("text-basic")
public String textBasic(Model model){
    model.addAttribute("data", "Hello <b>Spring!</b>");
    return "/basic/text-basic";
}
```

단순히 위처럼 바꾸면. 당연히 결과는 텍스트 이상하게 나올 것이다.



소스 보기로 하면 아래처럼 결과가 나온다.

`<` 부분이 `&lt;` 로 변경되고 `>` 부분은 `&gt;` 로 바뀌었다. 이것은 less than 이랑 greater than 의 부등호로 인식하는 것 같다. 



왜 이렇게 변경될까요…. 

이는 HTML 엔티티와 escape 기능과 관련이 있습니다.

**HTML 엔티티**

웹 브라우저는 < 를 HTML 태그의 시작으로 인식한다. 

그래서 < 를 태그의 시작이 아니라 **문자로 표현할 수 있는 방법**이 필요하다. 

이것이 **HTML 엔티티**라고 한다. 또 이렇게 HTML 에서 사용하는 특수문자를 HTML 엔티티로 변경하는 행위를 **“escape”** 라고 한다. 

타입리프의 `th:text` 와 `[[ ….. ]]` 는 **기본적으로 escape 을 제공**한다.

- `<` → `&lt;`
- `>` → `&gt;` (등 많은 HTML 엔티티)

그런데 우리는 escape 을 사용하지 않고 bold 체를 적용해야 한다. 

**Unescape**

타임리프는 escape 기능을 사용하지 않기 위해 아래 두 가지 기능을 제공한다.

- `th:text` → `th:**utext**`
- `[[ …. ]]` → `[**(** …. **)**]`

`*BasicController*` 에 추가.

```java
@GetMapping("text-unescaped")
public String textUnescaped(Model model){
    model.addAttribute("data", "Hello <b>Spring!</b>");
    return "/basic/text-unescaped";
}
```

`/resources/templates/basic/text-unescape.html`

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1>text vs utext</h1>
<ul>
    <li>th:text = <span th:text="${data}"></span></li>
    <li>th:utext=<span th:utext="${data}"></span></li>
</ul>
<h1><span th:inline="none">[[...]] vs [(...)]</span></h1>
<ul>
    <li><span th:inline="none">[[...]] = </span>[[${data}]]</li>
    <li><span th:inline="none">[(...)] = </span>[(${data})]</li>
</ul>

</body>
</html>
```

결과

[localhost:8080/basic/text-unescaped](http://localhost:8080/basic/text-unescaped)



위 페이지의 소스 보기 결과를 볼까요?

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1>text vs utext</h1>
<ul>
    <li>th:text = <span>Hello **&lt;b&gt;Spring!&lt;/b&gt;</span></li>**
    <li>th:utext=<span>Hello **<b>Spring!</b></span></li>**
</ul>
<h1><span>[[...]] vs [(...)]</span></h1>
<ul>
    <li><span>[[...]] = </span>Hello **&lt;b&gt;Spring!&lt;/b&gt;</li>**
    <li><span>[(...)] = </span>Hello **<b>Spring!</b></li>**
</ul>

</body>
</html>
```

우리가 원하는 대로 잘 적용이 된 것을 알 수 있습니다.

<aside>
💡 실제 서비스를 개발하다 보면 escape 을 사용하지 않아서 HTML이 정상 렌더링 되지 않는 많은 문제가 생긴다. 
escape 을 기본으로 하고 **꼭 필요할 때만 unescape 을 사용합시다!**

</aside>


