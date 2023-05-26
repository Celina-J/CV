<script setup>
import AOS from 'aos';
import 'aos/dist/aos.css';
import db from '../firebase/init.js';
import { getDocs, collection, getDoc, doc } from 'firebase/firestore';
</script>

<template>
    <div :data-aos="(screenWidth > 1000) ? 'slide-down' : 'fade-in'" class="sidebar">
        <div class="sidebar-container">
            <div :style="isFetchingUser ? { display: 'none' } : { display: 'block' }" class="fade-in">
                <div class="text-title font-1p5 m1v">Contact</div>
                <a href="mailto:celina.johansson@hotmail.com">{{ user.email }}</a>
                <br>
                <a :href="user.linkedin" target="_blank">My linkedIn</a>
                <div>
                    <span>{{ user.city }}, </span>
                    <span>{{ user.country }}</span>
                </div>
            </div>
            <div :style="isFetchingExp ? { display: 'none' } : { display: 'block', paddingBottom: '1em' }" class="fade-in">
                <div class="text-title font-1p5 m1v">Experiences</div>
                <img v-if="screenWidth < 1000" id="toggle-exp-btn" src="/images/arrows.svg" width="40" />
                <div translate="no" class="fade-in" :hidden="(experiencesVisable && screenWidth < 1000) ? true : false"
                    v-for="exp in experiences" :key=exp.name>
                    <div>{{ exp.name }}</div>
                    <div class="exp-level-container">
                        <div :style="{ 'width': exp.level }" class="exp-level"></div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            experiences: [],
            user: [],
            isFetchingExp: true,
            isFetchingUser: true,
            screenWidth: screen.width,
            experiencesVisable: true
        };
    },
    methods: {
        async getExp() {
            const experiences = await getDocs(collection(db, "experiences"));
            const procent = {
                1: '20%',
                2: '40%',
                3: '60%',
                4: '80%',
                5: '100%'
            }
            var data = [];
            experiences.forEach(exp => data.push(exp.data()));

            this.experiences = data.map(d => {
                d.level = procent[d.level];
                return d;
            });
            this.isFetchingExp = false;
        },

        async getUser() {
            const user = await getDoc(doc(db, "user-info", "GUjeK9ddX5dtQbPSlTfm"));

            if (!user.exists())
                return console.log("That user does not exist");

            this.user = user.data();
            this.isFetchingUser = false;
        },

        toggleExperiences() {
            const arrow = document.getElementById('toggle-exp-btn');

            if (this.screenWidth >= 1000)
                return;

            arrow.addEventListener('click', (e) => {
                this.experiencesVisable = !this.experiencesVisable;

                if (!this.experiencesVisable) {
                    arrow.classList.add('rotate');
                } else {
                    arrow.classList.remove('rotate');
                }
            });
        },
    },

    mounted() {
        this.toggleExperiences();
        this.getExp();
        this.getUser();
        AOS.init({ duration: 1000 });
    }
}
</script>
    
<style scoped>
.rotate {
    transition: transform 0.3s ease-in-out;
    transform: rotate(180deg);
}

.sidebar {
    min-height: 100vh;
    width: 400px;
    color: var(--white);
    background-color: #343536;
}

@media screen and (max-width: 1000px) {
    .sidebar {
        display: flex;
        justify-content: center;
        text-align: center;
        min-height: fit-content;
        width: 100%;
    }

    .sidebar-container {
        max-width: 400px;
        position: static;
    }

    .text-title {
        max-width: 100% !important;
    }
}

.sidebar-container {
    margin: 0 2em;
    top: 400px;
}

.exp-level-container {
    margin: .5em 0;
    width: 100%;
    background: var(--white);
    border-radius: 1em;
}

.exp-level {
    height: 10px;
    background: var(--light-blue);
    border-radius: 1em;
}

.text-title {
    border-bottom: var(--dark-teal) solid 4px;
    max-width: 60%;
}
</style>
