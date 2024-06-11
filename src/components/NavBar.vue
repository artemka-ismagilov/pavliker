<script setup></script>

<template>
  <div class="profile">
    <div>
      <h1 class="nameSite">Pavlikerㅤ</h1>
    </div>

    <div class="marginPhoto">
      <img src="../assets/avatar.jpg" alt="" class="photoMy" />
      <h1 class="userNameVisual">Goga228</h1>
    </div>
  </div>
</template>

<style scoped>
.profile {
  display: flex;
  align-items: center; /* Центрируем элементы по вертикали */
  justify-content: space-between;
  color: white;
  font-family: Verdana, Geneva, Tahoma, sans-serif;
  font-size: 11px;
  border-bottom: 1px solid black;
}
.userNameVisual{
  margin-top: 10px;
}
.nameSite {
  border-right: 1px solid black;
  /* padding-right: 10px;  */
  margin-top: 10px;
}
.photoMy {
  height: 40px;
  width: 40px;
  border-radius: 50%;
  margin-right: 5px;
}
.marginPhoto {
  display: flex;
  align-items: center; /* Центрируем элементы по вертикали */
}
</style>
