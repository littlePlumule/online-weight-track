<script setup>
import Modal from './components/Modal.vue';
import List from './components/List.vue';
import TheHeader from './components/TheHeader.vue';
import { ref, watch, onMounted } from 'vue';

let chartData = null;

const chart = ref(null);
const showModal = ref(false);
const lists = ref([]);
const weights = ref([]);
const dates = ref([]);

function modalShow() {
  showModal.value = true;
}

function modalClose() {
  showModal.value = false;
}

function limitList(limit) {
  let result = []
  lists.value.map(list => {
    Object.keys(list).map(key => { 
      if(limit.includes(key))  { 
        result.push(list[key]);
      }
    });
  });
  switch (limit) {
    case 'date':
      dates.value = result;
      break;
    case 'weight':
      weights.value = result;
      break;
  }
}

function addList(list) {
  lists.value.push(list);
  lists.value.sort((a, b) => a.date.localeCompare(b.data));
  limitList('weight');
  limitList('date');

  chartData.data.labels = JSON.parse(JSON.stringify(dates.value))
  chartData.data.datasets[0].data = JSON.parse(JSON.stringify(weights.value))
  chartData.update()
}

function deleteList(id) {
  let targetIndex = lists.value.findIndex(list => list.id === id);
  lists.value.splice(targetIndex, 1);
  limitList('weight');
  limitList('date');

  chartData.data.labels = JSON.parse(JSON.stringify(dates.value))
  chartData.data.datasets[0].data = JSON.parse(JSON.stringify(weights.value))
  chartData.update()
}

watch(
  lists,
  value => {
    localStorage.setItem('lists', JSON.stringify(value));
    limitList('weight');
    limitList('date');
  },
  { deep: true }
);

watch(
  dates,
  value => {
    localStorage.setItem('dates', JSON.stringify(value));
  },
  { deep: true }
);

watch(
  weights,
  value => {
    localStorage.setItem('weights', JSON.stringify(value));
  },
  { deep: true }
);

onMounted(() => {
  lists.value = JSON.parse(localStorage.getItem('lists')) || [];
  dates.value = JSON.parse(localStorage.getItem('dates')) || [];
  weights.value = JSON.parse(localStorage.getItem('weights')) || [];

  chartData = new Chart(chart.value, {
    type: 'line',
    data: {
      labels: JSON.parse(JSON.stringify(dates.value)),
      datasets: [{
        label: '體重',
        data: JSON.parse(JSON.stringify(weights.value)),
        backgroundColor: 'rgba(22, 166, 156, .5)',
        borderColor: '#21978f',
        borderWidth: 1,
        pointHoverRadius: 6,
        tension: 0,
      }]
    },
  });
});
</script>

<template>
  <modal :modal="showModal" @close="modalClose" @add-list="addList"></modal>
  <div class="container">
    <the-header @show-modal="modalShow"></the-header>
    <main>
      <list :lists="lists" @delete-list="deleteList"></list>
      <canvas id="myChart" ref="chart"></canvas>       
    </main>
  </div>
</template>

<style scoped>
.container {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 850px;
  border-radius: 8px;
  overflow: hidden;
  background-color: var(--main-background);
}

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

main {
  display: flex;
}

#myChart {
  width: 100%;
  max-width: 600px;
  padding: 10px;
}
</style>
