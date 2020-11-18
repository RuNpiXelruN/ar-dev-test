<template>
    <div class="uploader-section">
        <div class="canvas-wrapper">            
            <p v-if="!this.originalImageData">Nothing to see yet!<br>Upload an image below :)</p>
            <button v-else class="reset-btn" ref="resetBtn" @click="handleReset">RESET</button>
            <canvas ref="imgCanvas"></canvas>
        </div>
        <div class="uploader-wrapper">
            <div class="filename-wrapper">
                <span>NAME</span>
                <p class="filename-text">{{ imageName }}</p>
            </div>
            <label class="upload-wrapper">
                <input ref="imageInput" type="file" @change="onInputChange" accept="image/*">
                <div class="upload-btn-wrapper" tabindex="0">
                    <span class="upload-triangle"></span>
                    <span class="upload-text">UPLOAD</span>
                </div>
            </label>
        </div>
    </div>
</template>
<script>
export default {
    name: 'Uploader',
    props: {
        brightness: Number,
        contrast: Number
    },
    data() {
        return {
            imageName: '',
            context: null,
            originalImageData: null
        }
    },
    watch: {
        brightness() {
            this.adjustCanvas()
        },
        contrast() {
            this.adjustCanvas()
        }
    },
    methods: {
        handleReset() {
            this.context.putImageData(this.originalImageData, 0, 0)
            this.$emit('resetImg')
            this.$refs.resetBtn.blur()
        },

        adjustCanvas() {
            let imgData = this.context.getImageData(0, 0, this.context.canvas.width, this.context.canvas.height)
            
            this.adjustBrightness(imgData.data, this.originalImageData.data, this.brightness)
            this.adjustContrast(imgData.data, this.contrast)

            this.context.putImageData(imgData, 0, 0)
        },

        adjustBrightness(data, origData, brightness) {
            for (let i = 0; i < data.length; i+= 4) {
                data[i] = origData[i] + 255 * (brightness / 100)
                data[i+1] = origData[i+1] + 255 * (brightness / 100)
                data[i+2] = origData[i+2] + 255 * (brightness / 100)
            }
        },

        adjustContrast(data, contrast) {
            let multiplicationFactor = (259.0 * (contrast + 255.0)) / (255.0 * (259.0 - contrast))

            for (let i = 0; i < data.length; i+= 4) {
                data[i] = this.clipColor(multiplicationFactor * (data[i] - 128.0) + 128.0)
                data[i+1] = this.clipColor(multiplicationFactor * (data[i+1] - 128.0) + 128.0)
                data[i+2] = this.clipColor(multiplicationFactor * (data[i+2] - 128.0) + 128.0)
            }
        },

        clipColor(value) {
            if (value < 0) {
                value = 0
            } else if (value > 255) {
                value = 255
            }

            return value
        },

        onInputChange(e) {
            let upload = e.target.files
            if (upload.length) {
                this.setImage(upload[0])
            }
        },

        setImage(file) {
            let reader = new FileReader()
            reader.readAsDataURL(file)
            reader.onloadend = (e) => {                

                let img = new Image()
                img.src = e.target.result
                img.onload = () => {
                    this.imageName = file.name
                    let c = this.$refs.imgCanvas
                    let context = c.getContext('2d')
                    c.width = img.width
                    c.height = img.height
                    context.drawImage(img, 0, 0)
                    
                    let imgData = context.getImageData(0, 0, c.width, c.height)
                    this.originalImageData = imgData
                    
                    context.putImageData(imgData, 0, 0)
                    this.context = context

                    this.$emit('toggleDisabled', false)
                }
            }
        },
    },
}
</script>
<style lang="scss" scoped>
    $border-color: #DCDEE4;
    $bg-gray: #F6F8FA;

    @mixin flex-row {
        display: flex;
        flex-flow: row nowrap;
    }
    @mixin flex-col {
        display: flex;
        flex-flow: column nowrap;
    }
    @mixin font-heading {
        font-family: 'GraphikMedium';
        font-weight: 400;
    }
    @mixin font-body {
        font-family: 'GraphikRegular';
    }

    .uploader-section {
        @include flex-col;
        align-items: center;
        justify-content: center;
        width: 100%;
        border: 1px solid $border-color;
        border-radius: 5px;
        padding-bottom: 6px;

        .canvas-wrapper {
            @include flex-col;
            align-items: center;
            justify-content: center;
            width: 100%;
            height: 212px;
            position: relative;
            overflow: hidden;
            border-bottom: 1px solid $border-color;
            margin-bottom: 14px;

            canvas {
                width: 100%;
                position: absolute;
            }

            p {
                @include font-body;
                margin: 0;                
                color: #42506B;
                font-size: 12px;
                line-height: 15px;
             }

             .reset-btn {
                @include font-heading;
                z-index: 99;
                position: absolute;
                left: 2px;
                top: 2px;
                font-size: 12px;
                line-height: 25px;
                letter-spacing: 0.55px;
                color: #4A90E2;
                cursor: pointer;
                border-radius: 5px;
                border: 1px solid $border-color;
                padding-top: 3px;
                transition: background 0.3s ease;

                &:hover {
                    background: #e8e8e8;
                }
             }
        }

        .uploader-wrapper {
            @include flex-row;
            align-items: center;
            justify-content: space-between;
            width: 97%;

            .filename-wrapper {
                @include flex-row;
                align-items: center;
                justify-content: flex-start;
                width: 60%;
                height: 31px;
                background: white;
                border-radius: 5px;
                overflow: hidden;
                border: 1px solid $border-color;

                span {
                    @include font-heading;
                    width: 20%;
                    background: $bg-gray;
                    line-height: 25px;
                    font-size: 11px;
                    letter-spacing: 1.1px;
                    color: #8392A6;
                    padding: 3px 5px;
                    border-right: 1px solid $border-color;
                }

                .filename-text {
                    @include font-heading;
                    width: 80%;
                    padding: 0 6px;
                    text-align: left;
                    line-height: 25px;
                    font-size: 11px;
                    letter-spacing: 1.1px;
                    overflow: hidden;
                    display: block;
                    white-space: nowrap;
                    text-overflow: ellipsis;
                    color: #25A95B;
                }
            }

            .upload-wrapper {
                border: 1px solid $border-color;
                border-radius: 5px;
                padding: 4px 8px;
                margin: 2px;
                background: $bg-gray;
                display: inline-block;
                cursor: pointer;
                transition: background 0.3s ease;

                input[type="file"] {
                    position:absolute;
                    top: -1000px;
                }

                &:hover {
                    background: #e8e8e8;
                }
                &:active {
                    background: #e8e8e8;
                }

                .upload-btn-wrapper {
                    @include flex-row;
                    @include font-heading;
                    align-items: center;
                    justify-content: space-between;
                    width: 69px;
                    font-size: 12px;
                    line-height: 25px;
                    letter-spacing: 0.55px;
                    color: #4A90E2;

                    .upload-triangle {
                        width: 0;
                        height: 0;
                        border-right: 5px solid transparent;
                        border-bottom: 8px solid rgba(74, 144, 226, 78);
                        border-left: 5px solid transparent;
                    }
                }
            }
        }
    }
</style>