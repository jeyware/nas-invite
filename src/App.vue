<template>
  <div class="min-h-screen bg-pink-50 flex items-center justify-center p-4 font-sans overflow-hidden">
    
    <!-- STEP 0: THE BIG QUESTION -->
    <transition name="fade-slide" mode="out-in">
      <div v-if="step === 0" key="step0" class="text-center">
        <h1 class="text-4xl md:text-6xl font-bold text-pink-600 mb-8">
          Will you go on a date with me? 🌹
        </h1>
        <div class="flex justify-center gap-6 items-center h-20">
          <button 
            @click="nextStep"
            class="bg-pink-500 hover:bg-pink-600 text-white px-8 py-3 rounded-full text-2xl shadow-lg transform transition hover:scale-110"
          >
            Yes! ❤️
          </button>
          
          <!-- The "Runaway" Button -->
          <button 
            @mousemove="moveButton"
            @touchstart="moveButton"
            class="absolute bg-gray-400 text-white px-6 py-2 rounded-full text-xl shadow-md transition-all duration-100"
            :style="noButtonStyle"
          >
            No
          </button>
        </div>
      </div>

      <!-- STEP 1: DATE & TIME SELECTION -->
      <div v-else-if="step === 1" key="step1" class="bg-white p-8 rounded-3xl shadow-xl w-full max-w-md text-center">
        <h2 class="text-2xl font-bold text-pink-500 mb-6">Pick a time! ⏰</h2>
        <div class="grid grid-cols-2 gap-4 mb-8">
          <button v-for="time in times" :key="time" @click="selectTime(time)"
            class="p-3 border-2 border-pink-100 rounded-xl hover:border-pink-500 hover:bg-pink-50 transition">
            {{ time }}
          </button>
        </div>
      </div>

      <!-- STEP 2: FOOD SELECTION -->
      <div v-else-if="step === 2" key="step2" class="bg-white p-8 rounded-3xl shadow-xl w-full max-w-md text-center">
        <h2 class="text-2xl font-bold text-pink-500 mb-6">What are we eating? 😋</h2>
        <div class="grid grid-cols-2 gap-4 mb-8">
          <button v-for="food in foods" :key="food.name" @click="selectFood(food.name)"
            class="flex flex-col items-center p-4 border-2 border-pink-100 rounded-xl hover:border-pink-500 hover:bg-pink-50 transition">
            <span class="text-4xl mb-2">{{ food.icon }}</span>
            <span class="font-medium">{{ food.name }}</span>
          </button>
        </div>
      </div>

      <!-- STEP 3: FINAL CONFIRMATION -->
      <div v-else-if="step === 3" key="step3" class="text-center">
        <div class="text-8xl mb-6 animate-bounce">🎉</div>
        <h1 class="text-4xl font-bold text-pink-600 mb-4">It's a Date!</h1>
        <p class="text-xl text-gray-700">
          I'll be there at <span class="font-bold text-pink-500">{{ selectedTime }}</span> <br>
          to pick you up for <span class="font-bold text-pink-500">{{ selectedFood }}</span>!
        </p>
        <p class="mt-8 text-pink-400 animate-pulse">Can't wait to see you! ❤️</p>
      </div>
    </transition>

  </div>
</template>

<script setup>
import { ref, reactive } from 'vue';

const step = ref(0);
const selectedTime = ref('');
const selectedFood = ref('');

// Data Options
const times = ['6:00 PM', '7:00 PM', '8:00 PM', '9:00 PM'];
const foods = [
  { id: 'kebab', label: 'Kebab', icon: '🍢' },      // Kebab
  { id: 'icecream', label: 'Ice Cream Mix', icon: '🍨' }, // Bastani Maajoon
  { id: 'chicken', label: 'Fried Chicken', icon: '🍗' }, // Morgh Soukhari
  { id: 'sandwich', label: 'Sandwich', icon: '🥪' }    // Sandwich
];

// Runaway Button Logic
const noButtonStyle = reactive({
  position: 'relative',
  left: '0px',
  top: '0px'
});

const moveButton = (event) => {
  // Calculate random position within a safe range
  // This makes the button jump away when the cursor gets close
 const randomX = (Math.random() - 0.5) * 500; // Moves between -250px and 250px
  const randomY = (Math.random() - 0.5) * 500; // Moves between -250px and 250px
  
  noButtonStyle.transform = `translate(${randomX}px, ${randomY}px)`;
};

// Navigation Logic
const nextStep = () => {
  step.value++;
};

const selectTime = (time) => {
  selectedTime.value = time;
  nextStep();
};

const selectFood = (food) => {
  selectedFood.value = food;
  nextStep();
};
</script>

<style scoped>
/* Smooth transitions between steps */
.fade-slide-enter-active,
.fade-slide-leave-active {
  transition: all 0.5s ease;
}

.fade-slide-enter-from {
  opacity: 0;
  transform: translateY(20px);
}

.fade-slide-leave-to {
  opacity: 0;
  transform: translateY(-20px);
}
</style>
