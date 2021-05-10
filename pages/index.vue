<template>
    <main class="h-screen">
        <h1 class="title sr-only">
            baghawan
        </h1>

        <swiper class="swiper h-full" :options="swiperOption">
            <div class="parallax-bg absolute" slot="parallax-bg" data-swiper-parallax="-13%">Latest Works • Latest Works • Latest Works</div>
            <swiper-slide v-for="work of works" :key="work.slug" class="flex items-center justify-center">
                <div class="work--wrapper text-center w-full px-4">
                    <div class="thumb--wrapper" v-if="work.cover_video != null">
                        <NuxtLink :to="{ name: 'works-slug', params: { slug: work.slug } }" class="cta-float absolute w-full h-full">See Details</NuxtLink>
                        <video preload="auto" autoplay="true" muted loop playsinline>
                            <source :src="work.cover_video" type="video/mp4" />Your browser does not support the video tag. We suggest you upgrade your browser.
                        </video>
                    </div>
                    <figure class="thumb--wrapper" v-else>
                        <NuxtLink :to="{ name: 'works-slug', params: { slug: work.slug } }" class="cta-float absolute w-full h-full">See Details</NuxtLink>
                        <img :src="work.cover_image" :alt="'Gambar thumbnail' + work.title">
                    </figure>
                    <div class="work-title absolute">
                        <!-- <h2 class="text-4xl xs:text-5xl md:text-6xl font-bold mb-12">{{ work.title }}</h2> -->
                        <h2 class="text-xl md:text-4xl font-bold inline-block">{{ work.title }}</h2>
                        
                    </div>
                </div>
            </swiper-slide>
            <div class="swiper-pagination" slot="pagination"></div>
        </swiper>
    </main>
</template>

<script>
import { Swiper, SwiperSlide } from 'vue-awesome-swiper'
import 'swiper/css/swiper.css'

export default {
    components: {
        Swiper,
        SwiperSlide
    },

    async asyncData({ $content, params }) {
        const works = await $content('works', params.slug)
            .only(['title', 'description', 'cover_video', 'cover_image', 'slug'])
            .sortBy('order', 'asc')
            .fetch()

        return {
            works
        }
    },
    
    data(){
        return {
            headTitle: 'Front-End Engineer',
            swiperOption: {
                speed: 1000,
                allowTouchMove: false,
                slidesPerView: 1.35,
                centeredSlides: true,
                spaceBetween: 20,
                parallax: true,
                autoplay: {
                    delay: 4000,
                },
                pagination: {
                    el: '.swiper-pagination',
                    dynamicBullets: true
                },
                breakpoints: {
                    768: {
                        slidesPerView: 1.35,
                        spaceBetween: 80
                    },
                    1024: {
                        slidesPerView: 1.5,
                        spaceBetween: 140
                    }
                },
                on: {
                    slideChange: function (swiper) {
                        console.log(this);
                    }, 
                },
            }
        }
    },
    head(){
        return {
            title: this.headTitle,
            meta: [
                { hid: 'description', name: 'description', content: 'Front-end Engineer based in Indonesia, specialized in refined digital web experiences' },

                { name: 'og:title', content: `Baghawan | ${this.headTitle}` },
                { name: 'og:description', content: 'Front-end Engineer based in Indonesia, specialized in refined digital web experiences' },
                { name: 'og:image', content: '' },

                { name: 'twitter:title', content: `Baghawan | ${this.headTitle}` },
                { name: 'twitter:description', content: 'Front-end Engineer based in Indonesia, specialized in refined digital web experiences' },
                { name: 'twitter:image', content: '' }
            ]
        }
    }
}
</script>

<style lang="scss" scoped>
.background-image {
    background-repeat: no-repeat;
    background-size: cover;
    background-position: center;
    z-index: -1;
}

.parallax-bg {
    top: 0;
    left: 0;
    height: 100%;
    line-height: 100vh;
    white-space: nowrap;
    font-size: 8rem;
    opacity: 0.05;
    font-weight: 600;
    filter: blur(1px);
    -webkit-filter: blur(1px);

    @media screen and (min-width: 768px){
        font-size: 12rem;
    }

    @media screen and (min-width: 1024px){
        font-size: 15rem;
    }
}

.work--wrapper {
    position: relative;

    .thumb--wrapper {
        height: 0;
        position: relative;
        padding-bottom: 56.25%;

        > *:not(.cta-float) {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            object-fit: cover;
        }
    }

    .work-title {
        width: auto;
        top: 105%;
        left: 50%;
        padding: 3px 8px;
        background-color: #fff;
        transform: translate(-50%, -40%);
        opacity: 0;
        transition: opacity 500ms cubic-bezier(0.075, 0.82, 0.165, 1) 100ms, transform 1s cubic-bezier(0.075, 0.82, 0.165, 1) 100ms;

        @media screen and (min-width: 768px){
            top: 100%;
        }
    }

    
}

.swiper-slide-active {
    .work--wrapper {
        .work-title {
            opacity: 1;
            transform: translate(-50%, -50%);
            transition: opacity 500ms cubic-bezier(0.075, 0.82, 0.165, 1) 1000ms, transform 500ms cubic-bezier(0.075, 0.82, 0.165, 1) 1000ms;
        }
    }
}

.cta-float {
    top: 0;
    left: 0;
    z-index: 2;
    text-indent: -9999px;
}

.swiper-pagination {
    bottom: 2rem !important;

    /deep/ .swiper-pagination-bullet {
        background-color: #939597;

        &-active {
            background-color: #F5DF4D;
        }
    }
}

.dark-mode {
    .parallax-bg {
        opacity: 0.025;
    }

    .work--wrapper {
        .thumb--wrapper {
            &:after {
                background-color: rgba(0, 0, 0, 0.25);
            }
        }

        .work-title {
            background-color: #1a202c;
        }
    }
}
</style>
