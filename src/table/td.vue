<template>
<td class="mu-td" @mouseenter="handleMouseEnter" @mouseleave="handleMouseLeave" @click="handleClick">
  <slot></slot>
</td>
</template>

<script>
export default {
  name: 'ui-td',
  props: {
    name: {
      type: String
    }
  },
  methods: {
    handleMouseEnter (event) {
      this.$emit('hover', event)
      if (this.$parent.handleCellHover) this.$parent.handleCellHover(event, this.name, this)
    },
    handleMouseLeave (event) {
      this.$emit('hoverExit', event)
      this.$emit('hover-exit', event)
      if (this.$parent.handleCellHoverExit) this.$parent.handleCellHoverExit(event, this.name, this)
    },
    handleClick (event) {
      this.$emit('click', event)
      if (this.$parent.handleCellClick) this.$parent.handleCellClick(event, this.name, this)
    }
  }
}
</script>

