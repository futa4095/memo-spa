<template>
  <div class="memo-list">
    <MemoItem
      v-for="(memo, index) in memos"
      :memo="memo"
      :key="memo.id"
      @show="$emit('show', index)"
    />
  </div>
</template>

<script>
import MemoItem from "@/components/MemoItem.vue"

export default {
  components: {
    MemoItem,
  },
  props: {
    memos: Array,
  },
  emits: ["show"],
}
</script>
