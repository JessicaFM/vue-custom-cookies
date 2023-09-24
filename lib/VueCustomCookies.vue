<script setup>
import { ref, onBeforeMount } from 'vue';

const showCookies = ref(false);

const props = defineProps({
    msg: {
        type: String,
        default: 'Hello World!'
    },
    accept: {
        type: String,
        default: 'Accept'
    },   
    decline: {
        type: String,
        default: 'Decline'
    },
    backgroundColor: {
        type: String,
        default: 'rgba(0,0,0,.75)'
    },
    color: {
        type: String,
        default: '#fff'
    },
    align: {
        type: String,
        default: 'center'
    },   
    expired: {
        type: Number,
        default: 30
    }
})

function acceptCookies() {
    var expires = new Date();
    expires.setDate(expires.getDate() + parseInt(props.expired));
    expires.setTime(expires.getTime() + (24 * 60 * 60 * 1000));
    document.cookie = "vue-cookies=1;expires=" + expires.toUTCString();
    showCookies.value = false;
}

function declineCookies() {
    var expires = new Date();
    expires.setDate(expires.getDate() + parseInt(props.expired));
    expires.setTime(expires.getTime() + (24 * 60 * 60 * 1000));
    document.cookie = "vue-cookies=0;expires=" + expires.toUTCString();
    document.querySelector('.vue-cookies').style.display = 'none';
    showCookies.value = true;
}

function getCookies() {
    let cookieName = "vue-cookies="
    let decodedCookie = decodeURIComponent(document.cookie);
    let ca = decodedCookie.split(';');
    for(let i = 0; i <ca.length; i++) {
        let c = ca[i];
        while (c.charAt(0) == ' ') {
            c = c.substring(1);
        }
        if (c.indexOf(cookieName) == 0) {
            showCookies.value = false;
            return c.substring(cookieName.length, c.length);
        }
    }
    showCookies.value = true;
    return '';
}

onBeforeMount(() => {
    getCookies()
})
</script>
<template>
    <div class="vue-cookies" v-if="showCookies">
        <div class="vue-cookies__content">
            <p class="vue-cookies__text">{{ msg }}</p>
            <div class="vue-cookies__buttons">
                <button class="vue-cookies__button vue-cookies__button--accept" @click="acceptCookies">{{ accept }}</button>
                <button class="vue-cookies__button vue-cookies__button--decline" @click="declineCookies">{{ decline }}</button>
            </div>
        </div>
    </div>
</template>
<style scoped>
.vue-cookies {
    background-color: v-bind(backgroundColor);
    width: 100%;
    position: fixed;
    bottom: 0;
    z-index: 3;
}
.vue-cookies__content {
    display: flex;
    flex-direction: column;
    align-items: v-bind(align);
    padding: 2rem 1rem;
}
.vue-cookies__text {
    padding: 0px 2rem 2rem;
    margin: 0;
}
.vue-cookies__buttons {
    display: flex;
    flex-direction: row;
    gap: 20px;
}
.vue-cookies__buttons .vue-cookies__button {
    border: 1px solid v-bind(color);
    background-color: transparent;
    color: v-bind(color);
    padding: .5rem 3rem;
    border-radius: 10px;
    cursor: pointer;
    display: block;

    background: linear-gradient(to right, v-bind(color) 50%, transparent 50%);
    background-size: 200% 100%;
    background-position: right bottom;
    transition: all .5s ease-out;
}
.vue-cookies__buttons .vue-cookies__button:hover {
    background-color: v-bind(color);
    color: v-bind(backgroundColor);
    background-position: left bottom;
}
</style>