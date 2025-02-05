<template>
  <div class="container">
    <h1>ถอดรหัส Binary</h1>
    <p class="question">แปลงเลขฐานสองนี้เป็นข้อความ:</p>
    <p class="binary-question">{{ binaryQuestion }}</p>
    
    <textarea 
      v-model="binaryInput"
      placeholder="วาง คำตอบ ที่นี่..."
      class="binary-input"
      @input="resetResult"
    ></textarea>
    <button @click="convertBinary" class="convert-btn">แปลงรหัส</button>
    
    <div v-if="convertedText" class="result-box">
      <p>ผลลัพธ์: {{ convertedText }}</p>
      <transition name="fade">
        <div v-if="isCorrect" class="animation-text">
          ก็ทำได้นี้หว่า ~
        </div>
        <div v-else-if="isIncorrect" class="error-text">
          ผิดนะ ลองใหม่อีกครั้ง
        </div>
      </transition>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      binaryQuestion: '01000110 01110010 01100001 01101110 01101011 00101101 01010011 01101111 00101101 01000110 01110101 01100011 01101011 01101011 01101001 01101110 01100111 00101101 01001000 01100001 01101110 01100100 01110011 01101111 01101101 01100101 00101101 01100010 01110010 01101001 01101110 01100111 00101101 01100001 00101101 01100001 01101110 01110011 01110111 01100101',
      binaryInput: '',
      convertedText: '',
      isCorrect: false,
      isIncorrect: false,
      correctAnswer: 'Frank-So-Fuckking-Handsome-bring-a-answer'
    }
  },
  methods: {
    convertBinary() {
      // Trim whitespace and remove extra spaces
      const cleanInput = this.binaryInput.trim().replace(/\s+/g, ' ');
      
      try {
        // Split and convert binary to text
        const binaryArray = cleanInput.split(' ');
        this.convertedText = binaryArray
          .map(bin => {
            // Validate binary input
            if (!/^[01]{8}$/.test(bin)) {
              throw new Error('Invalid binary');
            }
            return String.fromCharCode(parseInt(bin, 2));
          })
          .join('');
        
        // Check correctness
        this.isCorrect = this.convertedText === this.correctAnswer;
        this.isIncorrect = !this.isCorrect;

        // Auto-clear on incorrect
        if (this.isIncorrect) {
          this.shakeErrorAnimation();
          setTimeout(() => {
            this.convertedText = '';
            this.isIncorrect = false;
          }, 2000);
        }
      } catch (error) {
        this.convertedText = 'เกิดข้อผิดพลาดในการแปลง';
        this.isCorrect = false;
        this.isIncorrect = true;
      }
    },
    resetResult() {
      this.convertedText = '';
      this.isCorrect = false;
      this.isIncorrect = false;
    },
    shakeErrorAnimation() {
      const input = document.querySelector('.binary-input');
      input.classList.add('shake');
      setTimeout(() => {
        input.classList.remove('shake');
      }, 500);
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
.question {
  font-size: 1.2rem;
  font-weight: bold;
}
.binary-question {
  background: #000000;
  color: #000000;
  padding: 1rem;
  border-radius: 10px;
  display: inline-block;
  font-family: monospace;
  font-size: 1rem;
  word-break: break-word;
  max-width: 90%;
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
  transition: all 0.3s;
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
  color: rgb(0, 0, 0);
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
.error-text {
  font-size: 1.5rem;
  color: #ff0000;
  margin-top: 1rem;
}
@keyframes jump {
  0% { transform: translateY(0); }
  50% { transform: translateY(-10px); }
  100% { transform: translateY(0); }
}
@keyframes shake {
  0%, 100% { transform: translateX(0); }
  10%, 30%, 50%, 70%, 90% { transform: translateX(-10px); }
  20%, 40%, 60%, 80% { transform: translateX(10px); }
}
.shake {
  animation: shake 0.5s;
}
.fade-enter-active, .fade-leave-active {
  transition: opacity 0.5s;
}
.fade-enter, .fade-leave-to {
  opacity: 0;
}
</style>