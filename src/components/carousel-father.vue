<template lang="pug">
  .carousel-container
    .slides
      .slide
        slot
    .controlls
      button.left(@click="toPrevSlide" class="btn") 
        img(v-lazy="leftIcon")
      .circles-container
        .circle(v-for="i in dataSlide.length" :class="`circle--${i} `" @click="goToSlide(i-1)")
      button.right(@click="toNextSlide" class="btn")
        img(v-lazy="rigthIcon")
    .thumbnails-container
      button.leftThumbnail(@click="toPrevThumb" class="btn") 
        img(v-lazy="leftIcon")
      .thumbnails
        .thumbnail(
          v-for="t,index in dataShow" 
          :key="index" 
          :class="`t--${index}`" 
          @click="goToThumbnail(index)"
          )
          h5 {{t.category}}
      button.rightThumbnail(@click="toNextThumb" class="btn") 
        img(v-lazy="rigthIcon")

</template>
<script>
import cardDocument from "../components/card-document";
import leftIcon from "../assets/arrow-left.png";
import rigthIcon from "../assets/arrow-rigth.png";

export default {
  name: "carousel-complex",
  props: {
    dataShow: {
      type: Array,
      required: true
    }
  },
  data: function() {
    return {
      currentSlide: 0,
      currentThumbnails: 0,
      leftIcon,
      rigthIcon
    };
  },
  components: {
    cardDocument
  },
  computed: {
    dataSlide: function() {
      return this.dataShow[this.currentThumbnails].datas;
    },
    infoSlide: function() {
      return this.dataSlide[this.currentSlide];
    }
  },
  watch: {
    infoSlide: function() {
      this.$emit("info", this.infoSlide);
    }
  },
  methods: {
    currentCircle: function(n, o) {
      this.$el.children[1].children[1].children[o].classList.remove("current");
      this.$el.children[1].children[1].children[n].classList.add("current");
    },
    currentThumbnail: function(n, o) {
      this.$el.children[2].children[1].children[o].classList.remove("current");
      this.$el.children[2].children[1].children[n].classList.add("current");
    },
    toNextSlide: function() {
      let old = this.currentSlide;
      if (this.currentSlide < this.dataSlide.length - 1) {
        this.currentSlide++;
      } else {
        this.currentSlide = 0;
      }
      this.currentCircle(this.currentSlide, old);
    },
    toPrevSlide: function() {
      let old = this.currentSlide;
      if (this.currentSlide > 0) {
        this.currentSlide--;
      } else {
        this.currentSlide = this.dataSlide.length - 1;
      }
      this.currentCircle(this.currentSlide, old);
    },
    goToSlide: function(i) {
      this.currentCircle(i, this.currentSlide);
      this.currentSlide = i;
    },
    toNextThumb: function() {
      let old = this.currentThumbnails;
      if (this.currentThumbnails < this.dataShow.length - 1) {
        this.currentThumbnails++;
      } else {
        this.currentThumbnails = 0;
      }
      this.currentCircle(0, this.currentSlide);
      this.currentSlide = 0;
      this.currentThumbnail(this.currentThumbnails, old);
    },
    toPrevThumb: function() {
      let old = this.currentThumbnails;
      if (this.currentThumbnails > 0) {
        this.currentThumbnails--;
      } else {
        this.currentThumbnails = this.dataShow.length - 1;
      }
      this.currentCircle(0, this.currentSlide);
      this.currentSlide = 0;
      this.currentThumbnail(this.currentThumbnails, old);
    },
    goToThumbnail: function(i) {
      this.currentThumbnail(i, this.currentThumbnails);
      this.currentThumbnails = i;
      this.currentCircle(0, this.currentSlide);
      this.currentSlide = 0;
    }
  },
  mounted: function() {
    this.$emit("info", this.infoSlide);
    this.$el.children[1].children[1].children[this.currentSlide].classList.add(
      "current"
    );
    this.$el.children[2].children[1].children[
      this.currentThumbnails
    ].classList.add("current");
  }
};
</script>
<style lang="stylus" scoped>
  .controlls
    flex-grow 1
    display flex
    margin 10px auto
  .circles-container
    display flex
    justify-content center
    align-items center
  .circle
    width 16px
    height 16px
    margin 0 10px
    background-color #F5A31A
    border-radius 50%
    transition transform 800ms
    &:hover
      cursor pointer
      transform scale(1.5)
    &.current
      background-color #D32626
  .thumbnails-container
    flex-grow 1
    display:flex
    align-items center
    margin 10px auto
  .thumbnails
    display flex
  .thumbnail
    width  128px
    height 128px
    margin 0 10px
    display flex
    justify-content center
    align-items center
    text-align center
    border-radius 5px
    background-color #F5A31A
    transition transform 800ms
    &:hover
      cursor pointer
      transform scale(1.2)

    &.current
      background-color #D32626
      h5
        color #EDF4F2
    h5
      font-weight 700
      font-size 1.3rem
      text-shadow 5px 5px 10px rgba(51, 63, 56, 0.3)
  .btn
    max-width 64px
    max-height 64px
    background-color transparent
    border-radius 5px
    border-style outset
    border none
    img
      max-width 32px
      max-height 32px
    &:hover
      background-color #EDF4F2
      box-shadow 5px 5px 10px rgba(51, 63, 56, 0.3)
      cursor pointer
@media screen and  (max-width: 767px)
  .carousel-container
    .circle
      width  8px
      height 8px
      margin 0 2px
    .thumbnail
      width  64px
      height 64px
      h5
        font-size 0.8rem
        overflow hidden
        text-overflow ellipsis
    .btn
      max-width 32px
      max-height 32px
      img
        max-width  16px
        max-height 16px
@media screen and (min-width: 768px) and (max-width: 1440px)
  .carousel-container
    .thumbnail
      width  96px
      height 96px
      h5
        font-size 1rem
</style>
