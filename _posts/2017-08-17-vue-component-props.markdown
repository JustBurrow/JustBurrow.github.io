---
title: Vue.js 컴포넌트의 이상한 props 접근.
layout: post
---

# Vue.js 컴포넌트의 이상한 props 접근.

Vue.js의 [컴포넌트](https://kr.vuejs.org/v2/guide/components.html)를 사용한 UI를 만들고 있었다. 그런데 컴포넌트의 `props`가 이상하게 작동했다. 정확하게는 `props`로 넘겨준 인자를 사용할 때 문제가 생겼다.

## 여기까진 문제 없었다.

{% raw %}
```xml
<!DOCTYPE html>
<html>
<head>
  <title>Component test</title>
  <script type="application/javascript" src="https://unpkg.com/vue"></script>
</head>
<body>
  <h1>Component test</h1>
  <div id="app">
    <ol>
      <li is="component-item"
       v-for="item in componentItems"
       v-bind:key="item.key" v-bind:comp="item" v-bind:msg="'static message'">
     </li>
    </ol>
  </div>
  <footer>
    <script type="text/x-template" id="tpl-component-item">
      <li>
        <p v-bind:id="comp.id">{{ msg }}</p>
      </li>
    </script>
    <script type="application/javascript">
      Vue.component("component-item", {
        template: "#tpl-component-item",
        props: ["comp", "msg"]
      });
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
    </script>
  </footer>
</body>
</html>
```
{% endraw %}

이 HTML은 다음과 같이 문제없이 리스트를 렌더링한다.

![문제없는 Vue.js의 렌더링]({{ base.url }}/assets/vue-component-props/01.png)

## 출력을 하면 에러가 나야 하는데..?

{% raw %}
```xml
<script type="text/x-template" id="tpl-component-item">
  <li v-bind:id="comp.id">
    <p>{{comp.text}}</p>
  </li>
</script>
```
{% endraw %}

![왜 에러 없이 되는 건데?]({{ base.url }}/assets/vue-component-props/02.png)

`comp.text` 출력문에서 `undefined`의 `text` 속성에 접근할 수 없다는 에러와 함께 렌더링이 안됐었는데? 된다?
이 에러 때문에 포스트 작성을 시작했고, 문제 없는 `msg` 출력 코드를 커밋 하기 전에 한 테스트에서도 렌더링이 안됐는데? 왜 다시 하니까 에러도 없고 렌더링도 잘 되는 거지? 왜?
