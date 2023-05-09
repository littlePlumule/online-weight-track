<script setup>
  const props = defineProps(['lists']);
  const emit = defineEmits(['delete-list']);

  function deleteList(id) {
    emit('delete-list', id);
  }
</script>

<template>
  <ul>
    <li v-for="list of lists" :key="list.id">
      <div class="time">{{ list.date }}</div>
      <div class="weight">{{ list.weight }}kg</div>
      <button class="delete" type="button" @click="deleteList(list.id)">&#x2716;</button>
    </li>
  </ul>
</template>

<style scoped>
ul {
  flex-grow: 1;
  background-color: var(--list);
  padding: 10px 0;
  height: 300px;
  overflow: auto;
}

ul::-webkit-scrollbar {
  width: 7px;
}

ul::-webkit-scrollbar-button {
  background: transparent;  
  border-radius: 4px;  
}

ul::-webkit-scrollbar-track-piece {
  background: transparent;  
}

ul::-webkit-scrollbar-thumb {
  border-radius: 4px;  
  background-color: rgba(0, 0, 0, 0.4);
  border: 1px solid slategrey;  
}

ul::-webkit-scrollbar-track {
  box-shadow: transparent;  
}

li {
  display: flex;
  justify-content: space-evenly;
  align-items: center;
  font-size: 1.2rem;
  padding: 10px;
}

li:hover {
  background-color: var(--li-hover);
}

li + li {
  margin-top: 10px;
}

.delete {
  border: none;
  outline: none;
  background-color: transparent;
  cursor: pointer;
  font-size: 1.3rem;
  color: var(--cancel);
}

.delete:hover {
  color: var(--danger);
}
</style>