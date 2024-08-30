<script setup lang="ts">
import { ref } from 'vue';

const formShow = ref<boolean>(false);
const memoText = ref<string>('');
const memos = ref<Array<{ id: number; date: string; text: string; backgroundColor: string }>>([]);
const alertFormText = ref<string>('');

function addMemo(text: string) {
  if (text.trim().length === 0) {
    alertFormText.value = 'Please enter memo first';
    return;
  }
  memos.value.push({
    id: Date.now(),
    date: new Date().toLocaleDateString(),
    text,
    backgroundColor: getRandomColor()
  });
  memoText.value = '';
  formShow.value = false;
  alertFormText.value = '';
}
function getRandomColor(): string {
  return `#${Math.floor(Math.random() * 16777215).toString(16)}`;
}

function deleteMemo(id: number) {
  memos.value = memos.value.filter((e) => e.id !== id);
}
</script>
<template>
  <main>
    <div v-if="formShow" class="form-overlay">
      <div class="form-overlay__form-modal">
        <p v-if="alertFormText" style="color: red">{{ alertFormText }}</p>
        <button class="form-modal__close-btn" @click="formShow = false">&times;</button>
        <textarea name="memo" id="memo" cols="30" rows="10" v-model="memoText"></textarea>
        <button class="form-modal__save-btn" @click="addMemo(memoText)">Save</button>
      </div>
    </div>
    <div class="container">
      <div class="container__card-menu">
        <div class="card-menu__header-card">
          <h1 class="header-card__title">Memo</h1>
          <button class="header-card__btn" @click="formShow = true">+</button>
        </div>
        <div class="container__card-container">
          <div
            class="card-container__card"
            :style="{ backgroundColor: memo.backgroundColor }"
            v-for="memo in memos"
            :key="memo.id"
          >
            <p>{{ memo.text }}</p>
            <div class="card__card-footer">
              <p>{{ memo.date }}</p>
              <button class="card-footer__btn-remove" @click="deleteMemo(memo.id)">X</button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </main>
</template>
<style scoped>
.container__card-container {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5em;
}
.card-container__card {
  width: 15em;
  height: 15em;
  padding: 0.5em;
  overflow: hidden;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}
main {
  min-height: 100svh;
  overflow: hidden;
}

.form-overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100vh;
  background-color: rgba(0, 0, 0, 0.77);
  z-index: 100;
  display: flex;
  justify-content: center;
  align-items: center;
}

.form-overlay__form-modal {
  width: 420px;
  background-color: white;
  border-radius: 5%;
  padding: 1.7em;
  position: relative;
  display: flex;
  flex-direction: column;
}

.form-modal__save-btn {
  padding: 0.5em;
  font-size: 1em;
  width: 100%;
  background-color: var(--card-title-color);
  border: none;
  cursor: pointer;
  border-radius: 5%;
  margin-top: 1em;
  color: white;
}

.form-modal__close-btn {
  position: absolute;
  top: 3px;
  right: 5px;
  width: 30px;
  height: 30px;
  background-color: transparent;
  border: none;
  font-size: 1.5em;
  cursor: pointer;
}

.header-card__title {
  font-size: 2.5em;
  font-weight: bold;
  margin-bottom: 25px;
  color: var(--card-title-color);
}
.header-card__btn {
  border: none;
  color: white;
  padding: 0.2em;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 2em;
  height: 2em;
  border-radius: 100%;
  background-color: var(--card-title-color);
  cursor: pointer;
}
.container {
  max-width: 900px;
  padding: 10px;
  margin: 0 auto;
}
.card-menu__header-card {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.card__card-footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.card-footer__btn-remove {
  color: white;
  font-weight: bold;
  border: none;
  background-color: transparent;
}
</style>
