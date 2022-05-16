<template>
  <component
    :is="htmlComponent"
    v-bind="attrComponent"
    @click="onClick"
    :class="baseClass"
  >
    <slot></slot>
  </component>
</template>

<script setup lang="ts">
import { computed, PropType } from 'vue';
type ButtonSize = "sm"|"md"|"lg"

const props = defineProps({
  variant: {
    type: String,
    default: "primary"
  },
  size: {
    type: String as PropType<ButtonSize>,
    default: "md"
  },
  outline: {
    type: Boolean,
    default:false
  },
  disabled: {
    type: Boolean,
    default: false
  },
  block: {
    type: Boolean,
    default:false
  },
  radius: {
    type: String,
    default: ""
  },
  redirect: {
    type: [String,Object],
    default: ""
  }
})

const emit = defineEmits([
  'click'
])

const baseClass = computed(() => {
  return [
    'btn',
    `btn-${props.size}`,
    `btn-${props.variant}`,
    props.radius ? `rounded-${props.radius}` : null,
    props.block ? 'w-full' : null
  ]
});

const htmlComponent = computed(() => {
  if(props.redirect && props.redirect) {
    if(typeof props.redirect == "string" && /http/.test(props.redirect)) {
      return "a";
    }

    return "router-link"
  }

  return 'button';
});

const attrComponent = computed(() => {
  const attrs = {
    a: {
      href: props.redirect,
      target: "_blank"
    },
    button: {
      type: "button"
    },
    "router-link": {
      to: props.redirect
    }
  }

  return attrs[htmlComponent.value]
});

const onClick = () => emit('click')

</script>

<style scoped>
.btn {
  @apply inline-block text-zinc-800 align-middle border border-transparent
  py-2 px-3 rounded-sm transition-all;
}
.btn-primary { @apply bg-blue-500 text-white hover:bg-blue-600; }
.btn-secondary { @apply bg-gray-500 text-white hover:bg-gray-600; }
.btn-success { @apply bg-green-500 text-white hover:bg-green-600; }
.btn-danger { @apply bg-red-500 text-white hover:bg-red-600; }
.btn-warning { @apply bg-yellow-500 text-white hover:bg-yellow-600; }
.btn-info { @apply bg-teal-500 text-white hover:bg-teal-600; }
.btn-light { @apply bg-zinc-100 text-zinc-900 hover:bg-zinc-200; }
.btn-dark { @apply bg-zinc-800 text-white hover:bg-zinc-900; }

.btn-sm { @apply py-1 text-sm }
.btn-md { @apply px-4 py-2 }
.btn-lg { @apply px-4 py-2 text-lg }
</style>