<script setup>
import "@/assets/styles/screen-info.scss";
import { ref, computed } from "vue";

const showPopup = ref(false);
const popupTitle = ref("");
const popupValue = ref("");

const openPopup = (title) => {
  popupTitle.value = title;
  showPopup.value = true;
  popupValue.value = "";
};

const closePopup = () => {
  showPopup.value = false;
};

const saveValue = () => {
  console.log(`${popupTitle.value} saved with value: ${popupValue.value}`);
  closePopup();
};

// 🌡️ อัปเดตสีคลื่นตามอุณหภูมิ
const temperatures = computed(() =>
  Array.from({ length: 8 }, (_, i) => {
    const value = 35 + i;
    const y = 220 - i * 25; // จัดให้เว้นระยะห่างเหมาะสม
    return { value, y };
  })
);

const liquidHeight = computed(() => {
  const temp = parseFloat(popupValue.value) || 37;
  return Math.min(190, Math.max(10, (temp - 35) * 25));
});

const liquidY = computed(() => 220 - liquidHeight.value);

const liquidColor = computed(() => {
  const temp = parseFloat(popupValue.value) || 37;
  if (temp <= 36) return "#00bcd4"; // ฟ้า (เย็น)
  if (temp <= 37.5) return "#20b2aa"; // ฟ้าอมเขียว (ปกติ)
  if (temp <= 38.5) return "#ffa500"; // ส้ม (เริ่มร้อน)
  return "#ff4500"; // แดง (ไข้สูง)
});
</script>

<template>
  <div>
    <div class="col-bg">
      <div class="col-text">
        <h2>ข้อมูลคัดกรอง</h2>
      </div>
      <div class="col-info-1">
        <img src="/img/icon/icon-info/icon-info1.png" alt="" />
      </div>
      <div class="col-info-2">
        <img src="/img/icon/icon-info/icon-info2.png" alt="" />
      </div>
      <div class="col-info-3">
        <img src="/img/icon/icon-info/icon-info3.png" alt="" />
      </div>
      <div class="col-info-4">
        <img src="/img/icon/icon-info/icon-info4.png" alt="" />
      </div>
      <div class="col-info-5">
        <img src="/img/icon/icon-info/icon-info5.png" alt="" />
      </div>
      <div class="col-info-6" @click="openPopup('น้ำหนัก/ส่วนสูง')">
        <div class="col-img">
          <div class="col-text-img">
            <h3>160/80</h3>
          </div>
        </div>
      </div>

      <div class="col-info-7" @click="openPopup('อุณหภูมิ')">
        <div class="col-img">
          <div class="col-text-img">
            <h3>37 ํC</h3>
          </div>
        </div>
      </div>

      <div class="col-info-8" @click="openPopup('ออกซิเจน')">
        <div class="col-img">
          <div class="col-text-img">
            <h3>38 c</h3>
          </div>
        </div>
      </div>

      <div class="col-info-9" @click="openPopup('ความดัน')">
        <div class="col-img">
          <div class="col-text-img">
            <h3>39.9</h3>
          </div>
        </div>
      </div>
    </div>
    <div class="btn-seve">
      <NuxtLink to="/thanku">
        <button>บันทึก</button>
      </NuxtLink>
    </div>
    <div v-if="showPopup" class="popup-overlay">
      <div class="popup">
        <h2>{{ popupTitle }}</h2>
        <div class="col-img">
          <img
            v-if="popupTitle === 'น้ำหนัก/ส่วนสูง'"
            src="/img/popup/bmi.png"
            alt="น้ำหนัก/ส่วนสูง"
          />
          <!-- <img
            v-if="popupTitle === 'อุณหภูมิ'"
            src="/img/popup/weigh.png"
            alt="อุณหภูมิ"
          /> -->
          <div v-if="popupTitle === 'อุณหภูมิ'" class="thermometer-container">
            <svg
              viewBox="0 0 150 300"
              class="thermometer"
              xmlns="http://www.w3.org/2000/svg"
            >
              <!-- หลอดแก้ว -->
              <rect x="55" y="20" width="10" height="200" rx="5" fill="#ddd" />
              <circle cx="60" cy="240" r="20" fill="#ddd" />

              <!-- ของเหลวในปรอท -->
              <rect
                x="57"
                :y="liquidY"
                width="6"
                :height="liquidHeight"
                rx="3"
                :fill="liquidColor"
                class="liquid"
              />
              <circle
                cx="60"
                cy="240"
                r="18"
                :fill="liquidColor"
                class="liquid"
              />

              <!-- 🔥 ขีดตัวเลขอุณหภูมิด้านข้าง -->
              <g v-for="temp in temperatures" :key="temp.value">
                <!-- ขีด -->
                <line
                  x1="40"
                  x2="55"
                  :y1="temp.y"
                  :y2="temp.y"
                  stroke="#666"
                  stroke-width="2"
                />
                <!-- ตัวเลข -->
                <text x="10" :y="temp.y + 5" fill="#333" font-size="14px">
                  {{ temp.value }}°C
                </text>
              </g>
            </svg>
          </div>

          <img
            v-if="popupTitle === 'ออกซิเจน'"
            src="/img/popup/height.png"
            alt="ออกซิเจน"
          />
          <img
            v-if="popupTitle === 'ความดัน'"
            src="/img/popup/bmi.png"
            alt="ความดัน"
          />
        </div>
        <input v-model="popupValue" placeholder="Enter value" />
        <button @click="saveValue">บันทึก</button>
        <button class="btn-close" @click="closePopup">ปิด</button>
      </div>
    </div>
  </div>
</template>
<style scoped>
.popup-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 10;
}

.popup {
  background-color: white;
  padding: 20px;
  border-radius: 8px;

  text-align: center;
  font-size: 40px;
}
.popup img {
  width: 20%;
}
input {
  margin: 10px 0;
  padding: 8px;
  width: 90%;
}

button {
  margin: 5px;
  padding: 8px 25px;
  border: none;
  background-color: #20b2aa;
  color: white;
  cursor: pointer;
  font-size: 40px;
  border-radius: 10px;
}
.btn-close {
  background-color: #d43636;
}
.btn-close:hover {
  background-color: #e02020;
}
button:hover {
  background-color: #2fccc4;
}
.col-text {
  top: 5%;
  font-size: 60px;
}
.btn-seve {
  position: absolute;
  top: 0;
  bottom: 18%;
  left: 0;
  right: 0;
  display: flex;
  align-items: end;
  justify-content: center;
  z-index: 5;
}
@keyframes glow {
  0% {
    box-shadow: 0 0 10px rgba(32, 178, 170, 0.8);
    transform: scale(1);
  }
  50% {
    box-shadow: 0 0 30px rgba(32, 178, 170, 1);
    transform: scale(1.1);
  }
  100% {
    box-shadow: 0 0 10px rgba(32, 178, 170, 0.8);
    transform: scale(1);
  }
}

.btn-seve button {
  padding: 10px 1rem;
  font-size: 70px;
  width: 280px;
  height: 250px;
  border-radius: 50%;
  background-color: #20b2aa;
  color: white;
  border: none;
  cursor: pointer;
  animation: glow 2s infinite alternate;
}
.thermometer-container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 250px;
}

.thermometer {
  width: 100px;
  height: 250px;
}

.liquid {
  transition: all 0.5s ease-in-out;
}
</style>
