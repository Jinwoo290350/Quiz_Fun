<template>
  <div class="container">
    <h1>ถอดรหัส Binary</h1>
    <textarea 
      v-model="binaryInput"
      placeholder="วาง binary ที่นี่..."
      class="binary-input"
    ></textarea>
    <button @click="convertBinary" class="convert-btn">แปลงรหัส</button>
    
    <div v-if="convertedText" class="result-box">
      <p>ผลลัพธ์: {{ convertedText }}</p>
      <transition name="fade">
        <div v-if="isCorrect" class="animation-text">
          ก็ทำได้นี้หว่า ~
        </div>
      </transition>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      binaryInput: '',
      convertedText: '',
      isCorrect: false
    }
  },
  methods: {
    convertBinary() {
      const binaryArray = this.binaryInput.trim().split(' ')
      this.convertedText = binaryArray
        .map(bin => String.fromCharCode(parseInt(bin, 2)))
        .join('')
      
      this.isCorrect = this.convertedText === 'Frank-So-Fuckking-Handsome-bring-a-answer'
    }
  }
}
</script>

<style scoped>
.container {
  padding: 2rem;
  background: linear-gradient(45deg, #ff9a9e, #fad0c4);
  min-height: 100vh;
  text-align: center;
  font-family: 'Kanit', sans-serif;
}

.binary-input {
  width: 80%;
  height: 150px;
  padding: 1rem;
  margin: 1rem 0;
  background: #ffdde1;
  border: 2px solid #f9ecee;
  border-radius: 10px;
  font-size: 1.2rem;
}

.convert-btn {
  background: #f9ecee;
  padding: 0.8rem 2rem;
  border: none;
  border-radius: 25px;
  cursor: pointer;
  font-size: 1.1rem;
  transition: all 0.3s;
}

.convert-btn:hover {
  transform: scale(1.05);
  background: #ff9a9e;
  color: white;
}

.result-box {
  margin: 2rem auto;
  padding: 1.5rem;
  background: #f9ecee;
  border-radius: 15px;
  max-width: 600px;
}

.animation-text {
  font-size: 1.5rem;
  color: #ff9a9e;
  margin-top: 1rem;
  animation: jump 0.8s infinite;
}

@keyframes jump {
  0% { transform: translateY(0); }
  50% { transform: translateY(-10px); }
  100% { transform: translateY(0); }
}

.fade-enter-active, .fade-leave-active {
  transition: opacity 0.5s;
}
.fade-enter, .fade-leave-to {
  opacity: 0;
}
</style>