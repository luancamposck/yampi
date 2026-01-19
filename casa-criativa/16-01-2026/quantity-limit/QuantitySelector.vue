<template>
    <div
        class="quantity-selector relative"
        :class="{ disabled: disabled }"
    >
        <span class="quantity-btn -minus" @click="decrease()">
          <i class="icon icon-minus" />
        </span>

        <input
            v-model.number.lazy="currentValue"
            type="text"
            name="product-quantity"
            :disabled="disabled"
            @keypress="checkInput($event)"
            @blur="checkIfEmpty()"
            @input="notifyInteraction()"
        >

        <span class="quantity-btn -more" @click="increase()">
          <i class="icon icon-more" />
        </span>
    </div>
</template>

<script>
export default {
  name: 'QuantitySelector',

  props: {
    value: { type: Number, default: 1 },
    min: { type: [Number, Boolean], default: 1 },
    max: { type: [Number, Boolean], default: false },
    disabled: { type: Boolean, default: false },
  },

  data: () => ({
    currentValue: 1,
    touched: false,
  }),

  computed: {
    isMaxReached() {
      if (this.max === false) return false
      const max = Number(this.max)
      const val = Number(this.currentValue)
      if (Number.isNaN(max) || Number.isNaN(val)) return false
      return val >= max
    },

    showLimitReached() {
      // aparece só quando atingiu o max e o usuário já mexeu
      return this.touched && this.isMaxReached && !this.disabled
    },
  },

  watch: {
    value: {
      immediate: true,
      handler() {
        this.currentValue = this.value
      },
    },

    currentValue() {
      if (this.currentValue === '') return

      const val = Number(this.currentValue)
      if (Number.isNaN(val)) {
        this.currentValue = this.min !== false ? Number(this.min) : 0
        return
      }

      if (this.min !== false && val < Number(this.min)) {
        this.currentValue = Number(this.min)
        return
      }

      if (this.max !== false && val > Number(this.max)) {
        this.currentValue = Number(this.max)
        return
      }

      this.$emit('input', val)
      this.$emit('change', val)
    },
  },

  methods: {
    notifyInteraction() {
      this.$emit('interaction')
    },

    decrease() {
      if (this.disabled) return
      this.notifyInteraction()
      if (this.min !== false && this.currentValue <= Number(this.min)) return
      this.currentValue--
    },

    increase() {
      if (this.disabled) return
      this.notifyInteraction()
      if (this.max !== false && this.currentValue >= Number(this.max)) return
      this.currentValue++
    },

    checkIfEmpty() {
      this.notifyInteraction()
      if (this.currentValue === '') {
        this.currentValue = this.min !== false ? Number(this.min) : 0
        return
      }

      const val = Number(this.currentValue)
      if (this.max !== false && !Number.isNaN(val) && val > Number(this.max)) {
        this.currentValue = Number(this.max)
      }
    },
  }
}
</script>
