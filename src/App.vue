<template>
 <div>
   <app-nav></app-nav>
   <app-minter v-if="$eth.showMint"></app-minter>
   <app-banner v-if="!$eth.showMint"></app-banner>
   <app-story></app-story>
   <app-launch-roadmap></app-launch-roadmap>
   <app-developer-roadmap></app-developer-roadmap>
   <app-faq></app-faq>
   <app-team></app-team>
   <app-join></app-join>
   <app-footer></app-footer>

 </div>
</template>

<script>

// import { config } from 'dotenv'
import AppNav from "./components/nav";
import AppBanner from "./components/banner";
import AppStory from "./components/story";
import AppLaunchRoadmap from "./components/launch-roadmap";
import AppDeveloperRoadmap from "./components/developer-roadmap";
import AppFaq from "./components/faq";
import AppTeam from "./components/team";
import AppJoin from "./components/join";
import AppFooter from "./components/footer";
import AppMinter from "./components/minter.js";
import ethConverter from 'ethereum-unit-converter'

import {mapActions} from "vuex";

export default {
  name: 'App',
  components: {
    AppMinter,
      AppFooter,
      AppJoin,
      AppTeam,
      AppFaq,
      AppDeveloperRoadmap,
      AppLaunchRoadmap,
      AppStory,
      AppBanner,
      AppNav
  },
  data: () => ({
    showMint: false
  }),
  computed: {
    getShowMint: function () {
      return this.showMint
    }
  },
  updated() {

    this.$nextTick(function () {
      this.showMint = this.$eth.showMint
    })

  },

  methods: {

    ...mapActions(['setIsRevealed', 'setIsPaused', 'setCost', 'setIsWhitelisted', 'setMaxSupply', 'setTotalSupply', 'setMaxMintPerNFT', 'setMaxMint', 'setGasPrice']),
  },

  async mounted() {

    this.$eth.getTotalSupply().then((supply) => {
      this.setTotalSupply(Number(supply))
    })
    this.$eth.getMaxSupply().then((supply) => {
      this.setMaxSupply(Number(supply))
    })
    // await this.checkIsWhitelisted().then(console.log)
    this.$eth.checkWhitelistEnabled().then(this.setIsWhitelisted)
    this.$eth.getCost().then((cost) => {
      this.setCost(Number(ethConverter(cost, 'wei').ether))
    })
    this.$eth.checkIsPaused().then(this.setIsPaused)
    this.$eth.checkRevealed().then(this.setIsRevealed)
    this.$eth.checkMaxMintAmountPerAddress().then(this.setMaxMintPerNFT)
    this.$eth.checkMaxMintAmount().then(this.setMaxMint)
    this.$eth.checkGasPrice().then(this.setGasPrice)

  }
}
</script>

<style>
  @import './assets/css/styles.css';
  @import './assets/css/ss.css';
</style>
