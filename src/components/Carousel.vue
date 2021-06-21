<template>
  <div id="carousel">
    <div id="leftArrow" class="arrowButtons" @click="prevSlide">
      <img src="@/assets/arrow-left.svg" />
    </div>

    <div id="rightArrow" class="arrowButtons" @click="nextSlide">
      <img src="@/assets/arrow-right.svg" />
    </div>

    <div id="indicatorContainer">
      <div v-for="(dot, dotIndex) in slideData?.length" :key="'dot' + dotIndex">
        <div
          class="dotIndicator"
          :style="
            dotIndex === activeIndex
              ? { backgroundColor: 'white' }
              : { backgroundColor: 'grey' }
          "
        ></div>
      </div>
    </div>

    <transition-group :name="slideDirection">
      <div
        v-for="(slide, slideIndex) in slideData"
        :key="'slide' + slideIndex"
        class="slideContainer"
        v-show="slideIndex === activeIndex"
      >
        <div
          class="content"
          :class="{
            leftCTA: slide.ctaPosition === 'left',
            rightCTA: slide.ctaPosition === 'right',
          }"
        >
          <h1>{{ slide.title }}</h1>
          <h3 v-if="slide.heading">{{ slide.heading }}</h3>

          <div class="ctaContainer">
            <div
              v-for="(action, actionIndex) in slide.cta"
              :key="'action' + actionIndex"
            >
              <div
                class="ctaButton"
                :class="{ multipleCTAButtons: slide.cta.length > 1 }"
                @click="handleGoToLink(slideIndex, actionIndex)"
              >
                {{ action.label }}
              </div>
            </div>
          </div>
        </div>
        <img :src="isDesktopView ? slide.media.desktop : slide.media.mobile" />
      </div>
    </transition-group>
  </div>
</template>

<script>
export default {
  name: "Carousel",
  props: {
    slideData: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {
      activeIndex: 0,
      slideDirection: "",
      loopId: null,
      interval: 8000,
    };
  },
  computed: {
    isDesktopView() {
      let showDesktopView = window.innerWidth > 767 ? true : false;
      return showDesktopView;
    },
  },
  methods: {
    prevSlide() {
      this.pauseLoop();
      this.slideDirection = "slide-left";

      if (this.activeIndex === 0) {
        this.activeIndex = this.slideData.length - 1;
      } else {
        this.activeIndex--;
      }

      this.loop();
    },
    nextSlide() {
      this.slideDirection = "slide-right";

      if (this.activeIndex < this.slideData.length - 1) {
        this.activeIndex++;
      } else {
        this.activeIndex = 0;
      }
    },
    handleGoToLink(slideIndex, actionIndex) {
      window.open(this.slideData[slideIndex].cta[actionIndex].url);
    },
    pauseLoop() {
      clearTimeout(this.loopId);
    },
    loop() {
      this.loopId = setTimeout(() => {
        this.nextSlide();
        this.loop();
      }, this.interval);
    },
  },
  mounted() {
    this.loop();
  },
};
</script>

<style lang="scss" scoped>
h1,
h3 {
  margin: 0px;
  margin-bottom: 1.5rem;
}

h1 {
  font-size: 2.2rem;
}

h3 {
  font-size: 0.75rem;
  width: 80%;
  white-space: break-spaces;
  text-align: center;
}

img {
  height: 100%;
  width: 100%;
  object-fit: cover;
}

#carousel {
  width: 100%;
  height: 100%;
  white-space: nowrap;
  overflow: hidden;
  position: relative;
  display: flex;
  align-items: center;
}

#leftArrow {
  position: absolute;
  z-index: 2;
  left: 0;
}

#rightArrow {
  position: absolute;
  z-index: 2;
  right: 0;
}

.arrowButtons {
  display: none;
}

.slideContainer {
  display: inline-block;
  width: 100%;
  height: 100%;

  .content {
    position: absolute;
    width: 100%;
    height: 100%;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    color: white;
  }
}

.ctaContainer {
  display: flex;
  flex-direction: column;

  .ctaButton {
    border: 2px solid white;
    padding: 8px 24px;
    width: 170px;
    display: flex;
    align-items: center;
    justify-content: center;
    margin: 8px 0px;

    :hover {
      transition: 0.35s ease;
      opacity: 0.7;
      cursor: pointer;
    }
  }
}

#indicatorContainer {
  position: absolute;
  display: flex;
  color: white;
  z-index: 2;
  bottom: 0;
  width: 100%;
  display: flex;
  justify-content: center;

  .dotIndicator {
    width: 12px;
    height: 12px;
    border-radius: 50%;
    margin: 0px 8px;
    margin-bottom: 3em;
  }
}

/* slide left transition */
.slide-left-enter-active,
.slide-left-leave-active {
  transition: all 3s ease;
  position: absolute;
}

.slide-left-enter,
.slide-left-enter-from {
  transform: translateX(-100%);
}
.slide-left-enter-to,
.slide-left-leave,
.slide-left-leave-from {
  transform: translateX(0%);
}

.slide-left-leave-to {
  transform: translateX(100%);
}

/* slide right transition */
.slide-right-enter,
.slide-right-enter-from {
  display: inline-block;
  position: absolute;
  left: 100%;
}

.slide-right-enter-active {
  transition: all 3s ease;
  position: absolute;
  left: 100%;
}

.slide-right-enter-to {
  left: 0;
}

.slide-right-leave,
.slide-right-leave-from {
  position: absolute;
  left: 0;
}

.slide-right-leave-active {
  position: absolute;
  transition: all 3s ease;
}

.slide-right-leave-to {
  left: -100%;
}

/* tablet and desktop */
@media (min-width: 767px) {
  .arrowButtons {
    display: block;
    width: 60px;
    height: 60px;
    margin: 2em;
  }

  .slideContainer {
    .leftCTA {
      display: flex;
      align-items: flex-start;
      margin-left: 12em;
    }

    .rightCTA {
      align-items: flex-start;
      margin-left: calc(50% + 6em);
    }
  }

  .ctaContainer {
    flex-direction: row;

    .multipleCTAButtons {
      margin: 0px 24px;
    }
  }
}
</style>