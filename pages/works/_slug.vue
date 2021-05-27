<template>
    <article>
        <div class="container">
            <nuxt-content class="max-w-xl lg:max-w-4xl mx-auto" :document="work" />
        </div>
    </article>
</template>

<script>
export default {
    async asyncData({ $content, params }) {
        const work = await $content('works', params.slug).fetch()

        return { work }
    },

    // data(){
    //     return {
    //         headTitle: work.description
    //     }
    // },
    head(){
        return {
            title: this.work.title,
            meta: [
                { hid: 'description', name: 'description', content: this.work.description },

                { name: 'og:title', content: `Baghawan | ${this.work.title}` },
                { name: 'og:description', content: this.work.description },
                { name: 'og:image', content: this.work.cover_image },

                { name: 'twitter:title', content: `Baghawan | ${this.work.title}` },
                { name: 'twitter:description', content: this.work.description },
                { name: 'twitter:image', content: this.work.cover_image }
            ]
        }
    }
}
</script>

<style lang="scss" scoped>
article {
    padding-top: 90px;
    font-size: 20px;
    line-height: 1.65;

    /deep/ h1 {
        font-size: 2.5rem;
        font-weight: 600;
        margin-bottom: 1.5rem;
    }
}
</style>