<template>
  <!-- eslint-disable-next-line vue/require-component-is -->
  <component
    v-bind="linkProps"
    :key="link.slug"
    :aria-label="link.title"
    class="flex font-medium capitalize group whitespace-nowrap"
    :class="{
      [activeClass]: forceActive || currentSlug === link.slug,
      [inactiveClass]: !forceActive && currentSlug !== link.slug
    }"
  >
    {{ link.title }}
  </component>
</template>

<script>
import { defineComponent, computed } from '@nuxtjs/composition-api'
import { useNav } from '~/plugins/nav'

export default defineComponent({
  props: {
    link: {
      type: Object,
      required: true
    },
    forceActive: {
      type: Boolean,
      default: false
    },
    activeClass: {
      type: String,
      default: 'text-primary'
    },
    inactiveClass: {
      type: String,
      default: 'hover:d-primary-text-hover'
    }
  },
  setup(props) {
    const { currentSlug } = useNav()

    const linkProps = computed(() => {
      const { to, href } = props.link
      if (to?.length) {
        return {
          is: 'Link',
          to
        }
      } else if (href?.length) {
        return {
          is: 'Link',
          static: true,
          to: '',
          href,
          blank: true
        }
      } else {
        return {
          is: 'span'
        }
      }
    })

    return {
      currentSlug,
      linkProps
    }
  }
})
</script>

<style scoped lang="postcss">
.nuxt-link-active {
  color: rgba(52, 211, 153);
}
.menu li {
  display: block;
  position: relative;
}
</style>
