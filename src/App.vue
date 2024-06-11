<script setup>
import { ref, onMounted, watch } from 'vue';
import NavBar from './components/NavBar.vue';
let count = ref(300);
let timeOut = ref(1500);
let interval;
let promocodeUsed = ref(false);
let superpromocodeUsed = ref(false);
let passiveUpLvl = ref(1);
let saleUpgrade = ref(300);
let countUpgrade = ref(0);
let highScore = ref(localStorage.getItem('highScore') || 0); // Получаем рекорд из localStorage

let code = 'popa';
let supercode = 'summer with Pavliker';
function promocode() {
  if (!promocodeUsed.value) {
    let userInput = prompt('Введите промокод:');
    if (userInput === code) {
      count.value += 1000;
      promocodeUsed.value = true;
      setTimeout(() => {
        // Устанавливаем таймер на 3 минуты
        promocodeUsed.value = false;
      }, 180000); // 180000 миллисекунд = 3 минуты
    }
    if (userInput === supercode) {
      count.value += 5000;
      superpromocodeUsed.value = false;
    }
  } else {
    alert('Промокод уже был использован. Попробуйте снова через 3 минуты.');
  }
}
function countUp() {
  if (count.value >= 50) {
    count.value++;
  }
}
function countPlus(num) {
  count.value += num;
}
function zeroCount() {
  count.value = 0;
}

function upgradeCount() {
  if (passiveUpLvl.value < 15) {
    if (count.value >= saleUpgrade.value) {
      count.value -= saleUpgrade.value; // Вычитаем стоимость улучшения из счета
      clearInterval(interval);
      timeOut.value -= 100;
      interval = setInterval(() => {
        countUp();
      }, timeOut.value);
      saleUpgrade.value += 300; // Увеличиваем стоимость следующего улучшения на 300
      passiveUpLvl.value++;
    } else {
      alert('Недостаточно средств для улучшения!');
    }
  } else {
    alert('Вы достигли максимального уровня пассивки.');
  }
}
function checkHighScore() {
  if (count.value > highScore.value) {
    highScore.value = count.value;
    localStorage.setItem('highScore', highScore.value); // Сохраняем новый рекорд в localStorage
  }
}
// Следим за изменением счета и проверяем рекорд
watch(count, () => {
  checkHighScore();
});
onMounted(() => {
  interval = setInterval(() => {
    countUp();
  }, timeOut.value);
});
</script>

<template>
  <NavBar />
  <div class="highScore">Ваш рекорд: {{ highScore }}</div>
  <div class="buttons">
    <button class="btnForCount1" @click="zeroCount()">Обнулить</button> <br />
    <button class="btnForCount2" @click="promocode">Ввести промокод</button
    ><br />
    <button
      class="btnForCount3"
      v-if="count >= saleUpgrade - 300"
      @click="upgradeCount()"
    >
      Улучшить
    </button>
    <br />
  </div>
  <div class="contPassive">
    <h1 class="titleSaleUpgrade">Стоимость улучшения: {{ saleUpgrade }}</h1>
    <h2 class="passiveCount">Уровень пассивки: {{ passiveUpLvl }}</h2>
  </div>
  <div class="globalBlock">
    <div class="titleMy">
      {{ count }}
    </div>
    <div class="buttonContainer">
      <button class="btnKrug" @click="countPlus(1)">+</button>
    </div>
  </div>
</template>

<style scoped>
* {
  font-family: 'Trebuchet MS', 'Lucida Sans Unicode', 'Lucida Grande',
    'Lucida Sans', Arial, sans-serif;
}
.globalBlock {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  height: 100vh;
}
.highScore {
  color: gold;
  font-size: 24px;
  /* position: absolute; */
  top: 10px;
  right: 10px;
  display: flex;
  justify-content: center;
}
.titleSaleUpgrade {
  color: rgb(0, 255, 0);
  margin: 0;
  font-size: 24px;
  margin-top: 240px;
  position: absolute;
}
.passiveCount {
  margin-top: 200px;
  position: static;
  color: white;
}
.contPassive {
  width: 400px;
  height: 30px;
  position: absolute;
}
.buttons {
  padding: 15px;
  position: absolute;
}
.titleMy {
  color: white;
  font-family: 'Lucida Sans', 'Lucida Sans Regular', 'Lucida Grande',
    'Lucida Sans Unicode', Geneva, Verdana, sans-serif;
  font-size: 64px;
}
.buttonContainer {
  display: flex;
  justify-content: center;
}
.btnForCount1 {
  margin-bottom: 5px;
  font-size: 24px;
  cursor: pointer;
}
.btnForCount1:hover {
  background-color: red;
  transition: 0.5s;
  color: white;
}
.btnForCount2 {
  margin-bottom: 5px;
  font-size: 24px;
  cursor: pointer;
}
.btnForCount3 {
  margin-bottom: 5px;
  font-size: 24px;
  cursor: pointer;
}
.btnKrug {
  width: 64px;
  height: 64px;
  border-radius: 50%;
  border: 1px solid black;
  cursor: pointer;
  font-size: 30px;
}
</style>
