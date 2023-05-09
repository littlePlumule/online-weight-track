<script setup>
  const emit = defineEmits(['show-modal']);
  
  function modalShow() {
    emit('show-modal');
  }
</script>

<template>
  <header>
    <h1 class="title">Online Weight Tracking</h1>
    <button class="add" type="button" @click="modalShow">&#x2b; 新增資料
    </button>
  </header>
</template>

<style scoped>
header {
  width: 100%;
  padding: 5px 0;
  text-align: center;
  position: relative;
  background-color: var(--header);
}

.title {
  font-size: 2rem;
  color: var(--title);
}

.add {
  position: absolute;
  right: 5px;
  top: 0;
  bottom: 0;
  border: none;
  outline: none;
  background-color: transparent;
  cursor: pointer;
  font-size: 1.2rem;
  font-weight: bold;
}

.add:hover {
  color: var(--add);
}
</style>