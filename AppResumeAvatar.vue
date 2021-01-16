<template>
  <div class="avatar">
    <img :src="txt" />
  </div>
</template>
<script>
export default {
  props: {
    txt: String,
  },
};
</script>