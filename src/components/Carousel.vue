<template>
	<div id="carousel">
		<transition-group name="slide">
			<div 
				v-for="(slide, slideIndex) in slideData"
				:key="'slide' + slideIndex"
				class="slideContainer"
				v-show="slideIndex === activeIndex"
			>
				<div class="content">
					{{ slide }}
				</div>
			</div>
		</transition-group>
		<div id="tempButton">
			<button @click="nextSlide">Next</button>
		</div>
	</div>
</template>

<script>
export default {
	name: 'Carousel',
	data() {
		return {
			activeIndex: 0,
			slideData: ['1', '2', '3']
		}
	},
	methods: {
		nextSlide() {
			if (this.activeIndex < this.slideData.length - 1) {
				this.activeIndex++;
			} else {
				this.activeIndex = 0;
			}
		}      
	}
}
</script>

<style lang="scss" scoped>
#carousel {
	width: 100%;
	height: 100%;
	white-space: nowrap;
	overflow: hidden;
	position: relative;
}

.slideContainer {
  display: inline-block;
  width: 100%;
  height: 50%;
}

.content {
	border: 1px solid red;
	height: 100%;
}

#tempButton {
	position: absolute;
	bottom: 0;
}

.slide-enter,
.slide-enter-from {
  display: inline-block;
  position: absolute;
  left: 100%;
}

.slide-enter-active {
  transition: all 3s ease;
  position: absolute;
  left: 100%;
}

.slide-enter-to {
  left: 0;
}

.slide-leave,
.slide-leave-from {
  position: absolute;
  left: 0;
}

.slide-leave-active {
  position: absolute;
  transition: all 3s ease;
}

.slide-leave-to {
  left: -100%;
}
</style>