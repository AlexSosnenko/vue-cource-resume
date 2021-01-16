<template>
  <p>
    <button class="btn primary" @click="$emit('upload')">
      Загрузить комментарии
    </button>
  </p>
  <div class="card" v-if="comments.length > 0">
    <h2>Комментарии: {{ comments.length }}</h2>
    <ul class="list">
      <li class="list-item" v-for="comment in comments" :key="comment.id">
        <div>
          <p>
            <strong>{{ comment.email }} </strong>
          </p>
          <small>{{ comment.name }}</small>
        </div>
      </li>
    </ul>
  </div>
</template>
<script>
export default {
  emits: ["upload"],
  props: ["comments"],
};
</script>