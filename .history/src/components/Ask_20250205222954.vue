<template>
  <div class="container">
    <div class="card">
      <h1>ถอดรหัสไบนารี</h1>
      <p class="subtitle">แปลงรหัสฐานสองเป็นข้อความ</p>
      
      <div class="binary-section">
        <div class="binary-label">รหัสที่ต้องถอด:</div>
        <p class="binary-code">{{ binaryQuestion }}</p>
      </div>
      
      <textarea 
        v-model="binaryInput"
        placeholder="วางรหัสฐานสองที่นี่..."
        class="binary-input"
        @input="resetResult"
      ></textarea>
      
      <button 
        @click="convertBinary" 
        class="convert-btn"
        :disabled="!binaryInput.trim()"
      >
        ถอดรหัส
      </button>
      
      <div v-if="convertedText" class="result-section">
        <div 
          class="result-box" 
          :class="{
            'success-box': isCorrect, 
            'error-box': isIncorrect
          }">
          <p class="result-text">
            {{ resultMessage }}
          </p>
        </div>
      </div>
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
      isIncorrect: false
    }
  },
  computed: {
    resultMessage() {
      if (this.isCorrect) return 'ถูกต้อง! 🎉'
      if (this.isIncorrect) return 'ลองใหม่อีกครั้ง 🤔'
      return this.convertedText
    }
  },
  methods: {
    convertBinary() {
      try {
        const correctAnswer = 'Frank-Fuck-So-Handsome-bring-a-answer'
        const binaryArray = this.binaryInput.trim().split(/\s+/)
        
        this.convertedText = binaryArray
          .map(bin => String.fromCharCode(parseInt(bin, 2)))
          .join('')

        this.isCorrect = this.convertedText === correctAnswer
        this.isIncorrect = !this.isCorrect

        if (this.isIncorrect) {
          this.shakeErrorAnimation()
          setTimeout(() => {
            this.resetResult()
          }, 2000)
        }
      } catch (error) {
        this.isIncorrect = true
        this.convertedText = 'เกิดข้อผิดพลาด'
      }
    },
    resetResult() {
      this.convertedText = ''
      this.isCorrect = false
      this.isIncorrect = false
    },
    shakeErrorAnimation() {
      const input = document.querySelector('.binary-input')
      input.classList.add('shake')
      setTimeout(() => {
        input.classList.remove('shake')
      }, 500)
    }
  }
}
</script>

<style scoped>
.container {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  background: linear-gradient(135deg, #ff6b6b, #4ecdc4);
  font-family: 'Kanit', Arial, sans-serif;
}

.card {
  background: white;
  border-radius: 20px;
  box-shadow: 0 15px 35px rgba(0,0,0,0.1);
  padding: 2rem;
  width: 90%;
  max-width: 500px;
  text-align: center;
  transition: all 0.3s ease;
}

.card:hover {
  transform: translateY(-10px);
  box-shadow: 0 20px 40px rgba(0,0,0,0.15);
}

h1 {
  color: #333;
  margin-bottom: 10px;
  font-size: 1.8rem;
}

.subtitle {
  color: #666;
  margin-bottom: 20px;
  font-size: 1rem;
}

.binary-section {
  margin-bottom: 20px;
}

.binary-label {
  font-weight: bold;
  color: #333;
  margin-bottom: 10px;
}

.binary-code {
  background: #f0f0f0;
  padding: 10px;
  border-radius: 10px;
  font-family: monospace;
  word-break: break-all;
  font-size: 0.9rem;
}

.binary-input {
  width: 100%;
  height: 150px;
  padding: 15px;
  margin-bottom: 15px;
  border: 2px dashed #4ecdc4;
  border-radius: 10px;
  resize: none;
  font-size: 1rem;
  transition: all 0.3s ease;
}

.binary-input:focus {
  outline: none;
  border-color: #ff6b6b;
}

.convert-btn {
  background: #4ecdc4;
  color: white;
  border: none;
  padding: 12px 25px;
  border-radius: 25px;
  cursor: pointer;
  transition: all 0.3s ease;
  font-size: 1rem;
}

.convert-btn:hover {
  background: #45b7aa;
  transform: scale(1.05);
}

.convert-btn:disabled {
  background: #ccc;
  cursor: not-allowed;
}

.result-section {
  margin-top: 20px;
}

.result-box {
  padding: 15px;
  border-radius: 10px;
  font-size: 1rem;
}

.success-box {
  background: rgba(76, 175, 80, 0.1);
  color: #4CAF50;
}

.error-box {
  background: rgba(244, 67, 54, 0.1);
  color: #F44336;
}

@keyframes shake {
  0%, 100% { transform: translateX(0); }
  10%, 30%, 50%, 70%, 90% { transform: translateX(-10px); }
  20%, 40%, 60%, 80% { transform: translateX(10px); }
}

.shake {
  animation: shake 0.5s;
}
</style>