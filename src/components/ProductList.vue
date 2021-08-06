<template>
  <div class="container">
    <div class="form__wrap">
      <h1 class="form__wrap__title">Добавление товара</h1>

      <form class="form">
        <label for="title">Наименование товара<span>*</span></label>
        <input
          @change="inputOnChange"
          type="text"
          required
          id="title"
          v-model="addProductInfo.title"
          placeholder="Введите наименование товара"
        />
        <p :class="addProductInfo.title == '' ? 'required__field' : 'none'">
          {{ fieldRequired }}
        </p>
        <label for="description">Описание товара</label>
        <input
          type="text"
          id="description"
          v-model="addProductInfo.description"
          placeholder="Введите описание товара"
        />
        <label for="src">Ссылка на изображение товара<span>*</span></label>
        <input
          @change="inputOnChange"
          type="text"
          required
          id="src"
          v-model="addProductInfo.src"
          placeholder="Введите ссылку"
        />
        <p :class="addProductInfo.src == '' ? 'required__field' : 'none'">
          {{ fieldRequired }}
        </p>
        <label for="price">Цена товара<span>*</span></label>
        <input
          @change="inputOnChange"
          type="number"
          min="0"
          required
          id="price"
          v-model="addProductInfo.price"
          placeholder="Введите цену"
        />
        <p :class="addProductInfo.price == '' ? 'required__field' : 'none'">
          {{ fieldRequired }}
        </p>
        <button
          class="add__to__list"
          :disabled="isButtonDisabled"
          type="submit"
          @click="addToList"
        >
          Добавить товар
        </button>
      </form>
    </div>
    <div class="select__wrap" v-if="productList.length != 0">
      <p @click="areOptionsVisible = !areOptionsVisible">{{ selected }}</p>

      <div class="select" v-if="areOptionsVisible">
        <div
          @click="selectOption(category)"
          class="select__items"
          v-for="category in categories"
          :key="category.value"
        >
          {{ category.name }}
        </div>
      </div>
    </div>
    <div class="product__list">
      <div v-for="product in productList" :key="product" class="product">
        <div class="src__wrap">
          <img class="src" :src="`${product.src}`" />
        </div>
        <div class="text__wrap">
          <div class="title">{{ product.title }}</div>
          <div class="description">{{ product.description }}</div>

          <div class="price">{{ product.price }} руб.</div>
        </div>
        <button type="button" class="remove" @click="removeItem(product.id)">
          <img src="/img/delete.png" alt="delete" />
        </button>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: "ProductList",
  data() {
    return {
      areOptionsVisible: false,
      categories: [
        {
          name: "Цена: по убыванию",
          value: "toMin",
        },
        {
          name: "Цена: по возрастанию",
          value: "toMax",
        },
        {
          name: "По наименованию",
          value: "alphabet",
        },
      ],
      isButtonDisabled: true,
      selected: "По умолчанию",
      fieldRequired: "Поле является обязательным",
      productList: [],
      addProductInfo: {
        title: "",
        description: "",
        src: "",
        price: "",
      },
    };
  },
  beforeMount() {
    let userList = localStorage.getItem("userList");
    if (userList) {
      this.productList = JSON.parse(userList);
    }
  },

  methods: {
    addToList: function () {
      this.productList = [
        ...this.productList,
        { ...this.addProductInfo, id: new Date().getTime() },
      ];
      localStorage.setItem("userList", JSON.stringify(this.productList));
      document.querySelector(".form").reset();
    },
    removeItem: function (id) {
      this.productList = this.productList.filter((item) => item.id != id);
      localStorage.setItem("userList", JSON.stringify(this.productList));
    },
    selectOption: function (option) {
      this.selected = option.name;
      let key = option.value;
      this.areOptionsVisible = false;
      if (key == "toMax") {
        this.productList.sort(function (a, b) {
          return a.price - b.price;
        });
      }
      if (key == "toMin") {
        this.productList.sort(function (a, b) {
          return b.price - a.price;
        });
      }
      if (key == "alphabet") {
        this.productList.sort((a, b) =>
          a.title.toLowerCase().localeCompare(b.title.toLowerCase())
        );
      }
    },
    inputOnChange: function () {
      this.isButtonDisabled =
        !this.addProductInfo.title ||
        !this.addProductInfo.src ||
        !this.addProductInfo.price;
    },
  },
};
</script>

<style scoped>
.container {
  display: flex;
  align-items: stretch;
  flex-direction: row;
  max-width: 1920px;
  margin: 32px auto;
  position: relative;
}

.select {
  position: absolute;
  top: 30px;
  left: 0;
  right: 0;
  font-family: Source Sans Pro;
  font-style: normal;
  font-weight: normal;
  font-size: 10px;
  color: #b4b4b4;
  padding: 10px 16px;
  background: #fffefb;
  box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.1);
  border-radius: 4px;
  border-top: none;
}

.select__items {
  padding: 5px 0;
}

.select__wrap {
  position: relative;
  cursor: pointer;
  position: absolute;
  top: -20px;
  right: 31px;
  font-family: Source Sans Pro;
  font-style: normal;
  font-weight: normal;
  font-size: 12px;
  color: #b4b4b4;
  padding: 10px 16px;
  background: #fffefb;
  box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.1);
  border-radius: 4px;
}
.form__wrap__title {
  font-family: Source Sans Pro;
  font-style: normal;
  font-weight: 600;
  font-size: 28px;
  color: #3f3f3f;
  padding-left: 32px;
}
.src__wrap {
  width: 300px;
  height: 200px;
  display: flex;
}
.src {
  object-fit: contain;
  width: 100%;
}
.form {
  display: flex;
  flex-direction: column;
  width: 30vw;
  background-color: #fffefb;
  box-shadow: 0px 20px 30px rgba(0, 0, 0, 0.04),
    0px 6px 10px rgba(0, 0, 0, 0.02);
  border-radius: 4px;
  padding: 24px;
  width: 100%;
  max-width: 332px;
  min-width: 300px;
}

.form input {
  width: 90%;
  padding: 10px 16px;
  box-shadow: 0px 20px 30px rgba(0, 0, 0, 0.04),
    0px 6px 10px rgba(0, 0, 0, 0.02);
  border-radius: 4px;
}

.form input::placeholder {
  font-family: Source Sans Pro;
  font-style: normal;
  font-weight: normal;
  font-size: 12px;
  color: #b4b4b4;
}

.form label {
  font-family: Source Sans Pro;
  font-style: normal;
  font-weight: normal;
  font-size: 1em;
  color: #49485e;
  padding-bottom: 4px;
}
#price::-webkit-inner-spin-button {
  display: none;
}

.add__to__list {
  background: #7bae73;
  box-shadow: 0px 2px 4px rgba(0, 0, 0, 0.1);
  border-radius: 10px;
  font-family: Inter;
  font-style: normal;
  font-weight: 600;
  font-size: 12px;
  color: #ffffff;
  display: flex;
  justify-content: center;
  align-items: center;
  align-self: center;
  width: 90%;
  padding: 10px 0;
  margin-top: 34px;
}

.add__to__list:disabled {
  background-color: gray;
}

.required__field,
span {
  font-family: Source Sans Pro;
  font-style: normal;
  font-weight: normal;
  font-size: 1em;
  color: #ff8484;
  display: inline;
}

.none {
  display: none;
}

.product__list {
  display: flex;
  justify-content: flex-start;
  align-items: baseline;
  flex-wrap: wrap;
  margin: 24px 32px;
}

.product {
  display: flex;
  justify-content: center;
  flex-direction: column;
  align-items: flex-start;
  background: #fffefb;
  box-shadow: 0px 20px 30px rgba(0, 0, 0, 0.04),
    0px 6px 10px rgba(0, 0, 0, 0.02);
  border-radius: 4px;
  margin: 8px;
  max-width: 300px;
  position: relative;
}
.remove {
  position: absolute;
  top: -5px;
  right: -5px;
  background: #ff8484;
  box-shadow: 0px 2px 4px rgba(0, 0, 0, 0.1);
  border-radius: 10px;
  padding: 8px;
  display: none;
}

.product:hover .remove {
  display: flex;
}

.text__wrap {
  padding: 16px;
}

.title {
  font-family: Source Sans Pro;
  font-style: normal;
  font-weight: 600;
  font-size: 20px;
  color: #3f3f3f;
}
.description {
  font-family: Source Sans Pro;
  font-style: normal;
  font-weight: normal;
  font-size: 16px;
  color: #3f3f3f;
  padding: 16px 0 32px;
}

.price {
  font-family: Source Sans Pro;
  font-style: normal;
  font-weight: 600;
  font-size: 24px;
  color: #3f3f3f;
}
@media (max-width: 768px) {
  .container {
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }
  .product__list {
    margin: 0;
    justify-content: center;
  }

  .select__wrap {
    position: relative;
    top: 0;
    right: 0;
  }
  .select {
    z-index: 99;
  }
}
</style>