<template>
  <p>
    <a @click="$emit('show')">{{ memo.title }}</a>
  </p>
</template>

<script>
export default {
  props: {
    memo: Object,
  },
  emits: ["show"],
}
</script>
