<template>
    <div id="global-layout">
        <!-- <BigHeader v-on:home-nav-request="navRequest" :shiftText="shiftTexts"/> -->
        <header><h1>Header</h1></header>
        <component :is="layout"/>
        <footer><h1>Footer</h1></footer>
        <!-- TODO: We should have a javascript load strategy for optimization -->
    </div>
</template>

<script>
export default {
    data () {
        return {
            homeInnerNavigation: null,
        }
    },

    computed: {
        layout () {
            if (this.$page.path) {
                if (this.$frontmatter.layout) {
                    // You can also check whether layout exists first as the default global layout does.
                    return this.$frontmatter.layout
                }
                return 'Layout'
            }
            return 'NotFound'
        }
    },

    methods: {
        navRequest (section, force = false) {
            if (section && (force || this.homeInnerNavigation !== section)) {
                // console.log('Layout scrollTo:', section)
                this.homeScrollTo = section
                document.querySelector(`#${section}`).scrollIntoView({ 
                    behavior: 'smooth' 
                });
            }
        },
    },
}
</script>