<!-- components/StepMenu.vue -->
<template>
  <nav class="step-menu" ref="containerRef">
    <ul>
      <li
        v-for="(step, index) in steps"
        :key="index"
        :class="{ active: index === currentStep }"
        @click="goToStep(index)"
      >
        {{ step.label }}
      </li>
    </ul>
    <!-- Active Indicator -->
    <div class="indicator" :style="indicatorStyle"></div>
  </nav>
</template>

<script setup>
import { ref, onMounted, watch, onBeforeUnmount } from "vue";
import { useRouter, useRoute } from "vue-router";

const steps = [
  { label: "น้ำหนักส่วนสูง", route: "/weight-height" },
  { label: "อุณหภูมิ", route: "/temperature" },
  { label: "ออกซิเจน", route: "/oxygen" },
  { label: "ความดัน", route: "/pressure" },
];

const router = useRouter();
const route = useRoute();
const currentStep = ref(
  steps.findIndex((step) => step.route === route.path) || 0
);

// อ้างอิง container สำหรับวัดตำแหน่ง
const containerRef = ref(null);
const indicatorStyle = ref({ left: "0px", width: "0px" });

// ฟังก์ชันอัปเดตตำแหน่งของ indicator
const updateIndicator = () => {
  if (!containerRef.value) return;
  const items = containerRef.value.querySelectorAll("li");
  const activeItem = items[currentStep.value];
  if (activeItem) {
    indicatorStyle.value.left = activeItem.offsetLeft + "px";
    indicatorStyle.value.width = activeItem.offsetWidth + "px";
  }
};

// รันโค้ดเฉพาะเมื่ออยู่ใน client-side
onMounted(() => {
  updateIndicator();
  if (typeof window !== "undefined") {
    window.addEventListener("resize", updateIndicator);
  }
});

// อัปเดต indicator เมื่อเปลี่ยน step
watch(currentStep, updateIndicator);

// ลบ event listener เมื่อ component ถูกทำลาย
onBeforeUnmount(() => {
  if (typeof window !== "undefined") {
    window.removeEventListener("resize", updateIndicator);
  }
});

const goToStep = (index) => {
  currentStep.value = index;
  router.push(steps[index].route);
};
</script>

<style scoped>
.step-menu {
  position: relative;
  margin: 20px 0;
}
.step-menu ul {
  display: flex;
  list-style: none;
  padding: 0;
  margin: 0;
}
.step-menu li {
  cursor: pointer;
  padding: 10px 20px;
  transition: color 0.3s;
}
.step-menu li.active {
  font-weight: bold;
  color: #3490dc;
}

/* Active indicator style */
.indicator {
  position: absolute;
  bottom: 0;
  height: 2px;
  background-color: #3490dc;
  transition: left 0.3s, width 0.3s;
}
</style>
