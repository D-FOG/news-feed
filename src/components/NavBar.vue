
<template >
    <nav class="navbar">
        <template v-if="bigDevice">
            <div class="top">
                <h3>{{title}}</h3>
                <div class="right">
                    <input class="search" type="text" name="search" id="search" placeholder="Search" v-model="query" @keyup="onKeyUp">
                    <template v-if="isLightMode">
                        <DarkIcon type="mdi" :path="path" title="dark mode" fillColor="#eeeeee" size="24" @click="toggleMode" />
                    </template>
                    <template v-else>
                        <LightIcon type="mdi" :path="path" title="light mode" fillColor="#eeeeee" size="24" @click="toggleMode" />
                    </template>
                        <MenuIcon title="menu" @click="toggleMode" />
                        <div class="menu-list">
                            <p>Login</p>
                        </div>
                </div>
            </div>
        </template>
        <template v-else>
            <div class="top">
                <h3>{{title}}</h3>
                <div class="right">
                    <template v-if="isLightMode">
                        <DarkIcon type="mdi" :path="path" title="dark mode" fillColor="#eeeeee" size="24" @click="toggleMode" />
                    </template>
                    <template v-else>
                        <LightIcon type="mdi" :path="path" title="light mode" fillColor="#eeeeee" size="24" @click="toggleMode" />
                    </template>
                        <MenuIcon title="menu" />
                        <div class="menu-list">
                            <p>Login</p>
                        </div>
                </div>
            </div>
            <input class="search" type="text" name="search" id="search" placeholder="Search" v-model="query" @keyup="onKeyUp">
        </template>
    </nav>
</template>

<script>
import MenuIcon from 'vue-material-design-icons/Menu.vue';
import LightIcon from '@jamescoyle/vue-icon';
import { mdiWhiteBalanceSunny } from '@mdi/js';
import DarkIcon from '@jamescoyle/vue-icon';
import { mdiWeatherNight } from '@mdi/js';
export default {
    name: 'NavBar',
    components: {
        MenuIcon,
        LightIcon,
        DarkIcon,
    },
    props: {
        msg: String,
    },
    data(){
        return {
            title: 'News Feed',
            isLightMode: !this.darkModeOn(),
            path: this.darkModeOn() ? this.path = mdiWhiteBalanceSunny : mdiWeatherNight,
            searchTimer: null,
            bigDevice: window.innerWidth > 800 ? true : false,
        }
    },
    created() {
        this.darkModeOn() ? this.path = mdiWhiteBalanceSunny : mdiWeatherNight;
    },
    methods: {
        toggleMode() {
            this.isLightMode = !this.isLightMode;
            this.path = this.isLightMode ? mdiWeatherNight : mdiWhiteBalanceSunny;
            document.querySelector('body').classList.contains('light') ? document.querySelector('body').classList.remove('light') : document.querySelector('body').classList.add('light');
        },
        onKeyUp() {
            if (this.searchTimer) {
                clearTimeout(this.searchTimer);
            }
            this.searchTimer = setTimeout(() => {
                this.$emit('search', this.query);
                window.scrollTo({ top: 0, behavior: 'instant' });
            }, 500);
        },
        darkModeOn() {
            if (window.matchMedia && window.matchMedia('(prefers-color-scheme: dark)').matches) {
                return true;
            } else {
                document.querySelector('body').classList.add('light');
                return false;
            }
        },   
    }
}
</script>

<style lang="scss">
@import "../styles/_variables.scss";

.navbar {
    position: fixed;
    position: -webkit-sticky;
    top: 0;
    background-color: $dark-bg;
    width: 100%;
    z-index: 99;
    .top {
        padding: 0 20px;
        color: $white;
        display: flex;
        justify-content: space-between;
        align-items: center;
        /* height: 30px; */

        .right {
            display: flex;
            align-items: center;
            justify-content: right;
            gap: 12px;
            &:has(.search) {
                width: 50%;
            }
    
            svg:hover {
                cursor: pointer;
            }
            span {
                height: 24px;
            }
            .menu-list {
                display: none;
            }
            .search {
                max-width: 500px;
                margin: 0;
            }
        }
    }


    ::placeholder {
        color: $med-gray;
        opacity: 1; /* Firefox */
    }
    .search {
        margin-bottom: 16px;
        border-radius: 20px;
        border: none;
        padding: 12px 16px;
        background-color: $gray;
        color: $white;
        outline: none;
        width: 80%;
        max-width: 700px;
        box-shadow: rgba(0, 0, 0, 0.16) 0px 1px 4px;
    }
}
</style>
