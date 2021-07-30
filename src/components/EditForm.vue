<template>
    <div class="modal-mask">
      <div class="modal-wrapper">
        <div class="modal-container">
          <form class="memo-edit">
            <textarea
              name="memo-text"
              id="memo-text"
              cols="40"
              rows="5"
              v-model="text"
              required
            ></textarea>
            <button @click.prevent="save" class="primary">保存</button>
            <button @click.prevent="$emit('delete', memo.id)" class="primary">削除</button>
          </form>
        </div>
      </div>
    </div>
</template>

<script>
import { ref } from "vue"

export default {
  props: {
    memo: Object,
  },
  emits: ["save", "delete"],
  setup(props, context) {
    const text = ref(props.memo.text)
    const save = () => {
      const title = text.value.split("\n")[0] ?? ""
      const memo = { id: props.memo.id, title: title, text: text.value }
      context.emit("save", memo)
    }
    return {
      text,
      save,
    }
  },
}
</script>

<style scoped>
.modal-mask {
  position: fixed;
  z-index: 9998;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: table;
  transition: opacity 0.3s ease;
}

.modal-wrapper {
  display: table-cell;
  vertical-align: middle;
}

.modal-container {
  width: 390px;
  margin: 0px auto;
  padding: 0px 0px;
  background-color: #fff;
  border-radius: 2px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.33);
  transition: all 0.3s ease;
}

button {
  width: 170px;
}

.memo-edit {
  margin-left: 0px;
  margin-right: 0px;
}
</style>
