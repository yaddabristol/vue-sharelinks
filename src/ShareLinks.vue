<template>
  <div class="share-links">
    <slot name="before"></slot>
    <share-button
      v-for="platform in activePlatforms"
      :key="platform.name"
      :href="makeHref(platform)"
      :name="platform.name"
      :width="platform.width"
      :height="platform.height"
      :same-window="platform.sameWindow || false"
      @share-link-clicked="onShareLinkClicked"
    >
      <slot :name="platform.name">{{ platform.name }}</slot>
    </share-button>
    <slot name="after"></slot>
  </div>
</template>

<script>
import ShareButton from "./ShareButton.vue";

export default {
  components: {
    ShareButton
  },

  props: {
    platforms: {
      type: Array,
      default: () => []
    },
    extraPlatforms: {
      type: Array,
      default: () => []
    },
    url: {
      type: String,
      default: window.location.href
    },
    title: {
      type: String,
      default: document.title
    },
    image: {
      type: String,
      default: null
    }
  },

  data() {
    return {
      defaultPlatforms: [
        {
          name: "whatsapp",
          href: "whatsapp://send?text=%URL%",
          width: null,
          height: null,
          sameWindow: true
        },
        {
          name: "facebook",
          href: "https://www.facebook.com/sharer/sharer.php?u=%URL%",
          width: 400,
          height: 500
        },
        {
          name: "twitter",
          href: "https://twitter.com/intent/tweet?status=%TITLE%+-+%URL%",
          width: 540,
          height: 260
        },
        {
          name: "pinterest",
          href:
            "http://pinterest.com/pin/create/button/?url=%URL%&description=%TITLE%&media=%IMAGE%",
          width: 520,
          height: 570
        },
        {
          name: "tumblr",
          href: "http://www.tumblr.com/share/link?url=%URL%",
          width: 500,
          height: 500
        },
        {
          name: "linkedin",
          href: "https://www.linkedin.com/sharing/share-offsite/?url=%URL%",
          width: 520,
          height: 570
        }
      ]
    };
  },

  computed: {
    activePlatforms() {
      let active_platforms = [];

      if (this.platforms.length === 0) {
        active_platforms.push(...this.defaultPlatforms);
        active_platforms.push(...this.extraPlatforms);
        return active_platforms;
      }

      active_platforms.push(
        ...this.defaultPlatforms.filter(platform => {
          return this.platforms.includes(platform.name);
        })
      );

      active_platforms.push(...this.extraPlatforms);

      return active_platforms;
    },

    finalImage() {
      if (this.image) {
        return this.image;
      }

      var image = "";

      var og_image = document.querySelector('meta[property="og:image"]');

      if (og_image) {
        image = og_image.getAttribute("content");
      } else {
        var images = document.getElementsByTagName("img");

        if (images.length > 0) {
          image = images[0].getAttribute("src");
        }
      }

      return image;
    }
  },

  methods: {
    makeHref(platform) {
      return platform.href
        .replace("%URL%", encodeURIComponent(this.url).replace(/%20/g, "+"))
        .replace("%TITLE%", encodeURIComponent(this.title).replace(/%20/g, "+"))
        .replace(
          "%IMAGE%",
          encodeURIComponent(this.finalImage).replace(/%20/g, "+")
        );
    },

    onShareLinkClicked(platform, url) {
      this.$emit("share-link-clicked", platform, url);
    }
  }
};
</script>