<template>
    <div class="processing-wrapper">
        <RangeSlider 
            id="brightness"
            name="Brightness"
            :update="adjustBrightness"
            :disabled="disabled"
            :limiter="brightnessLimiter"
            text="Slide to adjust image brightness! ☀️"
            :reset="bSliderStatus"
            @sliderResetComplete="resetSliderStatus"
            :colors="bFillColors"
            :colorStop="brightnessLimiter"
        />
        
        <RangeSlider 
            id="contrast"
            name="Contrast"
            :update="adjustContrast"
            :disabled="disabled"
            :limiter="contrastLimiter"
            text="Slide to adjust image contrast! 🌓"
            :reset="cSliderStatus"
            @sliderResetComplete="resetSliderStatus"
            :colors="cFillColors"
            :colorStop="contrastLimiter"
        />
        
        <Uploader 
            :brightness="brightness"
            :contrast="contrast"
            @toggleDisabled="handleToggleDisabled"
            @resetImg="handleResetImg"
            @newUpload="handleResetImg"
        />
    </div>
</template>
<script>
import RangeSlider from './RangeSlider.vue'
import Uploader from './Uploader.vue'
export default {
    name: 'ImageProcessingSection',
    components: {
        RangeSlider,
        Uploader
    },
    data() {
      return {
          disabled: true,
          brightness: 0,
          brightnessLimiter: "0.50",
          contrast: 0,
          contrastLimiter: "0.50",
          bSliderStatus: false,
          cSliderStatus: false
      }  
    },
    computed: {
        bFillColors() {
            return this.disabled
                ? {
                    fill: "#bfbfbf",
                    track: "#e4e4e4"
                }
                : {
                    fill: "#25A95B",
                    track: "rgba(37, 169, 91, 0.25)"
                }
        },
        cFillColors() {
            return this.disabled
                ? {
                    fill: "#bfbfbf",
                    track: "#e4e4e4"
                }
                : {
                    fill: "#4A90E2",
                    track: "rgba(37, 113, 169, 0.25)"
                }
        },
    },
    methods: {
        handleResetImg() {
            this.brightness = 0
            this.brightnessLimiter = "0.50"
            this.contrast = 0
            this.contrastLimiter = "0.50"
            this.bSliderStatus = true
            this.cSliderStatus = true
        },

        resetSliderStatus(sliderType) {
            if (sliderType === "brightness") {
                this.bSliderStatus = false
            } else if (sliderType === "contrast") {
                this.cSliderStatus = false
            }
        },

        handleToggleDisabled(val) {
            this.disabled = val
        },

        adjustBrightness(e) {
            this.brightness = e.target.valueAsNumber
            this.brightnessLimiter = ((e.target.valueAsNumber + 100) / 200).toString()
        },
        
        adjustContrast(e) {
            this.contrast = e.target.valueAsNumber
            this.contrastLimiter = ((e.target.valueAsNumber + 100) / 200).toString()
        }
    }
}
</script>
<style lang="scss" scoped>
    .processing-wrapper {
        display: flex;
        flex-flow: column nowrap;
        align-items: center;
        justify-content: center;
        width: 90%;
        max-width: 460px;
    }

</style>