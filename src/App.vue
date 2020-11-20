<template>
  <div id="app">
    <Header />
    <HeroSection />
    <div class="image-processing-wrapper">
        <ImageProcessingSection />
    </div>
  </div>
</template>

<script>
import Header from './components/Header.vue'
import HeroSection from './components/HeroSection.vue'
import ImageProcessingSection from './components/ImageProcessingSection.vue'

export default {
  name: 'App',
  components: {
    Header,
    HeroSection,
    ImageProcessingSection
  }
}
</script>

<style>
    @font-face {
        font-family: 'GraphikMedium';
        font-display: swap;
        src: local('Graphik Medium Regular'), 
            url(~@/assets/fonts/Graphik_Medium_Regular.otf) format('otf'),
            url(~@/assets/fonts/Graphik_Medium_Regular.ttf) format('ttf'),
            url(~@/assets/fonts/Graphik_Medium_Regular.woff2) format('woff2');
    }
    @font-face {
        font-family: 'GraphikRegular';
        font-display: swap;
        src: local('Graphik Regular Regular'), 
            url(~@/assets/fonts/Graphik_Regular_Regular.otf) format('otf'),
            url(~@/assets/fonts/Graphik_Regular_Regular.ttf) format('ttf'),
            url(~@/assets/fonts/Graphik_Regular_Regular.woff2) format('woff2');
    }

    body {
        margin: 0;
        padding: 0;
        font-family: 'GraphikRegular';
    }

    #app {
        font-family: 'GraphikRegular';
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        color: #2c3e50;
    }

    .image-processing-wrapper {
        display: flex;
        flex-flow: row nowrap;
        align-items: center;
        justify-content: center;
        width: 100%;
        margin-top: 15px;
    }

</style>
