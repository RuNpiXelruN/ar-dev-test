<template>
    <div class="range-section">
        <div class="range-wrapper">
            <h3 :style="'color: ' + this.fillColor">{{ name }}</h3>
            <div class="range">
                <input
                    :id="id"
                    :ref="id + '_range'"
                    type="range"
                    min="-100"
                    max="100"
                    v-model="value"
                    @input="update"
                    :disabled="disabled"
                    :style="style"
                />
            </div>
            <p>{{ text }}</p>
        </div>
    </div>
</template>
<script>
export default {
    name: 'RangeSlider',
    props: {
        id: String,
        name: String,
        text: String,
        update: Function,
        disabled: Boolean,
        limiter: String,
        reset: Boolean,
        trackColor: String,
        fillColor: String,
        fillColorX: Function,
        colorStop: String
    },
    data() {
        return {
            value: "0"
        }
    },
    computed: {
        style() {
            return {
                'background-image':
                '-webkit-gradient(linear, left top, right top, '
                + 'color-stop(' + this.colorStop + ', ' + this.fillColor + '), '
                + 'color-stop(' + this.colorStop + ',' + this.trackColor + ')'
                + ')',
                "background-color": this.trackColor,
                '--fill-color': this.fillColor,
                '--track-color': this.trackColor,
            }
        }
    },
    watch: {
        reset(newVal) {
            if (newVal) {
                this.value = "0"
                this.$emit('sliderResetComplete', this.id)
            }
        }
    },
}
</script>
<style lang="scss" scoped>
    $fillColor: var(--fill-color);
    $trackColor: var(--track-color);

    @mixin thumb {
        background-color: $fillColor;
        border: 3px solid white;
        border-radius: 50px;
        height: 22px;
        width: 22px;
        text-align: center;
    }

    .range-section {
        display: flex;
        flex-flow: row nowrap;
        width: 100%;
        align-items: center;
        justify-content: center;
        border-radius: 5px;
        box-shadow: 0 0 10px 0 rgba(0, 0, 0, 0.1);
        margin-bottom: 10px;

        .range-wrapper {
            width: 90%;
            padding: 10px 0;

            h3 {
                margin-top: 5px;
                margin-bottom: 10px;
                font-family: 'GraphikMedium';
                font-weight: 400;
                font-size: 18px;
                line-height: 25px;
                font-weight: 400;
            }
            p {
                width: 100%;
                margin: 0;
                font-family: 'GraphikRegular';
                font-size: 13px;
                line-height: 25px;
            }

            .range {
                width: 100%;

                input {
                    width: 100%;
                    margin-bottom: 15px;
                }

                input[type="range"]{
                    -webkit-appearance: none;
                    -moz-apperance: none;
                    border-radius: 6px;
                    height: 6px;
                    background-image: -webkit-gradient(
                        linear,
                        left top,
                        right top,
                        color-stop(0.5, $fillColor),
                        color-stop(0.5, $trackColor)
                    );
                 
                    &::-webkit-slider-thumb {
                        @include thumb;
                        -webkit-appearance: none !important;
                    }

                    &::-moz-range-thumb {
                        @include thumb;
                        appearance: none;
                    }
                }
            }
        }
        
    }
</style>