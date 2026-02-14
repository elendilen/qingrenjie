<template>
  <div class="resumeEditor" :class="{htmlMode:enableHtml}" ref="container">
    <div v-if="enableHtml">
      <div v-html="result"></div>
      <section v-if="photos && photos.length" class="gallery">
        <h2 class="gallery-title">{{ galleryTitle }}</h2>
        <p class="gallery-subtitle">{{ gallerySubtitle }}</p>
        <div class="gallery-stage">
          <a class="gallery-main-link" :href="activePhotoUrl" target="_blank" rel="noopener">
            <transition name="fade" mode="out-in">
              <img
                :key="activePhotoUrl"
                class="gallery-main"
                :src="activePhotoUrl"
                :alt="galleryTitle"
                loading="lazy"
              />
            </transition>
          </a>
          <div class="gallery-indicators">
            <span
              v-for="(photo, index) in photos"
              :key="photo + '-' + index"
              class="gallery-dot"
              :class="{ 'is-active': index === activeIndex }"
            ></span>
          </div>
        </div>
        <div class="gallery-thumbs">
          <button
            v-for="(photo, index) in photos"
            :key="photo"
            class="gallery-thumb-button"
            :class="{ 'is-active': index === activeIndex }"
            @click="setActive(index)"
            type="button"
          >
            <img
              class="gallery-thumb"
              :src="formatPhotoUrl(photo)"
              :alt="'photo-' + (index + 1)"
              loading="lazy"
            />
          </button>
        </div>
      </section>
    </div>
    <pre v-else>{{result}}</pre>
  </div>
</template>

<script>
  import marked from 'marked'
  export default {
    props: ['markdown', 'enableHtml', 'photos', 'galleryTitle', 'gallerySubtitle'],
    name: 'ResumeEditor',
    data () {
      return {
        activeIndex: 0,
        timer: null,
        slideInterval: 3500
      }
    },
    computed: {
      result: function () {
        return this.enableHtml ? marked(this.markdown) : this.markdown
      },
      activePhoto: function () {
        if (!this.photos || !this.photos.length) {
          return ''
        }
        return this.photos[this.activeIndex % this.photos.length]
      },
      activePhotoUrl: function () {
        return this.formatPhotoUrl(this.activePhoto)
      }
    },
    mounted () {
      this.startSlideshow()
    },
    beforeDestroy () {
      this.stopSlideshow()
    },
    methods: {
      goBottom: function () {
        this.$refs.container.scrollTop = 100000
      },
      formatPhotoUrl: function (name) {
        if (!name) {
          return ''
        }
        return `sunny/${encodeURIComponent(name)}`
      },
      startSlideshow: function () {
        if (!this.photos || !this.photos.length) {
          return
        }
        this.stopSlideshow()
        this.timer = setInterval(() => this.nextPhoto(), this.slideInterval)
      },
      stopSlideshow: function () {
        if (this.timer) {
          clearInterval(this.timer)
          this.timer = null
        }
      },
      nextPhoto: function () {
        if (!this.photos || !this.photos.length) {
          return
        }
        this.activeIndex = (this.activeIndex + 1) % this.photos.length
      },
      setActive: function (index) {
        this.activeIndex = index
        this.startSlideshow()
      }
    }
  }

</script>

<style scoped>
  .htmlMode {
    animation: flip 2s;
  }

  .gallery-main-link {
    width: 100%;
    height: 100%;
    display: block;
    color: inherit;
    text-decoration: none;
  }

  .fade-enter-active,
  .fade-leave-active {
    transition: opacity .25s ease;
  }
  .fade-enter,
  .fade-leave-to {
    opacity: 0;
  }
  
  @keyframes flip {
    from {
      opacity: 0;
    }
    to {
      opacity: 1;
    }
  }
</style>