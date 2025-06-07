<template>
  <div class="homepage-container">
    <div class="container">
      <div class="left-content">
        <h1 class="main-title">{{ title }}</h1>
        <p class="subtitle">{{ subtitle }}</p>
        <div class="cta-buttons">
          <button class="cta-button shop-now" @click="goToShop">
            {{ shopButtonText }}
          </button>
          <button class="cta-button learn-more" @click="goToLearnMore">
            {{ learnMoreButtonText }}
          </button>
        </div>
      </div>
      <div class="right-content">
        <div
          class="bottles-orbit"
          :style="{ transform: `rotate(${orbitRotation}deg)` }"
        >
          <div class="orbit-center">
            <div
              class="bottle-container floating-animation"
              v-for="(bottle, index) in bottles"
              :key="`bottle-${bottle.id}`"
              :style="getBottlePosition(index)"
            >
              <img
                v-if="bottle.image"
                :src="bottle.image"
                alt="bottle"
                class="bottle-image"
                @error="handleImageError(index)"
              />
              <div v-else class="bottle-image placeholder-bottle">
                {{ bottle.name || `Brand ${index + 1}` }}
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted, onUnmounted } from "vue";
import cocktailImg from "../assets/Cocktail.jpg";
import passionMojitoImg from "../assets/PassionMojito.jpg";
import pineappleMintImg from "../assets/PineapleMint.jpg";

const props = defineProps({
  customeTitle: {
    type: String,
    default: "Kitu Jaba",
  },
  customSubtitle: {
    type: String,
    default: "Simply Refreshing",
  },
  customBottles: {
    type: Array,
    default: () => [],
  },
  rotationSpeed: {
    type: Number,
    default: 0.2,
  },
});

const emit = defineEmits(["shop-now", "learn-more", "bottle-click"]);

const title = ref(props.customeTitle);
const subtitle = ref(props.customSubtitle);
const shopButtonText = ref("Shop Now");
const learnMoreButtonText = ref("Learn More");
const orbitRotation = ref(0);

const defaultBottles = [
  {
    id: 1,
    name: "Cocktail",
    image: cocktailImg,
    alt: "Kitu Jaba Cocktail",
  },
  {
    id: 2,
    name: "Passion Mojito",
    image: passionMojitoImg,
    alt: "Kitu Jaba Passion Mojito",
  },
  {
    id: 3,
    name: "Pineapple Mint",
    image: pineappleMintImg,
    alt: "Kitu Jaba Pineapple Mint",
  },
];

const bottles = ref(
  props.customBottles.length > 0 ? props.customBottles : defaultBottles
);

let animationFrame = null;

const bottleCount = computed(() => bottles.value.length);

const getBottlePosition = (index) => {
  const angle = (360 / bottleCount.value) * index;
  const radius = 140;
  const radian = (angle * Math.PI) / 180;

  const x = Math.cos(radian) * radius;
  const y = Math.sin(radian) * radius;

  return {
    transform: `translate(${x - 40}px, ${y - 60}px)`,
    animationDelay: `${index * 0.5}s`,
  };
};

const goToShop = () => {
  emit("shop-now");

  console.log("Shop Now clicked");
};
const goToLearnMore = () => {
  emit("learn-more");

  console.log("Learn More clicked");
};

const handleImageError = (index) => {
  console.warn(`Failed to load image for bottle ${index + 1}`);
};

const handleBottleClick = (bottle, index) => {
  emit("bottle-click", { bottle, index });
};

const animateOrbit = () => {
  orbitRotation.value += props.rotationSpeed;
  if (orbitRotation.value >= 360) {
    orbitRotation.value = 0;
  }
  animationFrame = requestAnimationFrame(animateOrbit);
};

//For extrenal use
const updateBottleImage = (index, imageUrl) => {
  if (bottles.value[index]) {
    bottles.value[index].image = imageUrl;
  }
};

const updateBottleData = (index, data) => {
  if (bottles.value[index]) {
    bottles.value[index] = { ...bottles.value[index], ...data };
  }
};

const addBottle = (bottleData) => {
  bottles.value.push({
    id: Date.now(),
    name: "",
    image: "",
    alt: "",
    ...bottleData,
  });
};

const removeBottle = (index) => {
  if (bottles.value.length > 1) {
    bottles.value.splice(index, 1);
  }
};

onMounted(() => {
  animateOrbit();
});

onUnmounted(() => {
  if (animationFrame) {
    cancelAnimationFrame(animationFrame);
  }
});

defineExpose({
  updateBottleData,
  updateBottleImage,
  addBottle,
  removeBottle,
  bottles,
});
</script>

<style scope>
.homepage-container {
  font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
  background: #cf9230;
  background: linear-gradient(
    90deg,
    rgba(207, 146, 48, 1) 0%,
    rgba(148, 86, 47, 1) 50%,
    rgba(108, 36, 32, 1) 100%
  );
  min-height: 100vh;
  overflow-x: hidden;
}
.container {
  display: flex;
  min-height: 100vh;
  align-items: center;
  max-width: 1400px;
  margin: 0 auto;
  padding: 0 40px;
}
.left-content {
  flex: 1;
  z-index: 10;
}
.main-title {
  font-size: clamp(3rem, 8vw, 6rem);
  font-weight: 900;
  color: #fff;
  margin-bottom: 20px;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);
  letter-spacing: -2px;
}
.subtitle {
  font-size: clamp(1.2rem, 3vw, 2rem);
  color: #f4e4c1;
  margin-bottom: 60px;
  font-weight: 300;
  letter-spacing: 1px;
}
.cta-buttons {
  display: flex;
  gap: 25px;
  flex-wrap: wrap;
}
.cta-button {
  padding: 16px 32px;
  font-size: 1.1rem;
  font-weight: 600;
  border: none;
  border-radius: 50px;
  cursor: pointer;
  transition: all 0.3s ease;
  text-transform: uppercase;
  letter-spacing: 0.5px;
}
.shop-now {
  background: linear-gradient(45deg, #f4e4c1, #e6d7b8);
  color: #6c2420;
  box-shadow: 0 4px 15px rgba(244, 228, 193, 0.3);
}
.shop-now:hover {
  background: linear-gradient(45deg, #fff, #f4e4c1);
  transform: translateY(-2px);
  box-shadow: 0 4px 15px rgba(244, 228, 193, 0.5);
}
.learn-more {
  background: transparent;
  color: #f4e4c1;
  border: 2px solid #f4e4c1;
  box-shadow: 0 4px 15px rgba(244, 228, 193, 0.3);
}
.learn-more:hover {
  background: #f4e4c1;
  color: #6c2420;
  transform: translateY(-2px);
  box-shadow: 0 4px 15px rgba(244, 228, 193, 0.5);
}
.right-content {
  flex: 1;
  position: relative;
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
}
.bottles-orbit {
  position: relative;
  width: 400px;
  height: 400px;
  will-change: transform;
}
.orbit-center {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 300px;
  height: 300px;
  border-radius: 50%;
}
  
.bottle-container {
  position: absolute;
  top: 50%;
  left: 50%;
  width: 80px;
  height: 120px;
  transform-origin: 0 0;
  will-change: transform;
  cursor: pointer;
}
.bottle-image {
    width: 100%;
    height: 100%;
    object-fit: contain;
    filter: drop-shadow(0 10px 20px rgba(0,0,0,0.4));
    transition: transform 0.3s ease;
}

.bottle-image:hover {
    transform: scale(1.1);
}
.floating-animation {
    animation: float 3s ease-in-out infinite;
}
/* @keyframes float {
    0%, 100% {
        transform: translateY(0px) translateX(-50%) translateY(-50%);
    }
    50% {
        transform: translateY(-15px) translateX(-50%) translateY(-50%);
    }
} */
.placeholder-bottle {
    background: linear-gradient(45deg, rgba(255,255,255,0.2) 0%, rgba(255,255,255,0.2) 50%, rgba(255,255,255,0.1) 100%);
    border: 2px solid rgba(255,255,255,0.3);
    border-radius: 15px 15px 5px 5px;
    display: flex;
    justify-content: center;
    color: rgba(255,255,255,0.5);
    font-size: 12px;
    text-align: center;
    backdrop-filter: blur(10px);
}

@media (max-width: 768px) {
    .container {
        flex-direction: column;
        padding: 20px;
        text-align: center;
    }
    .left-content {
        margin-bottom: 40px;
    }
    .right-content {
        height: 50vh;
    }
    .bottles-orbit {
        width: 300px;
        height: 300px;
    }
    .orbit-center {
        width: 200px;
        height: 200px;
    }
    .bottle-container {
        width: 60px;
        height: 90px;
    }
    .cta-buttons {
        justify-content: center;
    }
}



</style>
