<template>
  <ProductList />
</template>

<script>
import ProductList from "./components/ProductList.vue";

export default {
  name: "App",
  components: {
    ProductList,
  },
};
</script>

<style>
@font-face {
  font-family: RotondaC;
  src: url(/fonts/RotondaC\ Regular.ttf);
}

/*Обнуление*/

* {
  padding: 0;
  margin: 0;
  border: 0;
}

/*Меняет подсчет ширины блока*/

*,
*:before,
*:after {
  -moz-box-sizing: border-box;
  -webkit-box-sizing: border-box;
  box-sizing: border-box;
}

/*Убираем линии обводки*/

:focus,
:active {
  outline: none;
}

a:focus,
a:active {
  outline: none;
}

/*Делает эти элементы блочными*/

nav,
footer,
header,
aside {
  display: block;
}

/*Уравнивает поведение шрифтов в разных браузерах
    и на телефонах убирает самовольное увеличение шрифтов*/

html,
body {
  font-size: 14px;
  -ms-text-size-adjust: 100%;
  -moz-text-size-adjust: 100%;
  -webkit-text-size-adjust: 100%;
  background-color: #ffffff;
}

/*Просим эти теги наследовать семейство шрифта*/

input,
button,
textarea {
  font-family: inherit;
}

/*Убираются некоторые особенности для разных браузеров*/

input::-ms-clear {
  display: none;
}

button {
  cursor: pointer;
}

p {
  font-size: 16px;
}

h1 {
  font-size: 32px;
  width: 100%;
  display: inline;
}

button::-moz-focus-inner {
  padding: 0;
  border: 0;
}

a,
a:visited {
  text-decoration: none;
  color: black;
}

a:hover {
  text-decoration: none;
}

ul li {
  list-style: none;
  font-weight: 900;
  font-family: "Montserrat", sans-serif;
}

img {
  vertical-align: top;
  max-height: 300px;
}

/*Обнуляем все стили заголовков*/

h1,
h2,
h3,
h4,
h5,
h6,
p,
span {
  font-size: inherit;
  font-weight: 900;
  font-family: "Montserrat", sans-serif;
}
</style>
