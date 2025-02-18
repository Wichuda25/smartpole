<script setup>
import "@/assets/styles/thanku.scss";
import { ref, onMounted } from "vue";

const isLoading = ref(true);

onMounted(() => {
  setTimeout(() => {
    isLoading.value = false;
  }, 2000);
});
</script>

<template>
  <div class="bg">
    <div v-if="isLoading" class="loader-overlay">
      <div class="loader"></div>
    </div>
    <NuxtLink v-else to="/home">
      <div class="col-bg">
        <div class="col-thank-1">
          <img src="/img/icon/icon-thank/icon-thank1.png" alt="" />
        </div>
        <div class="col-thank-2">
          <img src="/img/icon/icon-thank/icon-thank2.png" alt="" />
        </div>
        <div class="col-thank-3">
          <img src="/img/icon/icon-thank/icon-thank3.png" alt="" />
        </div>
        <div class="col-thank-4">
          <img src="/img/icon/icon-thank/icon-thank4.png" alt="" />
        </div>
        <div class="col-thank-5">
          <img src="/img/icon/icon-thank/icon-thank5.png" alt="" />
        </div>
        <div class="col-thank-6">
          <img src="/img/icon/icon-thank/icon-thank6.png" alt="" />
        </div>
        <div class="col-thank-7">
          <img src="/img/icon/icon-thank/icon-thank7.png" alt="" />
        </div>
        <div class="col-thank-8">
          <img src="/img/icon/icon-thank/icon-thank8.png" alt="" />
        </div>
        <div class="col-text-thank">
          <h2>ขอบคุณ</h2>
          <h2>ที่ใช้บริการ</h2>
        </div>
      </div>
    </NuxtLink>
  </div>
</template>
<style>
.bg {
  background-color: #20b2aa;
}
</style>
