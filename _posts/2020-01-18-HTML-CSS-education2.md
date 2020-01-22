---
layout: post
title: NHN Basecamp HTML/CSS 교육(2)
author: YunSeoWon
date: '2020-01-18 11:30:00'
category: html
summary: NHN Basecamp HTML/CSS 교육(2)
thumbnail: html_css.jpg
---



# HTML/CSS 교육 (2)

수업을 듣고 이해가 잘 되지 않아 집에서 계속 곱씹으면서 정리하고 있다. 프론트엔드 쪽으로 공부를 잘 안해봐서 그런지 쉽게 와닿지 않는다. 특히 레이아웃 배치와 관련해서 이해를 거의 하지 못했기 때문에 연습이 더 필요한 것 같다. 

이번에는 컨테이너 정렬부터 z-index까지 내용을 다룰 것이다.

  

## 가로 정렬과 세로 정렬

컨테이너를 나열하는 방법은 총 두 가지가 있다. 세로로 나열하는 **Block**과, 가로로 나열하는 **Inline**이 있다. 각각의 가로 정렬과 세로 정렬의 방법은 서로 다른데, 그 방법들을 알아보자.



#### Block의 가로 정렬

일단, 처음 block 적용을 다음과 같이 하자.

```html
<div class="container">
  <div class="child"></div>
  <div class="child"></div>
  <div class="child"></div>
</div>
```

```css
.container {
    width: 500px;
    height: 300px;
    display: "block";
    border: 1px solid #aaa;
}

.child:nth-child(1) {
    background-color: red;
    width: 100px;
    height: 100px;
}
.child:nth-child(2) {
    background-color: green;
    width: 100px;
    height: 100px;
}
.child:nth-child(3) {
    background-color: blue;
    width: 100px;
    height: 100px;
}
```

![before-block](/assets/img/posts/200118-git/before-block.png){:class="img-fluid"}



가로 정렬은 다음과 같이 한다.

```css
.child:nth-child(2) {
  	...
    margin: 0 auto; /* 가로 정렬 */
}
```

![hori-block](/assets/img/posts/200118-git/hori-block.png){:class="img-fluid"}



#### Inline의 가로 정렬

inline 적용을 다음과 같이 하자.

```html
<div class="container">
  <span class="inline">inline1</span>
  <span class="inline">inline2</span>
  <span class="inline">inline3</span>
</div>
```

```css
.container {
    width: 300px;
  	height: 200px;
    display: "inline";
    border: 1px solid #aaa;
}

.inline:nth-child(1) {
    color: red;
}
.inline:nth-child(2) {
    color: green;
}
.inline:nth-child(3) {
    color: blue;
}
```

![before-inline](/assets/img/posts/200118-git/before-inline.png){:class="img-fluid"}



가로 정렬은 다음과 같이 할 수 있다. (가운데 정렬 예시)

```css
.container {
  text-align: center; /* {left; right} */
}
```

![hori-inline](/assets/img/posts/200118-git/hori-inline.png){:class="img-fluid"}



세로 정렬은 line-height 속성으로 조절할 수 있다.

```css
.inline:nth-child(1) {
    color: red;
    line-height: 20px;
}
.inline:nth-child(2) {
    color: green;
    line-height: 120px;
}
.inline:nth-child(3) {
    color: blue;
    line-height: 70px;
}
```

![vert-inline](/assets/img/posts/200118-git/vert-inline.png){:class="img-fluid"}



하지만, 결과를 보면 세로 정렬은 block의 가로 정렬과 달리, 높이를 다르게 설정할 수 없다. 각각의 line-height가 다를 경우, 위치는 가장 큰 값으로 결정된다.



### Flex 가로/세로 정렬

Flex의 가로 세로 정렬은 두 가지 방법이 있다.

**justify-content**: flex-direction과 같은 방향을 정렬할 때 사용

**align-items**: flex-direction과 수직 방향을 정렬할 때 사용



justify-content의 정렬 방법은 크게 세 가지가 있다.

```css
.container {
	...
	justify-content: flex-start;		/* 시작 정렬 */
	/*justify-content: center;  		중앙 정렬 */
	/*justify-content: flex-end;  	끝 정렬 */
}
```



align-items의 정렬 방법도 마찬가지이다.

```css
.container {
	...
	align-items: flex-start;		/* 흐름 수직 방향으로부터 시작 정렬 */
	/*align-items: center;  		 흐름 수직 방향으로부터 중앙 정렬 */
	/*align-items: flex-end;  	 흐름 수직 방향으로부터 끝 정렬 */
}
```





## Position과 오버플로우 속성

### Position

Position은 html의 엘리먼트를 배치하는 방법을 지정한다. Position의 종류는 총 4가지이다.

#### Static

static은 기본 position값으로, left, right, top, bottom 값이 동작하지 않는다.

```css
.target {
	position: static;
	top: 10px;
	left: 10px;
}
```



#### Relative

relative는 엘리먼트를 원래 위치를 기준으로 이동하는 방법이다. top, bottom, left, right값을 이용하여 움직일 수 있다.

```css
.target {
	position: relative;
	top: 10px;
	left: 10px;
}
```



#### Absolute

absolute는 position: relative를 가진 가장 가까운 상위 엘리먼트를 기준으로 위치한다. 그런 엘리먼트가 없다면, body를 기준으로 위치한다. 이를 적용하면 자신의 원래 공간은 사라지게 된다.

```css
.target {
	position: absolute;
	top: 10px;
	left: 10px;
}
```



#### Fixed

transform 속성 값을 가진 가장 가까운 상위 엘리먼트를 가준으로 위치한다. 없으면 화면을 기준으로 위치한다.

```css
.target {
	position: fixed;
	top: 10px;
	left: 10px;
}
```





### Overflow

Overflow는 자식 엘리먼트가 부모 엘리먼트를 넘어갈 때의 렌더링 방식을 정의한다. 정의하는 지점은 부모 엘리먼트이다.



#### Visible

visible은 내용을 자르지 않고 그대로 보여준다. overflow의 기본 값이다.

```css
.parent {
  overflow: visible;
}
```



### Hidden

Hidden은 넘어가는 내용을 자른 후 보여준다.

```css
.parent {
  overflow: hidden;
}
```



#### Scroll

Scroll은 가로 세로 스크롤바를 항상 노출시킨다.

```css
.parent {
  overflow: scroll;
}
```



#### Auto

Auto는 내용이 넘치는 경우에만 스크롤바를 노출시킨다.

```css
.parent {
  overflow: auto;
}
```



### Text-overflow

Text overflow는 텍스트가 부모요소를 벗어날 때 사용한다. 말줄임 표시(...)를 위해 사용한다.

```css
.box {
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
}
```

text-overflow: ellipsis로 말줄임 표시를 할 수 있다. 이 때, overflow: hidden과 white-space: nowrap을 같이 써야 한다.

* white-space: nowrap - 개행을 모두 공백으로 처리하여 한 줄로 늘인 것 처럼 나타내게 한다.
* text-overflow: ellipsis - overflow가 일어날 때, (...) 표시를 한다.



### z-index

z-index는 어떤 것이 화면의 가장 위로 올라갈 것인지를 결정하는 것이다. 파워포인트에서 앞으로 보내기, 뒤로 보내기와 같은 역할을 한다고 하면 이해가 될 것이다.

z-index는 다음과 같이 정의할 수 있다.

```css
.box1 {
		...
    z-index: 1;
    ...
}
```

z-index는 다음과 같은 특성을 지닌다.

**1. 동일한 공간을 공유할 때, z-index가 낮은 것이 아래에 존재한다.**

z-index는 말 그대로 z축의 좌표라고 생각하면 된다. z-index값이 높을 수록 xy평면으로부터 위로 올라가게 되기 때문이다. 



![z-index1](/assets/img/posts/200118-git/z-index1.png){:class="img-fluid"}



**2. z-index 값이 같을 경우, 나중에 선언된 쪽이 위로 향한다.**

먼저, box 번호 순서대로 선언되었다고 가정하자. (1번이 첫 번째로, 5번이 5번 째로 선언) 그럴 때, 먼저 box3이 box5보다 z-index가 높을 경우, box3이 box5를 덮게 된다.

![z-index2](/assets/img/posts/200118-git/z-index2.png){:class="img-fluid"}



하지만, box3과 box5의 z-index가 동일(=3)할 경우, 나중에 선언된 box5가 box3을 덮게 된다.

![z-index3](/assets/img/posts/200118-git/z-index3.png){:class="img-fluid"}

