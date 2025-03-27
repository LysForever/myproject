<template>
  <div class="basketball-game">
    <div class="court">
      <div class="basket"></div>
      <div class="player" :style="playerStyle">
        <div class="head"></div>
        <div class="body"></div>
        <div class="arms"></div>
        <div class="legs"></div>
      </div>
      <div class="ball" :style="ballStyle"></div>
    </div>
    <button 
      @mousedown="startPress" 
      @mouseup="releasePress" 
      @mouseleave="cancelPress"
      :disabled="isAnimating"
      :style="buttonStyle"
    >
      投篮
    </button>
    <div v-if="resultMessage" class="result-message">{{ resultMessage }}</div>
  </div>
</template>

<script>
export default {
  name: 'BasketballGame',
  data() {
    return {
      isAnimating: false,
      pressStartTime: null,
      pressDuration: 0,
      playerStyle: {
        left: '50px'
      },
      ballStyle: {
        left: '65px',
        bottom: '60px',
        transition: 'all 0.3s ease'
      },
      buttonStyle: {
        backgroundColor: '#4CAF50'
      },
      resultMessage: ''
    }
  },
  methods: {
    startPress() {
      if (this.isAnimating) return;
      this.pressStartTime = new Date().getTime();
      this.updateButtonAnimation();
    },
    releasePress() {
      if (this.isAnimating || this.pressStartTime === null) return;
      this.pressDuration = new Date().getTime() - this.pressStartTime;
      this.shoot(this.pressDuration);
      this.pressStartTime = null;
      this.buttonStyle.backgroundColor = '#4CAF50'; // Reset button color
    },
    cancelPress() {
      this.pressStartTime = null;
      this.buttonStyle.backgroundColor = '#4CAF50'; // Reset button color
    },
    updateButtonAnimation() {
      if (this.pressStartTime !== null) {
        const elapsed = new Date().getTime() - this.pressStartTime;
        const maxDuration = 2000; // Maximum duration for full color change
        const progress = Math.min(elapsed / maxDuration, 1);
        this.buttonStyle.backgroundColor = `rgba(76, 175, 80, ${1 - progress})`;
        requestAnimationFrame(this.updateButtonAnimation);
      }
    },
    shoot(pressDuration) {
      this.isAnimating = true;
      this.resultMessage = ''; // Clear previous result message
      
      // 根据按压时间计算投篮距离
      const maxDistance = 200; // 最大投篮距离
      const distance = Math.min(maxDistance, pressDuration / 10);
      
      // 更新球的动画样式
      this.ballStyle = {
        ...this.ballStyle,
        left: `${65 + distance}px`,
        bottom: `${60 + distance / 2}px`,
        transition: 'all 1s ease-out'
      };
      
      setTimeout(() => {
        // 检查是否进球
        const ballCenterX = 65 + distance + 10; // 球的中心X坐标
        const ballCenterY = 60 + distance / 2 + 10; // 球的中心Y坐标
        const basketLeft = 230; // 篮筐的左边界
        const basketRight = 270; // 篮筐的右边界
        const basketTop = 100; // 篮筐的上边界
        const basketBottom = 130; // 篮筐的下边界

        // 确保球的中心点在篮筐的矩形框内
        if (ballCenterX > basketLeft && ballCenterX < basketRight &&
            ballCenterY > basketTop && ballCenterY < basketBottom) {
          this.resultMessage = '进球了！';
        } else {
          this.resultMessage = '未进球！';
        }
        
        this.ballStyle = {
          ...this.ballStyle,
          bottom: '60px',
          transition: 'all 0.5s ease-out'
        };
        setTimeout(() => {
          // 重置位置
          this.ballStyle = {
            left: '65px',
            bottom: '60px',
            transition: 'all 0.3s ease'
          };
          this.isAnimating = false;
        }, 500);
      }, 1000);
    }
  }
}
</script>

<style scoped>
.basketball-game {
  width: 300px;
  height: 400px;
  position: relative;
}

.court {
  width: 100%;
  height: 300px;
  background-color: #ffa500;
  position: relative;
  border: 2px solid #000;
}

.basket {
  width: 40px;
  height: 30px;
  border: 5px solid #ff4500;
  border-bottom: none;
  position: absolute;
  right: 30px;
  top: 100px;
}

.player {
  position: absolute;
  bottom: 20px;
  width: 30px;
  height: 60px;
}

.head {
  width: 20px;
  height: 20px;
  background-color: #000;
  border-radius: 50%;
  position: absolute;
  top: 0;
  left: 5px;
}

.body {
  width: 10px;
  height: 25px;
  background-color: #000;
  position: absolute;
  top: 20px;
  left: 10px;
}

.arms {
  width: 30px;
  height: 5px;
  background-color: #000;
  position: absolute;
  top: 25px;
  left: 0;
}

.legs {
  width: 20px;
  height: 5px;
  background-color: #000;
  position: absolute;
  bottom: 0;
  left: 5px;
}

.ball {
  width: 20px;
  height: 20px;
  background-color: #ff4500;
  border-radius: 50%;
  position: absolute;
}

button {
  margin-top: 20px;
  padding: 10px 20px;
  font-size: 16px;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

button:disabled {
  background-color: #cccccc;
  cursor: not-allowed;
}

button:hover:not(:disabled) {
  background-color: #45a049;
}

.result-message {
  margin-top: 20px;
  font-size: 18px;
  color: #333;
}
</style> 