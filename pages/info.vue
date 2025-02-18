<script setup>
import "@/assets/styles/index.scss";
import { ref, onMounted } from "vue";

const isLoading = ref(true);
// const cardData = ref(null);

onMounted(() => {
  // const storedData = localStorage.getItem("cardData");
  // if (storedData) {
  //   cardData.value = JSON.parse(storedData);
  // }
  setTimeout(() => {
    isLoading.value = false;
  }, 2000);
});
</script>
<template>
  <div class="">
    <div v-if="isLoading" class="loader-overlay">
      <div class="loader"></div>
    </div>

    <div v-else class="col-bg">
      <div class="col-16">
        <img src="/img/icon/icon16.png" alt="" />
      </div>
      <div class="col-17">
        <img src="/img/icon/icon17.png" alt="" />
      </div>
      <div class="col-18">
        <img src="/img/icon/icon18.png" alt="" />
      </div>
      <div class="col-19">
        <img src="/img/icon/icon19.png" alt="" />
      </div>
      <div class="col-text-info">
        <h2>ข้อมูลผู้เข้ารับบริการ</h2>
        <img src="/img/img-id.png" alt="" />
        <template v-if="cardData">
          <p><strong>เลขบัตร:</strong> {{ cardData.cardID }}</p>
          <p><strong>ชื่อ:</strong> {{ cardData.name }}</p>
          <p><strong>อายุ:</strong> {{ cardData.age }} ปี</p>
          <p><strong>สิทธิ:</strong> {{ cardData.insurance }}</p>
          <p><strong>โรงพยาบาล:</strong> {{ cardData.hospital }}</p>
        </template>
        <!-- <p v-else>กรุณาสอดบัตรเพื่อโหลดข้อมูล</p> -->
        <p>1400700220944</p>
        <p>ธนธรณ์ มีเสาเพ็ชร</p>
        <p>อายุ 24 ปี 4 เดือน 5 วัน</p>
        <p>(SSS) สิทธิประกันสังคม</p>
        <p>รพ.ขอนแก่น</p>
        <NuxtLink to="/screen-info">
          <button
            style="
              font-size: 50px;
              background-color: #20b2aa;
              color: white;
              padding: 10px 50px;
              border-radius: 10px;
              border: none;
            "
          >
            คัดกรอง
          </button>
        </NuxtLink>
      </div>
    </div>
  </div>
</template>
<style>
body {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
  background-color: #e4fbff;
}
</style>
