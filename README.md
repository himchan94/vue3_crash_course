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
**.prevent.stop**

```html
<button @click.prevent.stop="toggleBox">Toggle Box</button>
```

### component

```js
app.component("login-form", {
  template: `
        <form @submit.prevent="handleSubmit">
          <h1>{{title}}</h1>
          <input type="email" v-model="email"/>
          <input type="password" v-model="password"/>
          <button>Log in</button>
        </form>
        
        `,
  data() {
    return {
      title: "Login Form",
      email: "",
      password: "",
    };
  },
  methods: {
    handleSubmit() {
      console.log(this.email, this.password);
    },
  },
});
```

### component props

v-bind:

**shortcut** :

```js
app.component("login-form", {
  template: `
        <form @submit.prevent="handleSubmit">
          <h1>{{title}}</h1>
          <custom-input type="email" :label="emailLabel"/>
          <custom-input type="password" :label="passwordLabel" />
          <button>Log in</button>
        </form>
        
        `,
  components: ["custom-input"],
  data() {
    return {
      title: "Login Form",
      email: "",
      password: "",
      emailLabel: "Email",
      passwordLabel: "Password",
    };
  },
  methods: {
    handleSubmit() {
      console.log(this.email, this.password);
    },
  },
});
```

```js
app.component("custom-input", {
  template: `
          <label>
            {{label}}
            <input type="text"/>
          </label>
        `,
  props: ["label"],
});
```

#### 만약 props로 string을 주고 싶다면? (v-bind 빼주면 됨)

```js
<custom-input type='email' label='emailLabel' />
```

### props changing

부모 컴포넌트에서 내려준 props를 자식 컴포넌트에서 변경하고 싶다면
어떻게 해야할까?!

props는 읽기전용이라 변경할 수 없음

```js
app.component("login-form", {
  template: `
        <form @submit.prevent="handleSubmit">
          <h1>{{title}}</h1>
          <custom-input v-model="email" :label="emailLabel"/>
          <custom-input v-model="password" :label="passwordLabel" />
          <button>Log in</button>
        </form>

        `,
  components: ["custom-input"],
  data() {
    return {
      title: "Login Form",
      email: "",
      password: "",
      emailLabel: "Email",
      passwordLabel: "Password",
    };
  },
  methods: {
    handleSubmit() {
      console.log(this.email, this.password);
    },
  },
});
```

```js
app.component("custom-input", {
  template: `
          <label>
            {{label}}
            <input type="text" v-model="inputValue"/>
          </label>
        `,
  props: ["label", "modelValue"],
  computed: {
    inputValue: {
      get() {
        return this.modelValue;
      },
      set(value) {
        this.$emit("update:modelValue", value);
      },
    },
  },
  // data(){
  //   return {
  //     inputValue:'',
  //   }
  // }
});
```
