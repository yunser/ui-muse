<template>
<div class="mu-tooltip" :class="{'touched': touch, 'when-shown': show}" :style="tooltipStyle">
  <div class="mu-tooltip-ripple" :style="rippleStyle" :class="{'when-shown': show}" ref="ripple">
  </div>
  <span class="mu-tooltip-label">{{label}}</span>
</div>
</template>

<script>
export default {
  name: 'ui-tooltip',
  props: {
    label: {
      type: String
    },
    trigger: {},
    verticalPosition: {
      type: String, // top bottom
      default: 'bottom'
    },
    horizontalPosition: {
      type: String, // left right center
      default: 'center'
    },
    show: {
      type: Boolean,
      default: false
    },
    touch: {
      type: Boolean,
      default: false
    }
  },
  data () {
    return {
      offsetWidth: 0,
      triggerWidth: 0,
      triggerHeight: 0
    }
  },
  computed: {
    tooltipStyle () {
      const {horizontalPosition, verticalPosition, offsetWidth, touch, triggerWidth, triggerHeight, show} = this
      const touchMarginOffset = touch ? 10 : 0
      const touchOffsetTop = touch ? -20 : -10
      const offset = verticalPosition === 'bottom' ? 14 + touchMarginOffset : -14 - touchMarginOffset
      return {
        right: horizontalPosition === 'left' ? '0' : null,
        left: horizontalPosition === 'center' ? ((offsetWidth - triggerWidth) / 2 * -1) + 'px' : horizontalPosition === 'right' ? '0' : '',
        top: !show ? '-3000px' : verticalPosition === 'top' ? touchOffsetTop + 'px' : (triggerHeight - offset + touchMarginOffset + 2) + 'px',
        transform: `translate(0px, ${offset}px)`
      }
    },
    rippleStyle () {
      const {horizontalPosition, verticalPosition} = this
      return {
        left: horizontalPosition === 'center' ? '50%' : horizontalPosition === 'left' ? '100%' : '0%',
        top: verticalPosition === 'bottom' ? '0' : '100%'
      }
    }
  },
  methods: {
    setRippleSize () {
      const ripple = this.$refs.ripple
      const tooltip = this.$el
      if (!tooltip || !ripple) return
      const tooltipWidth = parseInt(tooltip.offsetWidth, 10) / (this.horizontalPosition === 'center' ? 2 : 1)
      const tooltipHeight = parseInt(tooltip.offsetHeight, 10)
      const rippleDiameter = Math.ceil((Math.sqrt(Math.pow(tooltipHeight, 2) + Math.pow(tooltipWidth, 2)) * 2))
      if (this.show) {
        ripple.style.height = `${rippleDiameter}px`
        ripple.style.width = `${rippleDiameter}px`
      } else {
        ripple.style.width = '0px'
        ripple.style.height = '0px'
      }
    },
    setTooltipSize () {
      this.offsetWidth = this.$el.offsetWidth
      if (!this.trigger) return
      this.triggerWidth = this.trigger.offsetWidth
      this.triggerHeight = this.trigger.offsetHeight
    }
  },
  mounted () {
    this.setRippleSize()
    this.setTooltipSize()
  },
  beforeUpdate () {
    this.setTooltipSize()
  },
  updated () {
    this.setRippleSize()
  }
}
</script>
