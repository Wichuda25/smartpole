<script setup>
import "@/assets/styles/screen-info.scss";
import NumericKeypad from "@/components/NumericKeypad.vue";
import { ref, computed } from "vue";

const showPopup = ref(false);
const popupTitle = ref("");
const popupValue = ref("");
const height = ref(160);
const weight = ref(80);

const heightPercentage = computed(() => (height.value / 250) * 100);
const weightAngle = computed(() => (weight.value / 150) * 90 - 45);
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

const waveColor = computed(() => {
  const oxygen = parseFloat(popupValue.value) || 98;
  if (oxygen > 95) return "#20b2aa"; // เขียวฟ้า (ปกติ)
  if (oxygen > 90) return "#ffa500"; // ส้ม (เริ่มต่ำ)
  return "#ff4500"; // แดง (อันตราย)
});

const updateHeightWeight = () => {
  const values = inputValue.value.split("/");
  if (values.length === 2) {
    height.value = values[0];
    weight.value = values[1];
  }
  inputValue.value = "";
};

const updateTemperature = () => {
  temperature.value = inputValue.value;
  inputValue.value = "";
};

const updateOxygen = () => {
  oxygenLevel.value = inputValue.value;
  inputValue.value = "";
};

const updateBloodPressure = () => {
  bloodPressure.value = inputValue.value;
  inputValue.value = "";
};

</script>

<script>
export default {
  data() {
    return {
      inputValue: "",
      numbers: [1, 2, 3, 4, 5, 6, 7, 8, 9, 0],
    };
  },
  methods: {
    addNumber(num) {
      this.inputValue += num;
    },
    clearInput() {
      this.inputValue = "";
    },
    confirmInput() {
      this.$emit("input-confirmed", this.inputValue);
    },
  },
};
</script>
<template>
  <div>
    <div class="col-bg">
      <div class="col-text">
        <h2 style="text-align: center;">ข้อมูลคัดกรอง</h2>
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
            <p style="font-size:45px; line-height: 0;">น้ำหนัก/ส่วนสูง</p>
            <h3 style="margin: 0.5rem;">160/80</h3>
          </div>
        </div>
      </div>

      <div class="col-info-7" @click="openPopup('ออกซิเจน')">
        <div class="col-img">
          <div class="col-text-img">
            <p style="font-size:35px; line-height: 0;">ออกซิเจนในเลือด</p>
            <h3 style="margin: 0.5rem;">100%</h3>

          </div>
        </div>
      </div>

      <div class="col-info-8" @click="openPopup('อุณหภูมิ')">
        <div class="col-img">
          <div class="col-text-img">
            <p style="font-size:60px; line-height: 0;">อุณหภูมิ</p>
            <h3 style="margin: 0.5rem;">38.50°C</h3>
          </div>
        </div>
      </div>

      <div class="col-info-9" @click="openPopup('ความดัน')">
        <div class="col-img">
          <div class="col-text-img">
            <p style="font-size:60px; line-height: 0;">ความดัน</p>
            <h3 style="margin: 0.5rem;">110/80</h3>
            <h6 style="margin: 0.5rem 0 0.5rem -40px;"><img src="/img/Heart.gif" alt="" width="100">100</h6>
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
          <!-- <img v-if="popupTitle === 'น้ำหนัก/ส่วนสูง'" src="/img/popup/height.png" alt="น้ำหนัก/ส่วนสูง" /> -->
          <div v-if="popupTitle === 'น้ำหนัก/ส่วนสูง'" class="wh-container">
            <!-- ไม้บรรทัดวัดความสูง -->
            <!-- <div class="height-box"> -->
            <!-- <div class="ruler">
                <div class="height-bar" :style="{ height: heightPercentage + '%' }"></div>
              </div> -->
            <!-- <p class="height-text">{{ height }} cm</p> -->
            <!-- </div> -->
            <div class="height-box">
              <p class="height-text" contenteditable="true" @input="updateHeight" @blur="validateHeight">
                {{ height }} cm
              </p>
            </div>
            <!-- เครื่องชั่งน้ำหนัก -->
            <div class="scale-box">
              <!-- <div class="scale">
                <div class="scale-needle" :style="{ transform: `rotate(${weightAngle}deg)` }"></div>
              </div> -->
              <p class="weight-text">{{ weight }} kg</p>
            </div>
            <!-- <NumericKeypad v-model="inputValue" @confirm="updateHeightWeight" /> -->
          </div>

          <!-- <img
            v-if="popupTitle === 'อุณหภูมิ'"
            src="/img/popup/weigh.png"
            alt="อุณหภูมิ"
          /> -->
          <div v-if="popupTitle === 'อุณหภูมิ'" class="thermometer-container">
            <svg viewBox="0 0 150 300" class="thermometer" xmlns="http://www.w3.org/2000/svg">
              <!-- หลอดแก้ว -->
              <rect x="55" y="20" width="10" height="200" rx="5" fill="#ddd" />
              <circle cx="60" cy="240" r="20" fill="#ddd" />

              <!-- ของเหลวในปรอท -->
              <rect x="57" :y="liquidY" width="6" :height="liquidHeight" rx="3" :fill="liquidColor" class="liquid" />
              <circle cx="60" cy="240" r="18" :fill="liquidColor" class="liquid" />

              <!-- 🔥 ขีดตัวเลขอุณหภูมิด้านข้าง -->
              <g v-for="temp in temperatures" :key="temp.value">
                <!-- ขีด -->
                <line x1="40" x2="55" :y1="temp.y" :y2="temp.y" stroke="#666" stroke-width="2" />
                <!-- ตัวเลข -->
                <text x="10" :y="temp.y + 5" fill="#333" font-size="14px">
                  {{ temp.value }}°C
                </text>
              </g>
            </svg>
          </div>

          <!-- <img
            v-if="popupTitle === 'ออกซิเจน'"
            src="/img/popup/height.png"
            alt="ออกซิเจน"
          /> -->
          <div v-if="popupTitle === 'ออกซิเจน'" class="oxygen-container">
            <svg viewBox="0 0 300 100" class="ecg-wave">
              <path
                d="M0,50 L20,50 L30,10 L40,50 L60,50 L70,30 L80,50 L100,50 L110,70 L120,50 L140,50 L150,20 L160,50 L180,50 L190,30 L200,50 L220,50 L230,70 L240,50 L260,50 L270,20 L280,50 L300,50"
                :stroke="waveColor" stroke-width="3" fill="none" stroke-linecap="round">
                <animateTransform attributeName="transform" type="translate" from="0 0" to="-40 0" dur="1s"
                  repeatCount="indefinite" />
              </path>
            </svg>
            <h3 class="oxygen-text">{{ popupValue || "98" }}%</h3>
          </div>
          <!-- <img v-if="popupTitle === 'ความดัน'" src="/img/popup/bmi.png" alt="ความดัน" /> -->
          <div v-if="popupTitle === 'ความดัน'" class="bp-container">
            <svg viewBox="0 0 300 100" class="bp-wave">
              <path
                d="M0,50 L20,60 L40,30 L60,70 L80,40 L100,50 L120,30 L140,70 L160,50 L180,40 L200,60 L220,30 L240,70 L260,50 L280,40 L300,50"
                :stroke="waveColor" stroke-width="4" fill="none" stroke-linecap="round">
                <animateTransform attributeName="transform" type="translate" from="0 0" to="-50 0" dur="1s"
                  repeatCount="indefinite" />
              </path>
            </svg>
            <h3 class="bp-text">{{ popupValue || "120/80" }} mmHg</h3>
          </div>
        </div>
        <div class="keypad-container">

          <div class="keypad">
            <button v-for="num in numbers" :key="num" @click="addNumber(num)">
              {{ num }}
            </button>
            <button @click="clearInput" class="clear-btn">C</button>
            <button @click="confirmInput" class="confirm-btn">OK</button>
          </div>
        </div>
        <button class="btn-save" @click="saveValue">บันทึก</button>
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
  width: 60%;
}

input {
  margin: 10px 0;
  padding: 8px;
  width: 90%;
}

.btn-save,
.btn-close {
  margin: 5px;
  padding: 8px 25px;
  border: none;
  background-color: #20b2aa;
  color: white;
  cursor: pointer;
  font-size: 60px;
  border-radius: 10px;
}

.btn-close {
  background-color: #d43636;
}

.btn-close:hover {
  background-color: #e02020;
}

.btn-save:hover {
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
  width: 144px;
  height: 299px;
}

.liquid {
  transition: all 0.5s ease-in-out;
}

.oxygen-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 120px;
}

.ecg-wave {
  width: 100%;
  height: 100px;
}

.oxygen-text {
  font-size: 28px;
  font-weight: bold;
  color: white;
  text-shadow: 0 0 5px #000;
}

.wh-container {
  display: flex;
  gap: 20px;
  justify-content: center;
  align-items: center;
}

/* ---- ไม้บรรทัด ---- */
.height-box {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.ruler {
  width: 20px;
  height: 200px;
  background: linear-gradient(to bottom, #ddd, #bbb);
  position: relative;
  border-radius: 10px;
}

.height-bar {
  width: 100%;
  background: linear-gradient(to bottom, #20b2aa, #008080);
  position: absolute;
  bottom: 0;
  transition: height 0.5s ease-in-out;
}

.height-text {
  font-size: 24px;
  font-weight: bold;
  /* margin-top: 10px; */
  margin: 0;
}

/* ---- เครื่องชั่งน้ำหนัก ---- */
.scale-box {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.scale {
  width: 100px;
  height: 50px;
  background: #ddd;
  border-radius: 50px 50px 0 0;
  position: relative;
  overflow: hidden;
}

.scale-needle {
  width: 4px;
  height: 30px;
  background: red;
  position: absolute;
  bottom: 5px;
  left: 50%;
  transform-origin: bottom center;
  transition: transform 0.5s ease-in-out;
}

.weight-text {
  font-size: 24px;
  font-weight: bold;
  /* margin-top: 10px; */
  margin: 0;
}

.height-text {
  font-size: 120px;
}

.weight-text {
  font-size: 120px;
}

.keypad-container {
  text-align: center;
  margin-top: 20px;
}

.keypad-display {
  width: 100%;
  padding: 15px;
  font-size: 24px;
  text-align: center;
  margin-bottom: 10px;
  border: 2px solid #aaa;
  border-radius: 8px;
  background: #f8f8f8;
  font-weight: bold;
}

.keypad {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 10px;
  justify-content: center;
}

button {
  padding: 20px;
  font-size: 24px;
  border: none;
  background: #eee;
  cursor: pointer;
  border-radius: 10px;
  font-weight: bold;
  transition: background 0.2s ease-in-out;
}

button:hover {
  background: #ccc;
}

.clear-btn {
  background: orange;
  color: white;
}

.confirm-btn {
  background: green;
  color: white;
}
</style>
