<script setup>
import "@/assets/styles/index.scss";
import { ref, onMounted, onUnmounted } from "vue";
import { io } from "socket.io-client";

const isLoading = ref(true);
const cardData = ref(null);

// const socket = io("http://localhost:3001");

onMounted(() => {
  setTimeout(() => {
    isLoading.value = false;
  }, 2000);

  // socket.on("cardData", (data) => {
  //   console.log("ข้อมูลบัตรประชาชน:", data);
  //   cardData.value = data;
  //   localStorage.setItem("cardData", JSON.stringify(data));
  // });
});

onUnmounted(() => {
  socket.disconnect();
});
</script>
<template>
  <div>
    <div v-if="isLoading" class="loader-overlay">
      <div class="loader"></div>
    </div>

    <NuxtLink v-else to="/info">
      <div class="col-bg">
        <div class="col-9">
          <img src="/img/icon/icon9.png" alt="" />
        </div>
        <div class="col-10">
          <img src="/img/icon/icon10.png" alt="" />
        </div>
        <div class="col-11">
          <img src="/img/icon/icon11.png" alt="" />
        </div>
        <div class="col-12">
          <img src="/img/icon/icon12.png" alt="" />
        </div>
        <div class="col-13">
          <img src="/img/icon/icon13.png" alt="" />
        </div>
        <div class="col-14">
          <img src="/img/icon/icon14.png" alt="" />
        </div>
        <div class="col-15">
          <img src="/img/icon/icon15.png" alt="" />
        </div>
        <div class="col-text">
          <h2>
            กรุณาสอดบัตร <br />
            เพื่อเข้ารับบริการ
          </h2>
          <!-- <p v-if="cardData">
            <strong>ชื่อ:</strong> {{ cardData.name }}<br />
            <strong>เลขบัตร:</strong> {{ cardData.cardID }}
          </p> -->
        </div>
      </div>
    </NuxtLink>
  </div>
</template>
