<template>
  <!-- Use 'light' color scheme implicitly by forcing the background -->
  <div class="min-h-screen w-full bg-gradient-to-br from-rose-100 via-purple-100 to-teal-100 flex items-center justify-center p-4 font-sans selection:bg-rose-200">
    
    <div class="w-full max-w-md lg:max-w-2xl">
      
      <!-- STEP 0 -->
      <div v-if="step === 0" class="text-center space-y-12 py-10">
        <h1 class="text-5xl md:text-6xl font-black text-slate-900 tracking-tight">
          Will you <br/> <span class="text-rose-600">go on a date?</span>
        </h1>
        
        <div class="flex flex-wrap justify-center items-center gap-6 h-32 relative">
          <button @click="nextStep" class="px-10 py-4 bg-rose-500 text-white rounded-full text-2xl font-bold shadow-lg shadow-rose-300 hover:scale-110 transition-transform">
            Yes! ❤️
          </button>
          <button @click="moveButton" :style="noButtonStyle" class="px-10 py-4 bg-white text-slate-600 rounded-full text-2xl font-bold shadow-md transition-all">
            No
          </button>
        </div>
      </div>

      <!-- STEP 1: TIME -->
      <div v-if="step === 1" class="space-y-6">
        <h2 class="text-3xl font-bold text-slate-900 text-center mb-8">Pick a time ⏰</h2>
        <div class="grid grid-cols-2 gap-4">
          <button v-for="time in times" :key="time" @click="selectTime(time)"
            class="p-6 bg-white/70 backdrop-blur-md border border-white rounded-3xl text-xl font-bold text-slate-800 shadow-sm hover:bg-rose-500 hover:text-white transition-all">
            {{ time }}
          </button>
        </div>
      </div>

      <!-- STEP 2: FOOD -->
      <div v-if="step === 2" class="space-y-6">
        <h2 class="text-3xl font-bold text-slate-900 text-center mb-8">What's the vibe? 😋</h2>
        <div class="grid grid-cols-2 gap-4">
          <button v-for="food in foods" :key="food.id" @click="selectFood(food)"
            class="flex flex-col items-center justify-center p-6 bg-white/70 backdrop-blur-md border border-white rounded-[2rem] shadow-sm hover:bg-white hover:shadow-xl transition-all group">
            <span class="text-4xl mb-2 group-hover:scale-125 transition-transform">{{ food.icon }}</span>
            <span class="text-slate-800 font-bold">{{ food.name }}</span>
          </button>
        </div>
      </div>

      <!-- STEP 3: BENTO CONFIRMATION -->
      <div v-if="step === 3" class="grid grid-cols-1 md:grid-cols-2 gap-4 animate-in fade-in zoom-in duration-500">
        
        <!-- Header -->
        <div class="md:col-span-2 bg-white/50 backdrop-blur-xl border border-white/60 rounded-[2.5rem] p-8 text-center shadow-xl shadow-rose-200/50">
          <div class="text-5xl mb-4">🥳</div>
          <h2 class="text-4xl font-black bg-clip-text text-transparent bg-gradient-to-r from-rose-600 to-purple-700">
            It's a Date!
          </h2>
          <p class="text-rose-700 font-semibold mt-2">See you very soon!</p>
        </div>

        <!-- Time Card -->
        <div class="md:col-span-2 bg-white/50 backdrop-blur-xl border border-white/60 rounded-[2.5rem] p-8 text-center shadow-lg shadow-purple-200/40">
          <p class="text-xs uppercase tracking-[0.2em] text-purple-700 font-bold mb-1">I'll be there at</p>
          <div class="text-5xl md:text-6xl font-black text-slate-900 tracking-tight">
            {{ arrivalTime }}
          </div>
          <div class="mt-4 inline-block px-4 py-1 bg-rose-500 text-white text-xs rounded-full font-bold animate-pulse">
            Arriving 30m early! 🏃‍♂️💨
          </div>
        </div>

        <!-- Food Card -->
        <div class="bg-white/50 backdrop-blur-xl border border-white/60 rounded-[2rem] p-6 text-center shadow-lg shadow-teal-200/40">
          <div class="text-3xl mb-2">{{ selectedFood?.icon }}</div>
          <p class="text-[10px] uppercase text-teal-700 font-bold">Food Vibe</p>
          <p class="text-slate-900 font-bold">{{ selectedFood?.name }}</p>
        </div>

        <!-- Heart Card -->
        <div class="bg-white/50 backdrop-blur-xl border border-white/60 rounded-[2rem] p-6 text-center shadow-lg shadow-orange-200/40">
          <div class="text-3xl mb-2">❤️</div>
          <p class="text-[10px] uppercase text-orange-700 font-bold">Status</p>
          <p class="text-slate-900 font-bold">Confirmed</p>
        </div>

        <div class="md:col-span-2">
          <button @click="step = 0" class="w-full py-4 bg-slate-900 text-white rounded-2xl font-bold shadow-lg hover:bg-black transition-all">
            Restart Invite
          </button>
        </div>
      </div>

    </div>
  </div>
</template>

<script setup>
import { ref, reactive, computed } from "vue";

const step = ref(0);
const selectedFood = ref(null);
const selectedTime = ref(""); 

// --- TIME CALCULATION LOGIC ---
const arrivalTime = computed(() => {
  if (!selectedTime.value) return "";
  const dummyDate = new Date();
  const match = selectedTime.value.match(/(\d+):(\d+)\s*(AM|PM)/i);

  if (!match) return "Invalid Time";

  let [_, hours, minutes, period] = match;
  hours = parseInt(hours, 10);
  minutes = parseInt(minutes, 10);

  if (period.toUpperCase() === "PM" && hours < 12) hours += 12;
  else if (period.toUpperCase() === "AM" && hours === 12) hours = 0;

  dummyDate.setHours(hours, minutes, 0, 0);
  dummyDate.setMinutes(dummyDate.getMinutes() - 30);

  return dummyDate.toLocaleTimeString([], {
    hour: "2-digit",
    minute: "2-digit",
    hour12: true,
  });
});

// --- DATA ---
const times = ["6:00 PM", "7:00 PM", "8:00 PM", "9:00 PM"];
const foods = [
  { id: "kebab", name: "Kebab", icon: "🍢" },
  { id: "icecream", name: "Ice Cream Mix", icon: "🍨" },
  { id: "chicken", name: "Fried Chicken", icon: "🍗" },
  { id: "sandwich", name: "Sandwich", icon: "🥪" },
];

// --- UI LOGIC ---
const noButtonStyle = reactive({
  position: "relative",
  transform: "translate(0px, 0px)",
});

const moveButton = () => {
  // Use viewport-aware math so it doesn't fly off screen entirely
  const x = (Math.random() - 0.5) * 200; 
  const y = (Math.random() - 0.5) * 200;
  noButtonStyle.transform = `translate(${x}px, ${y}px)`;
};

const nextStep = () => (step.value++);
const selectTime = (time) => {
  selectedTime.value = time;
  nextStep();
};
const selectFood = (food) => {
  selectedFood.value = food;
  nextStep();
};
</script>

<style>
/* This forces the browser to render colors as "Light Mode" 
   even if the user has dark mode enabled on their phone */
:root {
  color-scheme: light;
}

* {
  transition: background-color 0.3s ease, transform 0.2s ease;
}

/* Optional: Prevent text from being "auto-inverted" by browsers */
body {
  background-color: #fff1f2; /* Fallback rose-100 */
  color: #1e293b; /* Fallback slate-800 */
}
</style>
