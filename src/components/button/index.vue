<template lang="pug">
  router-link(
    v-if="href"
    tag="button"
    class="c-button"
    :class="classNames"
    :to="href"
  )
    c-icon(v-if="iconName" :name="iconName" valign="middle")
    span(v-if="$slots.default")
      slot
  button(
    v-else
    class="c-button"
    :type="type"
    :class="classNames"
    @click="onClick"
  )
    c-icon(v-if="iconName" :name="iconName" valign="middle")
    span(v-if="$slots.default")
      slot
</template>

<script>
// import css
import './index.css'

import {
  toVueProps,
  toClassNames
} from '../../scripts/utils'

const name = 'c-button'
const block = `c-button`
const modifiers = [
  'primary',
  'success',
  'warning',
  'danger',
  'round',
  'outline',
  'flat',
  'loading'
]
const props = Object.assign(
  {
    href: String,
    size: String,
    icon: String,
    type: {
      type: String,
      default: 'button'
    },
    autofocus: Boolean
  },
  toVueProps(modifiers)
)
const classNames = toClassNames(block, modifiers)

export default {
  name,
  props,
  inject: {
    $buttonGroup: { default: null },
    $form: { default: null }
  },
  computed: {
    iconName () {
      return this.loading ? 'loader' : this.icon
    },
    actualSize () {
      const { size, $buttonGroup, $form } = this
      return size ||
        ($buttonGroup && $buttonGroup.size) ||
        ($form && $form.size)
    },
    classNames () {
      const classList = classNames.call(this)
      const size = this.actualSize
      if (size) classList.push(`c-button--${size}`)
      return classList
    }
  },
  methods: {
    onClick (e) {
      this.$emit('click', e)
    }
  },
  mounted () {
    if (this.autofocus) {
      this.$el.focus()
    }
  }
}
</script>
