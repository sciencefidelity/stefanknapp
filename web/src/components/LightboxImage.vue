<template>
  <div
    class="sanity-image"
    :width=width
    :height=height
  >
    <v-lazy-image
      :alt=title
      :src="$urlForImage(link, $static.metadata.sanityOptions)
        .quality(70)
        .width(600)
        .fit(fit)
        .crop(crop)
        .url()"
      decoding="async"
      loading="lazy"
      :width=width
      :height=height
    />
  </div>
</template>

<static-query>
  query {
    metadata {
      sanityOptions {
        projectId
        dataset
      }
    }
  }
</static-query>

<script lang="ts">
import VLazyImage from "v-lazy-image";

export default {
  name: 'SanityImage',
  components: {
    VLazyImage
  },
  props: {
    title: String,
    link: Object,
    w: String,
    h: String,
    width: Number,
    height: Number,
    pos: {
      type: String,
      default: '50% 50%',
    },
    fit: {
      type: String,
      default: 'crop',
    },
    crop: {
      type: String,
      default: 'center',
    },
  }
}
</script>

<style lang="scss" scoped>
@use '../assets/scss/breakpoints' as b;

.sanity-image {
  position: relative;
  display: grid;
  justify-content: center;
}

img {
  height: auto;
  width: auto;
  max-width: 75vw;
  max-height: calc(80vh - 4.4rem);
  @include b.mq(sm) {
    max-width: 100vw;
  }
}

.v-lazy-image {
  position: relative;
  opacity: 0;
  transition: opacity 1s;
  z-index: 2;
}

.v-lazy-image-loaded {
  opacity: 1;
}
</style>
