<script setup>
import "@/assets/styles/index.scss";
import { ref, onMounted } from "vue";

const isLoading = ref(true);

onMounted(() => {
  setTimeout(() => {
    isLoading.value = false;
  }, 2000);
});
</script>

<template>
  <div>
    <div v-if="isLoading" class="loader-overlay">
      <div class="loader"></div>
    </div>
    <NuxtLink v-else to="/home">
      <div class="col-bg">
        <div class="col-1">
          <img src="/img/icon/icon1.png" alt="" />
        </div>
        <div class="col-2">
          <img src="/img/icon/icon2.png" alt="" />
        </div>
        <div class="col-3">
          <img src="/img/icon/icon3.png" alt="" />
        </div>
        <div class="col-4">
          <img src="/img/icon/icon4.png" alt="" />
        </div>
        <div class="col-5">
          <img src="/img/icon/icon5.png" alt="" />
        </div>
        <div class="col-6">
          <img src="/img/icon/icon6.png" alt="" />
        </div>
        <div class="col-7">
          <img src="/img/icon/icon7.png" alt="" />
        </div>
        <div class="col-8">
          <img src="/img/icon/icon8.png" alt="" />
        </div>
        <div class="col-welcome">
          <img src="/img/welcome.png" alt="" />
        </div>
      </div>
    </NuxtLink>
  </div>
</template>
