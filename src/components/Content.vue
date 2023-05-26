<script setup>
import AOS from 'aos';
import 'aos/dist/aos.css';
import db from '../firebase/init.js';
import { getDocs, collection, getDoc, doc } from 'firebase/firestore';
</script>

<template>
    <div>
        <div class="content-container">
            <div class="detail2 m3v fade-in"
                :style="isFetchingBackgrounds ? { display: 'none' } : { display: 'block' }">
                <img src="/images/detail2.svg">
            </div>
            <div :style="isFetchingBackgrounds ? { display: 'none' } : { display: 'block' }" class="fade-in">
                <div class="title content-title font-2 m1v">PROFILE</div>
                <div class="m1v">{{ userBackground.description }}</div>
            </div>
            <div :style="isFetchingEducations ? { display: 'none' } : { display: 'block' }" class="fade-in">
                <div class="title content-title font-2 m1v">EDUCATION</div>
                <div v-for="edu in educations" :key="edu.id">
                    <div class="title-date-box m1v font-1p2">
                        <div class="sub-title">{{ edu.name }}</div>
                        <div>{{ edu.date }}</div>
                    </div>
                    <div class="m1v">{{ edu.description }}</div>
                </div>
            </div>
            <div :style="isFetchingWorks ? { display: 'none' } : { display: 'block' }" class="fade-in">
                <div class="title content-title font-2 m1v">WORK</div>
                <div v-for="work in works" :key="work.id">
                    <div class="title-date-box font-1p2 m1v">
                        <div class="sub-title">{{ work.name }}</div>
                        <div>{{ work.date }}</div>
                    </div>
                    <div class="m1v">{{ work.description }}</div>
                    <div translate="no" class="work-exp-container">
                        <span v-for="(exp, key) in work.experiences" :key="key">{{ exp }}
                            <line class="exp-line">|</line>
                        </span>
                    </div>
                </div>
            </div>
            <div :style="isFetchingBackgrounds ? { display: 'none' } : { display: 'block' }"
                class="fade-in detail2 detail-bottom m3v">
                <img src="/images/detail2.svg">
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            userBackground: [],
            educations: [],
            works: [],
            isFetchingBackgrounds: true,
            isFetchingEducations: true,
            isFetchingWorks: true,
            screenWidth: screen.width
        };
    },
    methods: {
        async getUserBackground() {
            const userBackground = await getDoc(doc(db, "background", "eYALMvyII1p201mfwqUu"));

            if (!userBackground.exists())
                return console.log("That document does not exist");

            this.userBackground = userBackground.data();
            this.isFetchingBackgrounds = false;
        },

        async getEducations() {
            const educations = await getDocs(collection(db, "educations"));
            educations.forEach(row => this.educations.push(row.data()));

            this.isFetchingEducations = false;
        },

        async getWorks() {
            const works = await getDocs(collection(db, "works"));
            works.forEach(row => this.works.push(row.data()));

            this.isFetchingWorks = false;
        },
    },

    mounted() {
        this.getWorks();
        this.getEducations();
        this.getUserBackground();
        AOS.init({ duration: 1000 });
    }
}
</script>
    
<style scoped>
.content-container {
    max-width: 100%;
    margin-top: 400px;
    padding: 0 1em;
}

.title-date-box {
    display: flex;
    justify-content: space-between;
}

.content-title {
    margin-bottom: .5em;
    padding-bottom: .3em;
    border-bottom: rgb(34, 34, 34, .3) solid 4px;
    max-width: 50%;
}

.work-exp-container {
    display: flex;
    flex-wrap: wrap;
    gap: .4em;
    background-color: var(--dark-teal);
    color: var(--white);
    border-radius: 10px;
    padding: .2em .5em;
}

.exp-line {
    color: var(--light-blue);
}

.detail2>img {
    width: 100%;
    max-width: 300px;
}

.detail-bottom {
    text-align: end;
    width: 100%;
}

@media screen and (max-width: 1000px) {
    .content-container {
        display: block;
        grid-area: content;
        margin-top: 0;
    }

    .detail2 {
        text-align: center;
    }

    .detail2>img {
        min-width: none;
    }

    .detail-bottom {
        text-align: center !important;
    }

    .content-title {
        padding: .5em 0;
        text-align: center;
        margin: auto;
        max-width: 80% !important;
    }

    .work-exp-container {
        border-radius: 0% !important;
    }

    .title-date-box>.sub-title {
        font-size: 1.1em;
    }
}

@media screen and (max-width: 650px) {
    .title-date-box {
        text-align: center;
        flex-direction: column !important;
    }
}
</style>
