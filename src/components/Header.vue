<script setup>
import AOS from 'aos';
import 'aos/dist/aos.css';
</script>

<template>
    <div>
        <div :data-aos="(screenWidth > 1000) ? 'slide-left' : 'fade-in'" class="header">
            <div class="header-img">
                <img src="/images/profilbild.jpg" alt="">
            </div>
            <div class="header-text">
                <div class="title" :class="(screenWidth < 1000) ? 'font-1p5' : 'font-2p5'">FULLSTACK - WEB DEVELOPER</div>
                <div class="title" :class="(screenWidth < 1000) ? 'font-3' : 'font-5'">CELINA JOHANSSON</div>
            </div>
            <img class="detail1" src="/images/detail1.svg">
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            screenWidth: screen.width,
        };
    },

    mounted() {
        AOS.init({ duration: 1000});
    }
}
</script>
    
<style scoped>
.header {
    display: grid;
    top: 2em;
    width: 97%;
    left: 2em;
    border: var(--white) solid 1em;
    background-image: linear-gradient(to right, var(--light-blue), var(--dark-blue));
    grid-template-columns: 1fr 2fr;
    color: var(--white);
    vertical-align: middle;
    position: absolute;
    z-index: 1000;
}


.header-text {
    display: flex;
    gap: 3em;
    align-self: center;
    flex-direction: column;
}

.header-img {
    padding: 1em;
}

.header-img>img {
    border: .5em solid white;
    max-height: 250px;
    object-fit: contain;
    border-radius: 50%;
}

.detail1 {
    position: absolute;
    z-index: -100;
    height: 100%;
    right: 50px;
}

@media screen and (max-width: 1000px) {
    .header {
        grid-area: header;
        display: block;
        position: static;
        width: 100%;
        margin: 0;
        top: 0;
        border: none;
        text-align: center;
    }

    .detail1 {
        display: none;
    }

    .header-text {
        gap: 1.5em !important;
        padding-bottom: 1em;
    }
}
</style>
