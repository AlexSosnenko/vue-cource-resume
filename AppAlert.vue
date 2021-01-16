<template>
  <div class="container" v-if="alert.text">
    <div class="container alert" :class="alert.type">
      <h3>{{ alert.title }}</h3>
      <p>{{ alert.text }}</p>
      <button class="btn" :class="alert.type" @click="$emit('close')">
        Закрыть
      </button>
    </div>
  </div>
</template>
<script>
export default {
  emits: ["close"],
  props: ["alert"],
};
</script>
