<template>
  <div class="decoder-container">
    <div class="glass-card">
      <h1>🔍 อยากเข้าไปดูทำโจทย์ให้</h1>
      <p class="subtitle">แปลงรหัสฐานสองเป็นข้อความลับ</p>
      
      <div class="binary-display">
        <div class="binary-label">รหัสที่ต้องถอด:</div>
        <code class="binary-code">{{ binaryQuestion }}</code>
      </div>
      
      <textarea 
        v-model="binaryInput"
        placeholder="วางรหัสฐานสองที่นี่..."
        class="binary-input"
        @input="resetResult"
      ></textarea>
      
      <button 
        @click="convertBinary" 
        class="decode-button"
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
        const correctAnswer = 'แ'
        const binaryArray = this.binaryInput.trim().split(/\s+/)
        
        this.convertedText = binaryArray
          .map(bin => String.fromCharCode(parseInt(bin, 2)))
          .join('')

        // Normalize both strings before comparison
        const normalizedInput = this.normalizeString(this.convertedText)
        const normalizedCorrect = this.normalizeString(correctAnswer)

        this.isCorrect = normalizedInput === normalizedCorrect
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
    normalizeString(str) {
      return str
        .toLowerCase()
        .replace(/[^a-z]/g, '')
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
.decoder-container {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  font-family: 'Arial', sans-serif;
  margin: 0;
  padding: 0;
}

.glass-card {
  background: rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(10px);
  border-radius: 20px;
  border: 1px solid rgba(255, 255, 255, 0.2);
  box-shadow: 0 15px 35px rgba(0, 0, 0, 0.1);
  padding: 2rem;
  width: 90%;
  max-width: 500px;
  text-align: center;
  transition: all 0.3s ease;
}

h1 {
  color: white;
  margin-bottom: 10px;
  font-size: 2rem;
  text-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.subtitle {
  color: rgba(255, 255, 255, 0.7);
  margin-bottom: 20px;
  font-size: 1rem;
}

.binary-display {
  margin-bottom: 20px;
}

.binary-label {
  color: white;
  margin-bottom: 10px;
}

.binary-code {
  background: rgba(0, 0, 0, 0.2);
  color: white;
  padding: 10px;
  border-radius: 10px;
  font-family: monospace;
  word-break: break-all;
  font-size: 0.9rem;
  display: block;
}

.binary-input {
  width: 100%;
  height: 150px;
  padding: 15px;
  margin-bottom: 15px;
  background: rgba(255, 255, 255, 0.1);
  border: 2px dashed rgba(255, 255, 255, 0.3);
  border-radius: 10px;
  resize: none;
  color: white;
  font-size: 1rem;
  transition: all 0.3s ease;
}

.binary-input::placeholder {
  color: rgba(255, 255, 255, 0.5);
}

.decode-button {
  background: linear-gradient(to right, #6a11cb 0%, #2575fc 100%);
  color: white;
  border: none;
  padding: 12px 25px;
  border-radius: 25px;
  cursor: pointer;
  transition: all 0.3s ease;
  font-size: 1rem;
}

.decode-button:hover {
  transform: scale(1.05);
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.2);
}

.decode-button:disabled {
  background: rgba(255, 255, 255, 0.2);
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
  background: rgba(76, 175, 80, 0.3);
  color: white;
}

.error-box {
  background: rgba(244, 67, 54, 0.3);
  color: white;
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