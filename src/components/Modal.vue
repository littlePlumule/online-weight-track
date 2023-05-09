<script setup>
  import { v4 as uuidv4 } from 'uuid';
  import { ref } from 'vue';

  const props = defineProps(['modal']);
  const emit = defineEmits(['close', 'add-list']);

  const errorShow = ref(false);
  const inputWeight = ref('');
  const inputDate = ref('');

  function close() {
    inputWeight.value = '';
    inputDate.value = '';
    errorShow.value = false;
    emit('close');
  }

  function addList() {
    if (!inputWeight.value || !inputDate.value) {
      errorShow.value = true;
    } else {
      emit('add-list', {
        id: uuidv4(),
        weight: inputWeight.value,
        date: inputDate.value,
      });
      close();
    }
  }
</script>

<template>
  <div class="overlay" v-if="modal" @click.self="close"></div>
  <form class="modal" v-if="modal" @submit.prevent="addList">
    <label>
      體重
      <input 
        type="number" 
        placeholder="請輸入體重(kg)" 
        v-model.trim="inputWeight"> 
    </label>                 
    <label>
      日期
      <input 
        type="date" 
        placeholder="請選擇日期"
        v-model.trim="inputDate">     
    </label>
    <p v-if="errorShow">不得為空</p>
    <button type="submit" class="add">新增</button>        
    <button type="button" class="cancel" @click="close">取消</button>       
  </form>
</template>

<style scoped>
.overlay {
  position: fixed;
  height: 100%;
  width: 100%;
  background-color: var(--overlay);
  z-index: 3;
}

.modal {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 308px;
  background-color: var(--modal);
  border-radius: 8px;
  z-index: 4;
  padding: 15px;
}

label {
  color: white;
  display: block;
  font-size: 1.2rem;
}

label + label {
  margin-top: 10px;
}

input{
  display: block;
  width: 100%;
  border: none;
  outline: none;
  font-size: 1.2rem;
  padding: 4px;
  border-radius: 5px;
}

button {
  margin-top: 10px;
  width: 50%;
  font-size: 1.4rem;
  padding: 5px 0;
  cursor: pointer;
  border: none;
  outline: none;
  color: #fff;
}

p {
  background-color: var(--error);
  color: #fff;
  border-radius: 8px;
  font-size: 1rem;
  padding: 2px 0;
  font-weight: normal;
  text-align: center;
  margin-top: 10px;
}

.add {
  background-color: var(--add);
  border-top-left-radius: 5px;
  border-bottom-left-radius: 5px;
}

.add:hover {
  background-color: var(--add-hover);
}

.cancel {
  background-color: var(--cancel);
  border-top-right-radius: 5px;
  border-bottom-right-radius: 5px;
}

.cancel:hover {
  background-color: var(--cancel-hover);
}
</style>