<template>
  <h1 v-if="typeContent === 'title'">{{ content }}</h1>

  <div class="avatar" v-else-if="typeContent === 'avatar'">
    <img :src="content" />
  </div>

  <h2 v-else-if="typeContent === 'subtitle'">{{ content }}</h2>

  <p v-else-if="typeContent === 'text'">{{ content }}</p>
</template>

<script>
export default {
  props: {
    content: String,
    typeContent: String,
  },
};
</script>

<style lang="scss" scoped></style>
