<template>
  <div class="header">
    <div class="wrap">
      <div class="fixed-header">
        <div class="page-container">
          <div class="header-container">
            <router-link to="/" v-on:click.native="scrollTop">
              <div class="top-logo">
                <img class="logo-large" src="~@/assets/images/logo.png">
              </div>
            </router-link>
            <div class="top-menu">

              <b-nav>
                <b-nav-item v-bind:to="'/'" v-on:click="scrollTop"> {{ $t("header.home") }} </b-nav-item>
                <b-nav-item href="#pagelink-about-mew">{{ $t("header.about") }}</b-nav-item>
                <b-nav-item href="#pagelink-faqs">{{ $t("reused.faqs") }}</b-nav-item>
                <b-nav-item href="#pagelink-news">{{ $t("header.news") }}</b-nav-item>
                <div class="current-language-flag">
                  <img class="show" data-flag-name="gb" src="~@/assets/images/flags/gb.svg">
                  <img data-flag-name="kr" src="~@/assets/images/flags/kr.svg">
                  <img data-flag-name="jp" src="~@/assets/images/flags/jp.svg">
                  <img data-flag-name="cn" src="~@/assets/images/flags/cn.svg">
                  <img data-flag-name="ru" src="~@/assets/images/flags/ru.svg">
                  <img data-flag-name="de" src="~@/assets/images/flags/de.svg">
                </div>
                <b-nav-item-dropdown class="language-menu" id="nav7_ddown" text="English" extra-toggle-classes="nav-link-custom" right>
                  <b-dropdown-item v-on:click="languageItemClicked" v-for='language in languages' v-bind:class="{active: $root._i18n.locale === language.code}" v-bind:key='language.code+language.lang' :data-flag-name="language.code">{{language.lang}}</b-dropdown-item>
                </b-nav-item-dropdown>
              </b-nav>

            </div>
            <div class="mobile-menu">
              <div class="mobile-menu-button">
                <div class="bar-1"></div>
                <div class="bar-2"></div>
                <div class="bar-3"></div>
              </div>
            </div>

            <router-link to="/create-wallet" class="nounderline">
              <div class="get-free-wallet">
                {{ $t("reused.getAFreeWallet") }}
              </div>
            </router-link>

          </div><!-- .header-container -->
        </div><!-- .page-container -->
      </div><!-- .fixed-header -->
    </div><!-- .wrap -->
  </div><!-- .header -->
</template>

<script>
export default {
  data () {
    return {
      languages: [
        {lang: 'English', code: 'gb'},
        {lang: 'Korean', code: 'kr'},
        {lang: 'Japanese', code: 'jp'},
        {lang: 'Chinese', code: 'cn'},
        {lang: 'Russian', code: 'ru'},
        {lang: 'German', code: 'de'}
      ]
    }
  },
  methods: {
    scrollTop: function () {
      // Scroll to top of the page
      window.scrollTo(0, 0)
    },
    languageItemClicked: function (e) {
      const self = this

      var countryName = e.target.innerText
      document.querySelector('.language-menu a span').innerText = countryName

      // Hide current flag image
      var currentFlag = document.querySelector('.current-language-flag img.show')
      currentFlag.classList.remove('show')

      // Get flag name for clicked language
      var flag = e.target.getAttribute('data-flag-name')

      // Get flag image for the language and display it
      var newFlagImage = document.querySelector('[data-flag-name=' + flag + ']')
      newFlagImage.classList.add('show')

      // switch locale language
      // super hacky, but library doesn't support switching on runtime at the moment
      self.$root._i18n.locale = flag
      localStorage.setItem('locale', flag)
    },
    switchActiveLanguage: function (flag) {
      const elements = document.getElementsByClassName(`dropdown-item`)
      const currentFlag = document.querySelector('.current-language-flag img.show')
      const newFlagImage = document.querySelector('[data-flag-name=' + flag + ']')

      for (let i = 0; i < elements.length; i++) {
        if (elements[i].getAttribute('data-flag-name') === flag) {
          currentFlag.classList.remove('show')
          newFlagImage.classList.add('show')
          document.querySelector('.language-menu a span').innerText = elements[i].innerText
        }
      }
    }
  },
  mounted: function () {
    const self = this
    if (localStorage.getItem('locale') !== null) {
      self.$root._i18n.locale = localStorage.getItem('locale')
      self.switchActiveLanguage(self.$root._i18n.locale)
    } else {
      localStorage.setItem('locale', self.$root._i18n.locale)
      self.switchActiveLanguage(self.$root._i18n.locale)
    }

    // Scroll to top of the page
    window.scrollTo(0, 0)
    // Check if user scrolled window, then change header style.
    window.onscroll = function (e) {
      var element = document.querySelector('body')
      var header = document.querySelector('.fixed-header')
      var logoLarge = document.querySelector('.logo-large')
      var topPos = element.getBoundingClientRect().top
      if (topPos < -150) {
        header.classList.add('tiny-header')
        logoLarge.classList.add('logo-small')
      } else {
        header.classList.remove('tiny-header')
        logoLarge.classList.remove('logo-small')
      }
    }
  }
}
</script>

<style lang="scss" scoped>
  @import "HeaderContainer.scss";
</style>