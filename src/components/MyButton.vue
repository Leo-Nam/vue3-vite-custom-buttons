<template>
  <button v-bind="$attrs" :type="type" :class="classes" ref="button">
    <slot></slot>
  </button>
</template>

<script>
import { ref, onMounted } from 'vue'
export default {
  props: {
    type: {
      default: 'button',
      validator: (value) => {
        const allowed = ['button', 'submit', 'reset']
        return allowed.includes(value)
      }
    },
    sm: Boolean,
    md: {
      type: Boolean,
      default: true,
    },
    lg: Boolean,
    pill: Boolean
  },

  setup(props, context) {
    const classes = []
    const button = ref(null)

    // Prop로 처리되는 변수들
    if (props.sm) classes.push('sm')
    else if (props.lg) classes.push('lg')
    else classes.push('md')

    if (props.pill) classes.push('pill')

    onMounted(() => {
      // Non-prop로 처리되는 변수들
      console.log(context.attrs, 'context.attrs')
      Object.keys(context.attrs).forEach((attr) => {
        if (attr.startsWith('text-')) {
          const color = attr.substring(5) // index값이 5인 이후의 값(문자)를 가져오게 한다
          console.log(color, '1')
          button.value.style.color = color
        }
        if (attr.startsWith('background-')) {
          const color = attr.substring(11) // index값이 11인 이후의 값(문자)를 가져오게 한다
          console.log(color, '2')
          button.value.style.backgroundColor = color
        }
      })
    })

    return {
      classes,
      button
    }
  }
}
</script>

<style>
button {
  outline: none;
}
.sm {
  height: 20px;
  font-size: 13px;
}
.md {
  height: 30px;
  font-size: 22px;
}
.lg {
  height: 40px;
  font-size: 31px;
}
.pill {
  border-radius: 16px;
}
</style>