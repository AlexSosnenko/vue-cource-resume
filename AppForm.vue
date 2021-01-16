<template>
  <form class="card card-w30" @submit.prevent="submitForm">
    <div class="form-control">
      <label for="type">Тип блока</label>
      <select id="type" v-model="tp">
        <option value="title">Заголовок</option>
        <option value="subtitle">Подзаголовок</option>
        <option value="avatar">Аватар</option>
        <option value="text">Текст</option>
      </select>
    </div>

    <div class="form-control">
      <label for="value">Значение</label>
      <textarea id="value" rows="3" v-model="val"></textarea>
      <small v-if="val.length < 4">Введите более 3-х символов</small>
    </div>

    <button class="btn primary" :disabled="val.length < 4">Добавить</button>
  </form>
</template>
<script>
export default {
  emits: ["add"],
  data() {
    return {
      tp: "title",
      val: "",
    };
  },
  methods: {
    submitForm() {
      if (this.val.length > 3) {
        this.$emit("add", this.tp, this.val);
        this.tp = "title";
        this.val = "";
      }
    },
  },
};
</script>