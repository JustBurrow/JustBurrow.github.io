---
title: Vue.js 컴포넌트의 이상한 props 접근.
layout: post
---

Vue.js의 [컴포넌트](https://kr.vuejs.org/v2/guide/components.html)를 사용한 UI를 만들고 있었다. 그런데 컴포넌트의 `props`가 이상하게 작동했다. 정확하게는 `props`로 넘겨준 인자를 사용할 때 문제가 생겼다.

## 여기까진 문제 없었다.

{% raw %}
```html
<!DOCTYPE html>
<html>
<head>
  <title>Component test</title>
  <script type="application/javascript" src="https://code.jquery.com/jquery-3.2.1.min.js"></script>
  <script type="application/javascript" src="https://unpkg.com/vue"></script>
  <script type="application/javascript">
    Vue.component("component-item", {
      template: "#tpl-component-item",
      props: ["comp", "msg"]
    });

    $(document).ready(function(){
      var data = {
        componentItems: [{
          key: 0,
          id: 1,
          text: "test component item #1"
        }]
      };
      var app = new Vue({
        el: "#app",
        data: data
      });
    });
  </script>
</head>
<body>
  <h1>Component test</h1>
  <div id="app">
    <ol>
      <li is="component-item"
       v-for="item in componentItems" v-bind:key="item.key" v-bind:comp="item" v-bind:msg="'static message'"></li>
    </ol>
  </div>
  <script type="text/x-template" id="tpl-component-item">
    <li :id="comp.id">
      <p>{{msg}}</p>
    </li>
  </script>
</body>
</html>
```
{% endraw %}

이 HTML은 다음과 같이 문제없이 리스트를 렌더링한다.

![문제없는 Vue.js의 렌더링]({{ base.url }}/assets/vue-component-props/01.png)

## 에러 나기 시작

![뭔가가 잘못되기 시작]({{ base.url }}/assets/vue-component-props/02.png)

그런데 뭔가 이상하다. 메시지가 나오긴 나오는데 에러가 난다.
그래도 화면은 문제없이 나오니까 넘어가고, 테스트하느라 사용한 `msg`는 삭제.

{% raw %}
```html
<!DOCTYPE html>
<html>
<head>
  <title>Component test</title>
  <script type="application/javascript" src="https://code.jquery.com/jquery-3.2.1.min.js"></script>
  <script type="application/javascript" src="https://unpkg.com/vue"></script>
  <script type="application/javascript">
    Vue.component("component-item", {
      template: "#tpl-component-item",
      props: ["comp"]
    });

    $(document).ready(function(){
      var data = {
        componentItems: [{
          key: 0,
          id: 1,
          text: "test component item #1"
        }]
      };
      var app = new Vue({
        el: "#app",
        data: data
      });
    });
  </script>
</head>
<body>
  <h1>Component test</h1>
  <div id="app">
    <ol>
      <li is="component-item"
       v-for="item in componentItems" v-bind:key="item.key" v-bind:comp="item" v-bind:msg="'static message'"></li>
    </ol>
    <script type="text/x-template" id="tpl-component-item">
      <li :id="comp.id">
        <p>{{msg}}</p>
      </li>
    </script>
  </div>
</body>
</html>
```
{% endraw %}

## 망했다

시험용 변수 대신 데이터를 출력하도록 수정.

{% raw %}
```html
<script type="text/x-template" id="tpl-component-item">
  <li :id="comp.id">
    <p>{{comp.text}}</p>
  </li>
</script>
```
{% endraw %}

![아무 것도 안나온다...]({{ base.url }}/assets/vue-component-props/03-1.png)
![아무 것도 안나온다...(2)]({{ base.url }}/assets/vue-component-props/03-2.png)

그런데 나오라는 데이터는 안나오고 에러만 나온다.

## 이상해!!!

그래서 시험을 해보는데...

{% raw %}
```html
<script type="text/x-template" id="tpl-component-item">
  <li :id="comp.id">
    <p>{{comp}}</p>
  </li>
</script>
```
{% endraw %}

![너는 왜 나와...]({{ base.url }}/assets/vue-component-props/04.png)

에러가 나기는 하지만, 이건 왜 나오는지 이유가 뭔지 모르겠다.

## 결론

```
[Vue warn]: Property or method "comp" is not defined on the instance but referenced during render. Make sure to declare reactive data properties in the data option.

(found in <Root>)
```

여기에 사용한 코드보다는 좀 더 복잡한 코드를 작성하던 도중에 이 문제를 겪었다. 그리고 약 하루 반 정도를 썼는데, 원인은 아주 간단했다.
템플릿 엘리먼트의 위치가 문제였다.

{% raw %}
```html
<!-- 문제의 템플릿 엘리먼트 -->
<script type="text/x-template" id="tpl-component-item">
  <li :id="comp.id">
    <p>{{comp}}</p>
  </li>
</script>
```
{% endraw %}

처음에는 `div#app`과 `script#tpl-component-item` 엘리먼트가 형제 관계 였다.

```
<body>
  - <div id="app">
  - <script id="tpl-component-item">
```

그런데 이걸 묶어서 관리하겠다고 템플릿(`script#tpl-component-item`) 위치를 옮겼다.

```
<body>
  - <div id="app">
    - <script id="tpl-component-item">
```

`Vue` 오브젝트를 만들면

```javascript
var app = new Vue({
  el: "#app",
  data: data
});
```

`div#app` 밑에 있는 템플릿 엘리먼트를 스캔, HTML 엘리먼트에 뭔가 처리를 하고,

```javascript
Vue.component("component-item", {
  template: "#tpl-component-item",
  props: ["comp", "msg"]
});
```

컴포넌트(`component-item`)가 템플릿에 접근하면서 문제가 생긴 것이다.

아니면 반대로 컴포넌트를 등록하면서 템플릿 엘리먼트에 어떤 처리를 해두는데 이걸 `Vue` 오브젝트 초기화 과정에 잘못 건드는 것일 수도 있다.

어느 쪽이든, **`Vue` 애플리케이션이 되는 엘리먼트 밑에 템플릿 코드를 두지 말자.**

## 참고

* [예제 코드](https://github.com/JustBurrow/pages/tree/master/vue-component-props)
* [Vue 2.0+ v-for. Getting a 'Property or method is not defined on the instance but referenced during render` error](https://github.com/vuejs/vue/issues/4276#issuecomment-262192295)
