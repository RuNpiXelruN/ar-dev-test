<template>
    <div class="hero-wrapper">
        <div class="hero-img-wrapper">
            <img src="../assets/beyond-valley-svg.svg" class="hero-bg" alt="beyond valley illustration" />
        </div>
        <div class="profile-pic-wrapper">
            <img src="../assets/justin-headshot.jpeg" class="profile-pic" alt="profile pic of justin davidson" />
        </div>
    </div>
</template>
<script>
export default {
    name: 'HeroSection'
}
</script>
<style lang="scss" scoped>

    @mixin absoluteImage {
        position: absolute;
        margin: auto;
        min-width: 1000%;
        min-height: 1000%;
        top: -10000%;
        bottom: -10000%;
        left: -10000%;
        right: -10000%;
        transform: scale(0.1);
    }

    .hero-wrapper {
        display: flex;
        flex-flow: column nowrap;
        align-items: center;
        justify-content: flex-end;
        width: 100%;
        height: 24vh;
        overflow: hidden;
    }

    .hero-img-wrapper {
        position: relative;
        overflow: hidden;
        width: 100%;
        height: 100%;
    }
    
    .hero-bg {
        @include absoluteImage;
    }
    .profile-pic-wrapper {
        position: absolute;
        overflow: hidden;
        width: 60px;
        height: 60px;
        background: white;
        margin-bottom: -10px;
        border-radius: 100px;
        border: 2px solid white;
    }
    .profile-pic {
        @include absoluteImage;
    }
</style>