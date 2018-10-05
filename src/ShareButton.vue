<template>
  <a
    :href="href"
    class="share-link"
    rel="noopener noreferrer"
    @click.left="onClick"
  >
    <slot></slot>
  </a>
</template>

<script>
export default {
  props: {
    name: {
      type: String,
      required: true,
    },
    href: {
      type: String,
      required: true,
    },
    width: {
      type: Number,
      default: 400,
    },
    height: {
      type: Number,
      default: 500,
    },
    sameWindow: {
      type: Boolean,
      default: false,
    },
  },

  methods: {
    onClick(e) {
      if (!this.sameWindow) {
        e.preventDefault();
      }

      this.$emit('share-link-clicked', this.name, this.url);

      if (!this.sameWindow) {
        window.open(this.href, '', 'status=yes, width=' + this.width + ', height=' + this.height);
      }
    }
  },
};
</script>
