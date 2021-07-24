<template>
  <h1>Memo app SPA版♨︎</h1>
  <MemoList :memos="memos" @show="showMemo" />
  <a class="button" @click="newMemo">+ メモを追加する</a>
  <EditForm
    v-if="showEditForm"
    :memo="editingMemo"
    @save="saveMemo"
    @delete="deleteMemo"
  />
</template>

<script>
import MemoList from "@/components/MemoList.vue"
import EditForm from "@/components/EditForm.vue"
import { computed, ref } from "vue"

export default {
  components: {
    MemoList,
    EditForm,
  },
  setup() {
    const MEMO_STORAGE_KEY = "memoapp/memos"
    const loadMemos = () => {
      return JSON.parse(localStorage.getItem(MEMO_STORAGE_KEY)) ?? []
    }
    const saveMemos = () => {
      localStorage.setItem(MEMO_STORAGE_KEY, JSON.stringify(memos.value))
    }
    const memos = ref(loadMemos())
    const editingMemo = ref(null)
    const showEditForm = computed(() => {
      return !!editingMemo.value
    })
    const newMemo = () => {
      const memo = {
        id: new Date().toISOString(),
        title: "新規メモ",
        text: "",
      }
      memos.value.push(memo)
      showMemo(memos.value.length - 1)
    }
    const saveMemo = newMemo => {
      const memo = memos.value.find(memo => memo.id === newMemo.id)
      memo.title = newMemo.title
      memo.text = newMemo.text
      editingMemo.value = null
      saveMemos()
    }
    const deleteMemo = id => {
      const index = memos.value.findIndex(memo => memo.id === id)
      if (index >= 0) {
        memos.value.splice(index, 1)
      }
      editingMemo.value = null
      localStorage.setItem(MEMO_STORAGE_KEY, JSON.stringify(memos.value))
      saveMemos()
    }
    const showMemo = index => {
      editingMemo.value = memos.value[index]
    }
    return {
      memos,
      editingMemo,
      showEditForm,
      newMemo,
      saveMemo,
      deleteMemo,
      showMemo,
    }
  },
}
</script>
