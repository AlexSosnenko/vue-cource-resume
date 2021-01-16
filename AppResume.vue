<template>
  <component
    v-for="block in blocks"
    :key="block.id"
    :is="'app-resume-' + block.tp"
    :txt="block.val"
  ></component>
  <h3 v-if="blocks.length === 0">
    Добавьте первый блок, чтобы увидеть результат
  </h3>
  <div v-else>
    <button class="btn primary" @click="$emit('save')" :disabled="!needSave">
      Сохранить
    </button>
    <button class="btn danger" @click="$emit('delete')" :disabled="!dbKey">
      Удалить
    </button>
  </div>
</template>
<script>
import AppResumeTitle from "./AppResumeTitle";
import AppResumeSubtitle from "./AppResumeSubtitle";
import AppResumeAvatar from "./AppResumeAvatar";
import AppResumeText from "./AppResumeText";
export default {
  emits: ["save", "delete"],
  props: {
    blocks: Array,
    dbKey: String,
    needSave: Boolean,
  },
  components: {
    AppResumeTitle,
    AppResumeSubtitle,
    AppResumeAvatar,
    AppResumeText,
  },
};
</script>