<template>
  <div class="container">
    <QRGenerator />
  </div>
</template>

<script setup>
import A from './components/QRGenerator.vue';
</script>

<style>
*, *::before, *::after {
  box-sizing: border-box;
}

html, body {
  width: 100%;
  min-height: 100vh;
  margin: 0;
  padding: 0;
  overflow-x: hidden;
  /* ควบคุมการซูมบนมือถือ */
  touch-action: manipulation;
}

body {
  background: linear-gradient(45deg, #ff9a9e, #fad0c4, #ffdde1, #f9ecee);
  background-size: 400% 400%;
  animation: gradientBG 15s ease infinite;
  background-attachment: fixed;
  /* เพิ่มการป้องกันการเลือกข้อความ */
  user-select: none;
}

.container {
  width: 100%;
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
  /* ทำให้ container เป็น flex container */
  display: flex;
  flex-direction: column;
  align-items: center;
}

/* ปรับขนาดสำหรับแท็บเล็ต */
@media (max-width: 1024px) {
  .container {
    padding: 15px;
    transform: scale(0.95);
  }
}

/* ปรับขนาดสำหรับมือถือแนวตั้ง */
@media (max-width: 768px) {
  html {
    /* ควบคุม font-size พื้นฐาน */
    font-size: 14px;
  }
  
  .container {
    padding: 10px;
    transform: scale(0.85);
    min-height: 90vh;
  }
}

/* ปรับขนาดสำหรับมือถือขนาดเล็ก */
@media (max-width: 500px) {
  html {
    font-size: 12px;
  }
  
  .container {
    padding: 5px;
    transform: scale(0.75) translateY(-5%);
    min-height: 80vh;
  }
  
  body {
    background-size: 300% 300%;
  }
}

@keyframes gradientBG {
  0% {
    background-position: 0% 50%;
  }
  50% {
    background-position: 100% 50%;
  }
  100% {
    background-position: 0% 50%;
  }
}
</style>