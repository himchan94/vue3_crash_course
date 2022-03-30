```css
[v-cloak] {
  display: none;
}
```

```html
<div id="app" v-cloak>
  {{greeting}}
  <input @keyup.enter="greet(greeting + '!!!!')" v-model="greeting" />

  <hr />
  <button @click.prevent.stop="toggleBox">Toggle Box</button>
  <div v-if="isVisible" class="box"></div>
</div>
```

렌더링 처음 vue.js가 처리되기 이전에 {{}}, v-if 문 등 보여주지 않을 내용들이 화면에 그대로 노출된다. v-cloak을 사용하면 이를 감출 수 있다.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <style>
      .box {
        background-color: purple;
        height: 200px;
        width: 200px;
      }

      .box.two {
        background-color: red;
      }

      .box.three {
        background-color: blue;
      }

      [v-cloak] {
        display: none;
      }
    </style>
  </head>
  <body>
    <div id="app" v-cloak>
      {{greeting}}
      <input v-model="greeting" />

      <hr />

      <div v-if="isVisible" class="box"></div>
      <div v-else-if="isVisible2" class="box two"></div>
      <div v-else class="box three"></div>
    </div>

    <script src="https://unpkg.com/vue@next"></script>
    <script>
      let app = Vue.createApp({
        data: function () {
          return {
            greeting: "Hello Vue 3!",
            isVisible: false,
            isVisible2: true,
          };
        },
      });

      app.mount("#app");
    </script>
  </body>
</html>
```

### v-directives

v-if vs v-show

v-on : 이벤트

```html
<button v-on:click="isVisible= !isVisible">Toggle Box</button>
```

**v-on:은 @로 대체가능하다.**

```html
<button @click="isVisible= !isVisible">Toggle Box</button>
```

```html
   <button @click="toggleBox">Toggle Box</button>
      <div v-if="isVisible" class="box"></div>
    </div>

    <script src="https://unpkg.com/vue@next"></script>
    <script>
      let app = Vue.createApp({
        data: function () {
          return {
            greeting: "Hello Vue 3!",
            isVisible: false,
            isVisible2: true,
          };
        },

        methods: {
          toggleBox() {
            this.isVisible = !this.isVisible;
          },
        },
      });

      app.mount("#app");
    </script>

```

key와 바인딩도 가능

```html
<input @keyup.enter="greet(greeting + '!!!!')" v-model="greeting" />
```

e.stopPropagation도 가능

```html
<button @click.prevent.stop="toggleBox">Toggle Box</button>
```
