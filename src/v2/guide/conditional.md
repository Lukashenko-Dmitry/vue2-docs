---
title: Умовне промальовування
type: guide
order: 7
---

<div class="vueschool"><a href="https://vueschool.io/lessons/vuejs-conditionals?friend=vuejs" target="_blank" rel="sponsored noopener" title="Learn how conditional rendering works with Vue School">Learn how conditional rendering works with a free lesson on Vue School</a></div>

## `v-if`

Ця директива `v-if` використовується для умовного промальовування блоку. Отже, цей блок буде відмальовано лише тоді, коли вираз, що передається в директиву, повертає значення яке можн аінтерпретувати як істина.

``` html
<h1 v-if="awesome">Vue дивовижний!</h1>
```

Також є можливість додати "блок інакше", використавши `v-else`:

``` html
<h1 v-if="awesome">Vue дивовижний!</h1>
<h1 v-else>О ні 😢</h1>
```

### Умовні групи з `v-if` в `<template>`

Саме тому що `v-if` є директивою, це дозволяє додати її до одиночного блоку. Однак, що робити якщо ви бажаєте використати цю директиву до більш ніж одного елементу? В цьому випадку вам необхідно додати `v-if` в `<template>` елемент і огорнути ним ваші блоки, цей елемент буде керувати відмальовуванням блоків що знаходяться в середині а сам не зявиться в дереві елементів. Отже фінальний результат відмальовування не буде включати елемент `<template>`.

``` html
<template v-if="ok">
  <h1>Title</h1>
  <p>Paragraph 1</p>
  <p>Paragraph 2</p>
</template>
```

### `v-else`

Витакож можете використовувати `v-else` директиву щоб виділити "блок інакше" для `v-if`:

``` html
<div v-if="Math.random() > 0.5">
  Зараз ви бачите це
</div>
<div v-else>
  А зараз ні
</div>
```

Також `v-else` блок мусить розміщуатись безпосередньо після `v-if` чи `v-else-if` блоку - інакше він не буде розпізнаний і не буде відмальовуватись взагалі.

### `v-else-if`

> Нове у 2.1.0+

Отже `v-else-if`, як випливає з назви, використовується для "блоку інакше якщо" після `v-if`. На відміну від `v-else` його можна використовувати декілька разів:

```html
<div v-if="type === 'A'">
  A
</div>
<div v-else-if="type === 'B'">
  B
</div>
<div v-else-if="type === 'C'">
  C
</div>
<div v-else>
  Not A/B/C
</div>
```

Подібно до `v-else`, `v-else-if` елемент має слідувати безпосередньо після `v-if` чи `v-else-if` елементу.

### Контроль багаторазових елементів за допомогою `key`

Vue tries to render elements as efficiently as possible, often re-using them instead of rendering from scratch. Beyond helping make Vue very fast, this can have some useful advantages. For example, if you allow users to toggle between multiple login types:

Vue намагається промальовувати елементи максимально ефективно, часто повторно використовуючи їх замість побудови з нуля. Окрім того, що це допомагає зробити Vue дуже швидким, це може мати деякі корисні переваги. Наприклад, якщо ви дозволяєте користувачам перемикатися між кількома типами вводу

``` html
<template v-if="loginType === 'username'">
  <label>Username</label>
  <input placeholder="Введіть ваше Ім'я">
</template>
<template v-else>
  <label>Email</label>
  <input placeholder="Введіть вашу електронну адресу">
</template>
```

Отже, перемикання `loginType` у наведеному вище коді не зітре того, що користувач уже ввів. Оскільки обидва шаблони використовують однакові елементи, `<input>` лише змінюється його `placeholder`.

Ви можете перевірити це, ввівши трохи тексту у форму, і натиснувши кнопку перемикання:

{% raw %}
<div id="no-key-example" class="demo">
  <div>
    <template v-if="loginType === 'username'">
      <label>Username</label>
      <input placeholder="Enter your username">
    </template>
    <template v-else>
      <label>Email</label>
      <input placeholder="Enter your email address">
    </template>
  </div>
  <button @click="toggleLoginType">Toggle login type</button>
</div>
<script>
new Vue({
  el: '#no-key-example',
  data: {
    loginType: 'username'
  },
  methods: {
    toggleLoginType: function () {
      return this.loginType = this.loginType === 'username' ? 'email' : 'username'
    }
  }
})
</script>
{% endraw %}

Однак це далеко не завжди те чого ви бажаєте досягти, тому Vue пропонує вам спосіб сказати: "Ці два елементи абсолютно різні - не використовуй їх повторно". Додайте атрибут `key` з унікальними значеннями:

``` html
<template v-if="loginType === 'username'">
  <label>Username</label>
  <input placeholder="Вкажіть ваше ім'я" key="username-input">
</template>
<template v-else>
  <label>Email</label>
  <input placeholder="Вкажіть вашу електронну адресу" key="email-input">
</template>
```

Now those inputs will be rendered from scratch each time you toggle. See for yourself:
Тепер ці поля вводу будуть відмальовуватись з нуля кожного разу, коли ви перемикаєтесь. Переконайтесь самі:

{% raw %}
<div id="key-example" class="demo">
  <div>
    <template v-if="loginType === 'username'">
      <label>Username</label>
      <input placeholder="Enter your username" key="username-input">
    </template>
    <template v-else>
      <label>Email</label>
      <input placeholder="Enter your email address" key="email-input">
    </template>
  </div>
  <button @click="toggleLoginType">Toggle login type</button>
</div>
<script>
new Vue({
  el: '#key-example',
  data: {
    loginType: 'username'
  },
  methods: {
    toggleLoginType: function () {
      return this.loginType = this.loginType === 'username' ? 'email' : 'username'
    }
  }
})
</script>
{% endraw %}

Зауважте, що елементи <label> використовуються повторно, оскільки вони не містяь `key` атрибут.

## `v-show`

Another option for conditionally displaying an element is the `v-show` directive. The usage is largely the same:
Іншим варіантом умовного відображення елемента є директива `v-show`. Використання, в основному, схоже:

``` html
<h1 v-show="ok">Привіт!</h1>
```

Різниця полягає в тому, що елемент з `v-show` завжди буде відображатися і залишатись у DOM; `v-show` лише перемикає CSS властивість `display` елементу.

<p class="tip">Зверніть увагу, що `v-show` не підтримує елемент `<template>`, а також не працює з `v-else`.</p>

## `v-if` vs `v-show`

`v-if` - це "справжнє умовне промальовування", оскільки воно забезпечує належне знищення та повторне створення елментів та дочірніх компонентів усередині умовного блоку, коли перемикається умова промальовування.

`v-if` також **лінива**: якщо умова хибна при початковому промальовуванні, нічого не відбувається - умовний блок не відображатиметься, поки умова вперше не стане істинною.

Для порівняння, `v-show` набагато простіший - елемент завжди промальвується незалежно від початкових умов, з перемиканням CSS властивості `display`.

Взагалі кажучи, у `v-if` вищі витрати на перемикання, тоді як у `v-show` вищі початкові витрати на рендеринг. Тому слід віддати перевагу «v-show», якщо вам потрібно щось дуже часто перемикати, та «v-if», якщо стан навряд чи зміниться під час виконання.

Також слід зауважити, що у випадку використання `v-if` при перемиканні стану всі ввденні користувацькі дані будуть знищені.

## `v-if` with `v-for`

<p class="tip">Використовувати `v-if` та` v-for` разом **не рекомендується**. Перглянь [style guide](/v2/style-guide/#Avoid-v-if-with-v-for-essential) для детального інформування.</p>

При використанні разом із `v-if`, `v-for` має більш вищий пріоритет, ніж `v-if`. Перглянь <a href="../guide/list.html#v-for-with-v-if">промальовування списків</a> для детального інформування.
