---
layout: post
title: NHN Basecamp Javascript 교육
author: YunSeoWon
date: '2020-01-20 09:30:00'
category: javascript
summary: NHN Basecamp Javascript 교육
thumbnail: javascript.png
---





# Javascript DOM

이번에는 Javascript DOM에 대한 교육을 들었다. Javascript에서 HTML의 객체를 다루기 위해 알아야 할 것들에 대해 설명하였다. 7시간 동안 진행되는 교육이라 많은 것들을 배웠고 동시에 TodoList를 만드는 것을 주제로 실습도 하면서 DOM에 관해 배웠다. 







## Todolist를 만들자

FE를 개발할 때 이 주제로 개인 프로젝트를 하면 실력이 늘 것이다..

**SPEC**

- TODO 추가, 완료할 수 있다.
- TODO는 미완료 상태와 완료된 상태 디자인 적용
- 종류에 따라 갯수 카운팅(전체/활성/완료)
- TODO를 삭제 할수 있다.
- 종류별로 보기(전체/활성/완료)



## 환경구성

### Javascript 사용

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <title>JS FOR ROOKIE</title>
  </head>
  <body>
    <!--- 스크립트 파일을 인크루드하는 방식 ---->
    <script src="path/to/jsfile.js"></script>
      
    <!--- 스크립트를 직접 코딩하는 방식, 여기서 만든 변수는 전역 변수 ---->
    <script>
      .... your code here....
    </script>
  </body>
</html>
```





## 스크립트와 DOM과의 상호작용 시점

**DOM** (Document Object Model): 자바스크립트로 HTML 문서를 다룰 수 있게 브라우저 환경 제공하는 API



### 상호작용 시점 1

브라우저는 HTML 문서를 첫 줄부터 순서대로 파싱하여 필요헌 정보를 수집하고 렌더링한다.



### 상호작용 시점 2

JS가 평가되는 시점에  Document에 렌더링 되어있는 엘리먼트에만 접근 가능.



보통 스크립트를 body 태그 밑에 삽입하거나, window의 onload 이벤트를 이용하여 스크립트 실행을 지연시킬 수 있다. (엘리먼트를 먼저 선언 후 js 실행)



```HTML
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <title>javascript basic</title>
    <script>
      window.onload = function() {
      document.getElementById('clickme').addEventListener(); //OK
      };
    </script>
  </head>
  <body>
    <button id="clickme"></button>
  </body>
</html>
```

* window.onload()는 html 문서 렌더링이 끝난 후 실행시키는 함수이다.



### 엘리먼트 찾기

```javascript
document.getElementById('myId');  //returns HTMLElement or null
document.getElementsByTagName('div'); //returns HTMLCollection or null
document.getElementsByClassName('myClass'); //returns HTMLCollection or null
```

* getElementById: html 태그 중 해당 id을 가진 엘리먼트를 가져온다. (오직 하나)
* getElementByTagName: html 태그 중 해당 태그 이름을 가진 엘리먼트를 가져온다. (여러 개)
* getElementByClassName: html 태그 중 해당 클래스 이름을 가진 엘리먼트를 가져온다. (여러 개)



**HTMLCollection**

유사 배열

* 숫자로 인덱싱 가능, length 존재
* Array가 아니라 Array의 API를 사용할 수 없음
* Array의 API를 사용하려면 변환해야 함

live한 컬렉션이다.

* 참조 후 동일한 TagName이나 ClassName을 가진 엘리먼트가 존재하면, 자동으로 참조가 된다.



### 엘리먼트 안의 엘리먼트 찾기

HTML 엘리먼트는 다른 엘리먼트를 포함할 수 있다. (DOM은 트리구조로 모델링)

```javascript
element.getElementsByClassName('myClass');
element.getElementsByTagName('div');
```



**특정 엘리먼트의 자식 엘리먼트를 검색해 검색의 범위를 좁힐 수 있다.**

```javascript
var myContentsEl = document.getElementById('myContents');
myContentsEl.getElementByTagName('A');
```



### 엘리먼트에 이벤트 핸들러 적용

```javascript
element.addEventListener("click", function() {
  // Your code's here
}, false);
```

* 첫 번째 파라미터: 이벤트명 (click, focus, ...)
* 두 번째 파라미터: 이벤트 핸들러
  * this를 넣을 수도 있는데, 이는 이벤트가 적용된 엘리먼트이다.
* 세 번째 파라미터: 캡쳐링 여부



### 엘리먼트 이벤트 해제

```javascript
element.removeEventListener("click", handler, false);
```

* 두 번째 파라미터: 이벤트 핸들러.
  * 함수의 참조가 파라미터에 들어가는데, 참조를 저장해야 한다. 



### 동적으로 엘리먼트 만들기

**document.createElement(tagName)**

```javascript
var newDiv = document.createElement("div");
var newP = document.createElement("p");
```





**document.createTextNode(text)**

```javascript
var newText = document.createTextNode("텍스트 노드");
var newText2 = document.createTextNode("HELLO WORLD");
```

만들고 body 엘리먼트에 붙이지 않으면 화면에 나타나지 않는다.



```javascript
var newDiv = document.createElement("div");
var newP = document.createElement("p");
var newText = document.createTextNode("HELLO ROOKIES");

newP.appendChild(newText);
newDiv.appendChild(newP);

document.body.appendChild(newDiv);
```



그 결과는 다음과 같다.

```html
<body>
  <div>
    <p>HELLO ROOKIES</p>
  </div>
</body>
```



인덱스로 참조도 가능하다.

```javascript
document.body.childNodes[0].tagName == 'DIV';
document.body.childNodes[0].childNodes[0].tagName == 'P';
document.body.childNodes[0].childNodes[0].childNodes[0].nodeValue == 'HELLO ROOKIES';
```



### 자식 엘리먼트 삭제

지우려고 하는 대상의 참조를 알아야 지울 수 있다.

```javascript
var newDiv = document.createElement("div");
var newP = document.createElement("p");
var newText = document.createTextNode("HELLO ROOKIES");

newP.appendChild(newText);
newDiv.appendChild(newP);

document.body.appendChild(newDiv); //추가했지만
document.body.removeChild(newDiv); // 바로 삭제됨
```



## innerHTML을 이용해 동적으로 엘리먼트 만들기(1)

```javascript
element.innerHTML = '<div>text</div>';
```

- html 텍스트를 이용해 자식 노드를 생성

```javascript
document.body.innerHTML = "<div><p>HELLO ROOKIES</p></div>";
```

```HTML
<body>
  <div>
    <p>HELLO ROOKIES</p>
  </div>
</body>
```



**장점**

- 새로운 엘리먼트들을 만드는 상황에서는 보통 제일 빠름(엔진 내부에서 생성)
- DOM API를 이용하는것보다 편한 경우가 많다.

**단점**

- 엘리먼트에 작은 변화를 주는 경우 비효율적(돔트리를 모두 지우고 다시 만듬)
- 저 버전의 IE에서 상호참조로인한 메모리릭이 발생할 수 있다. (옛날 버전)



**주의점**

값이 세팅 될때마다 모든 자식 엘리먼트를 지우고 다시 만들기 때문에 Addition assginment(+=) 연산자를 직접 사용하는것을 피해야 한다.

```javascript
//wrong
div.innerHTML = '<p>text1</p>';
    // 자식으로 <p>text1</p> 을 만든다.
	// 여기까지는 괜찮은데..
div.innerHTML += '<p>text2</p>';
    // innerHTML === '<p>text1</p><p>text2</p>'
    // 모두 지우고 <p>text1</p> 다시만들고 <p>text2</p>도 만든다.
	// innertHTML의 내용은 변경되는데 모든 innerHTML 엘리먼트를 지우고 다시 innerHTML을 다시 만든다.. 반복이 늘어날수록 비효율적이다.

//correct
var content = '';

// HTML 스트링을 미리 만든다.
content += '<p>text1</p>'; // 루프에서 사용된다고 가정
content += '<p>text2</p>';

// 미리 string을 조합한 다음 innerHTML에 할당하자.......
div.innerHTML = content; // innerHTML 할당은 한번만
```



## textContent :LIVE:

- innerHTML과 비슷하지만 텍스트 노드를 생성하거나 참조할 수 있다.
- 자식엘리먼트들의 모든 텍스트 노드를 확인할 수 있다.

```javascript
element.textContent = 'new text content';
console.log(element.textContent);
```





## [Project] 1. TodoList 할일 추가

```javascript
var input = document.getElementById("todo-input");
input.value = "아무값 입력."

document.getElementById("add-button")
    .addEventListener("click", function() {
        var todoList = document.getElementById("todo-list");
        var li = document.createElement("li");
        //var textNode = document.createTextNode(input.value);
        //li.appendChild(textNode);

        li.textContent = input.value;
        todoList.appendChild(li);
    }, false);
```

* \<li> 사이에 오는 값은 textContent이다.
* textContent를 사용하지 않고 별도로 textNode를 만들어서 li에 child로 넣을 수도 있다.





## 체크박스 다루기 :LIVE:

**html**

```html
<input type="checkbox" value="someValue" checked />
```

* input의 type을 checkbox로 설정하면 된다.
* checked라는 프로퍼티를 이용하여 체크 되어있는지 나타낼 수 있다. (radio, checkbox)



**js**

```javascript
input.value // 값
input.checked // 체크여부(radio, checkbox)
input.type // HTML 어트리뷰트에 접근한다.(스펙상 정의된 어트리뷰트)
input.focus() // 포커스를 준다.
input.blur() //포커스를 해제한다.
// etc...
```



## 이벤트의 전파

이벤트는 특정한 방향으로 전파된다.

* 위에서 아래로
* 아래에서 위로



만들었던 todolist에서 체크박스를 클릭하면 이벤트가 발생이 되지 않는 것 처럼 보인다. li의 이벤트가 이루어지고, 체크박스의 이벤트가 이루어지기 때문에 두 번 작용되어 원래대로 되돌아오기 때문이다.



이벤트는 특정한 방향으로 전파된다.

* Capturing: 위에서 아래로
* bubbling: 아래에서 위로



순서는 캡처링 -> 실제 대상 엘리먼트 -> 버블링

체크박스를 클릭하면 캡처링으로 기본 체크박스 이벤트가 발생하고, 버블링이 발생하면서 li의 이벤트가 발생하여 체크박스 토글 이벤트가 또 발생한다. 그래서 두 번 체크되어서 이벤트가 발생되지 않는 것 처럼 보인다.



```javascript
element.addEventListener('click', function(eventObject) {
  eventObject.stopPropagation(); // 캡처링이나 버블링을 취소한다.(이벤트 전파를 차단한다)

  //etc
  eventObject.preventDefault(); // 디폴트 동작을 취소한다.(ex. 링크 이동 차단)
  //체크 박스의 클릭을 취소하는경우 브라우저마다 다른 동작을 한다.
  //그밖에 많에 정보를 포함
}, true); // true: capturing, false: bubbling
```

* eventObject: 이벤트와 관련된 객체. 
  * stopPropagation(): 캡쳐링이나 버블링을 취소할 수 있다.
  * preventDefault(): 디폴트 동작을 취소한다.



stopPropagation을 사용하지 않고 버블링 제어하기

```javascript
li.addEventListener("click", function(ev) {
            checkbox.checked = ev.target === li ? !checkbox.checked : checkbox.checked;
    		/*
    		if(ev.target.targetName == "li") {
    			checkbox.checked = !checkbox.checked;
    		}
    		*/
        }, false);
```

* ev.target은 이벤트가 발생한 타겟을 알려준다. checkbox를 클릭하면 target이 li가 아니라 checkbox로 된다. 이런식으로 이벤트 전파를 제어할 수 있다.



## CSS제어

엘리먼트 객체의 style 어트리뷰트 이용해 CSS를 적용한다.

```javascript
// css 속성을 그대로 사용함
element.style.width = '30px';
element.style.height = '100px';

element.style.display = 'block';	// block, inline: 구조에 남아있다. 
element.style.display = 'none';		// element: 구조상 빠진다. (visibility랑 다르다.)

// background-color같이 하이픈으로 이어진 속성은 카멜케이스로 변경
element.style.backgroundColor = '#f00';

// border같은 short-hand 속성도 그대로 사용함.
element.style.border = "1px solid red";

// 적용 가능 목록 확인
console.log(element.style);
```

head 부분에 style class를 지정하여 적용 가능



```html
<head>
	<style>
  		.complete {}
	</style> 
</head>
```

```javascript
li.addEventListener("click", function(ev) {
            if(ev.target === li) {
                checkbox.checked = !checkbox.checked;
            } 
            li.className = checkbox.checked ? "complete" : "incomplete";
        }, false);
```



## 카운팅: querySelector 

```javascript
document.querySelector('.myClass'); // 처음 찾은 한개만 리턴
document.querySelectorAll('.myClass'); // 해당되는 모든 엘리먼트 찾음
```



## Todo 삭제

DOM  상의 엘리먼트들은 트리구조로 서로 연결되어 있다. 그래서 엘리먼트의 노드 관련 속성들로 연결되어있는 엘리먼트들을 탐색할 수 있다.



```javascript
element.firstChild // 첫번째 자식
element.lastChild // 마지막 자식
element.parentNode // 부모
element.nextSibling // 다음 형제
element.previousSibling  // 이전 형제
element.childNodes // 자식들을 모두 담고 있는 HTMLCollection
```







### CSS 셀렉터를 이용하여 완료, 미완료, 전체를 눌렀을 때 필터링하는 방법

**CSS 설정 - 필터링**

```html
<style>
    .complete {
        color: red;
        text-decoration: line-through;
    }
    .hideIncomplete li:not(.complete) { display: none;} 
    /* 클래스 이름 (공백) 자식  -> 그 자식 이름에 해당 style을 맥인다.*/
    .hideComplete li.complete { display: none;}
</style>
```

**javascript 버튼 이벤트**

```JAVASCRIPT
completeCounter.addEventListener("click", function() {
    todoList.className = 'hideIncomplete';
}, false);

incompleteCounter.addEventListener("click", function() {
    todoList.className = 'hideComplete';
}, false);

totalCounter.addEventListener("click", function() {
    todoList.className = '';
}, false);
```

**고수는 이렇게 할 수도 있다.**

```javascript
document.getElementById('counter').addEventListener('click', function(ev) {
    switch(ev.target.id) {
        case 'total':
            todoList.className = '';
            break;
        case 'finished':
            todoList.className = 'hideIncomplete';
            break;
        case 'unfinished':
            todoList.className = 'hideComplete';
            break;
        default:
            todoList.className = '';

    }
}, false);
```


