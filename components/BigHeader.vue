<template>
    <section id="theOrigin" class="hero is-fullheight">
        <div class="hero-body"  v-resize="handleResize">
          <div class="container" v-scroll="handleScroll">
              <div class="columns">
                  <div class="column is-12-mobile is-2">
                      <span class="initial">J.</span>
                  </div>
                  <div class="column is-12-mobile is-10-tablet is-7">
                      <!-- <h2>Introduction</h2> -->
                      <h1>Hi! I am <span class="highlight">João</span>.<br>
                      <TextShifter :texts="shiftText" />
                      <br>Based in Stockholm.</h1>
                      <router-link v-if="$frontmatter.home" to="/about/#aboutSection" class="home-link highlight">Learn more about me...</router-link>
                      <a v-on:click="pageNavigate" :section="firstSection" class="scroll-to"></a>
                  </div>
              </div>
          </div>
        </div>
    </section>
</template>

<script>
import TextShifter from './TextShifter.vue'

/**
 * TODO: check load / reload and background image is huge due to margin-top of main text container
 */
export default {
  components: { TextShifter },

  props: ['home', 'shiftText'],

  data () {
    return {
      loading: true,
      hero: {},
      heroContainer: {},
    }
  },

  mounted () {
    this.init();
  },

  computed: {
    firstSection () {
      return this.$frontmatter.home ? 'blogSection' : 'aboutSection';
    }
  },

  methods: {
    calculateHero() {
      this.hero.el = this.$el.getElementsByClassName('hero-body')[0]
      this.hero.style = window.getComputedStyle ? getComputedStyle(this.hero.el, null) : this.hero.el.currentStyle
      this.hero.paddingBottom = parseInt(this.hero.style.paddingBottom)
      this.hero.height = this.hero.el.clientHeight - this.hero.paddingBottom
    },

    calculateHeroContainer() {
      this.heroContainer.el = this.hero.el.getElementsByClassName('container')[0]
      this.heroContainer.style = window.getComputedStyle ? getComputedStyle(this.heroContainer.el, null) : this.heroContainer.el.currentStyle
      this.heroContainer.marginTop = parseInt(this.heroContainer.style.marginTop)
      this.heroContainer.marginBottom = parseInt(this.heroContainer.style.marginBottom)
      this.heroContainer.height = this.heroContainer.el.offsetHeight + this.heroContainer.marginBottom
      this.heroContainer.minSpaceVisible = this.heroContainer.height + this.hero.paddingBottom
    },

    calculateOpacity() {
      return ((this.hero.height - window.scrollY - this.heroContainer.minSpaceVisible) / (this.hero.height - this.heroContainer.minSpaceVisible));
    },

    updateHeroContainerStyle(margin) {
      this.heroContainer.el.setAttribute(
        'style',
        `margin-top: ${margin}px; opacity: ${this.calculateOpacity()};`
      )
    },

    updateAll() {
      this.calculateHero()
      this.calculateHeroContainer()
      this.updateHeroContainerStyle()
    },

    init () {
      this.updateAll();
    },

    handleResize (evt, el) {
      this.updateAll();
    },

    handleScroll (evt, el) {
      this.calculateHero()
      this.calculateHeroContainer()
      if (this.heroContainer.minSpaceVisible < this.hero.height - window.scrollY) {
          this.updateHeroContainerStyle(window.scrollY)
      } else {
          this.updateHeroContainerStyle(this.heroContainer.minSpaceVisible)
      }
    },

    pageNavigate (ev) {
      if (this.home !== ev.srcElement.attributes.section.value) {
        this.$emit('home-nav-request', ev.srcElement.attributes.section.value, true)
      }
    },

  }

}  
</script>

<style lang="sass">
@import '../styles/config.sass'
$gradientTopStart: #ADB2B8
$gradientBottomEnd: #ffffff

#theOrigin
  .container
    margin: 0 45px
.hero
  /* display: block */
  // width: 100%
  // height: 100%
  // background: url('../../public/images/hero.png') right -150px top, -moz-linear-gradient(top, $gradientTopStart 0%, $gradientBottomEnd 100%)
  // background: url('../../public/images/hero.png') right -150px top, -webkit-gradient(linear, left top, right bottom, color-stop(0%, $gradientTopStart), color-stop(100%, $gradientBottomEnd))
  // background: url('../../public/images/hero.png') right -150px top, linear-gradient(170deg, $gradientTopStart, $gradientBottomEnd)
  background: url('../../public/images/hero_small_low.png') right -150px top, -moz-linear-gradient(top, $gradientTopStart 0%, $gradientBottomEnd 100%)
  background: url('../../public/images/hero_small_low.png') right -150px top, -webkit-gradient(linear, left top, right bottom, color-stop(0%, $gradientTopStart), color-stop(100%, $gradientBottomEnd))
  background: url('../../public/images/hero_small_low.png') right -150px top, linear-gradient(170deg, $gradientTopStart, $gradientBottomEnd)
  background-repeat: no-repeat
  background-size: cover
  background-blend-mode: overlay
  /* height: 100vh; */

  .initial
    font-size: 4em
    font-weight: bolder
    color: $evenDarkerTextColor
  h1
    padding-bottom: 0
    font-size: 20px
    line-height: 30px
    @media only screen and (min-width: 321px)
      font-size: 25px
    @media only screen and (min-width: 768px)
      font-size: 50px
      line-height: 62px
      padding-bottom: 40px
    @media only screen and (min-width: 992px)
      font-size: 55px
      line-height: 70px
    @media only screen and (min-width: 1200px)
      font-size: 60px
      line-height: 72px
      padding-bottom: 50px


  @media only screen and (min-width: 554px)
    // background: url('../../public/images/hero.png') right -160px top, -moz-linear-gradient(top, $gradientTopStart 0%, $gradientBottomEnd 100%)
    // background: url('../../public/images/hero.png') right -160px top, -webkit-gradient(linear, left top, right bottom, color-stop(0%, $gradientTopStart), color-stop(100%, $gradientBottomEnd))
    // background: url('../../public/images/hero.png') right -160px top, linear-gradient(170deg, $gradientTopStart, $gradientBottomEnd)
    background: url('../../public/images/hero_small_low.png') right -150px top, -moz-linear-gradient(top, $gradientTopStart 0%, $gradientBottomEnd 100%)
    background: url('../../public/images/hero_small_low.png') right -150px top, -webkit-gradient(linear, left top, right bottom, color-stop(0%, $gradientTopStart), color-stop(100%, $gradientBottomEnd))
    background: url('../../public/images/hero_small_low.png') right -150px top, linear-gradient(170deg, $gradientTopStart, $gradientBottomEnd)
    background-repeat: no-repeat
    background-size: contain
    background-blend-mode: overlay

  @media only screen and (min-width: 768px)
    // background: url('../../public/images/hero.png') right -100px top, -moz-linear-gradient(top, $gradientTopStart 0%, $gradientBottomEnd 100%)
    // background: url('../../public/images/hero.png') right -100px top, -webkit-gradient(linear, left top, right bottom, color-stop(0%, $gradientTopStart), color-stop(100%, $gradientBottomEnd))
    // background: url('../../public/images/hero.png') right -100px top, linear-gradient(170deg, $gradientTopStart, $gradientBottomEnd)
    background: url('../../public/images/hero_small_low.png') right -100px top, -moz-linear-gradient(top, $gradientTopStart 0%, $gradientBottomEnd 100%)
    background: url('../../public/images/hero_small_low.png') right -100px top, -webkit-gradient(linear, left top, right bottom, color-stop(0%, $gradientTopStart), color-stop(100%, $gradientBottomEnd))
    background: url('../../public/images/hero_small_low.png') right -100px top, linear-gradient(170deg, $gradientTopStart, $gradientBottomEnd)
    background-repeat: no-repeat
    background-size: contain
    background-blend-mode: overlay

  @media only screen and (min-width: 992px)
    // background: url('../../public/images/hero.png') right top, -moz-linear-gradient(top, $gradientTopStart 0%, $gradientBottomEnd 100%)
    // background: url('../../public/images/hero.png') right top, -webkit-gradient(linear, left top, right bottom, color-stop(0%, $gradientTopStart), color-stop(100%, $gradientBottomEnd))
    // background: url('../../public/images/hero.png') right top, linear-gradient(170deg, $gradientTopStart, $gradientBottomEnd)
    background: url('../../public/images/hero_small_low.png') right -50px top, -moz-linear-gradient(top, $gradientTopStart 0%, $gradientBottomEnd 100%)
    background: url('../../public/images/hero_small_low.png') right -50px top, -webkit-gradient(linear, left top, right bottom, color-stop(0%, $gradientTopStart), color-stop(100%, $gradientBottomEnd))
    background: url('../../public/images/hero_small_low.png') right -50px top, linear-gradient(170deg, $gradientTopStart, $gradientBottomEnd)
    background-repeat: no-repeat
    background-size: contain
    background-blend-mode: overlay

.hero-body
  a
    &:hover
      text-decoration: none

  .highlight
      font-weight: 300
      letter-spacing: 2px
  h2
      font-weight: bold

html
  &.html-onload 
    .hero-content
      opacity: 0

  &.html-loaded
    .hero-content
      animation-duration: 1.5s
      -webkit-animation-name: fadeIn
      animation-name: fadeIn

  &.no-csstransitions
    .hero-content
      opacity: 1

</style>
