<template lang="pug">
  div
    label.checkbox(v-for="(x, index) in item.items",
                  :key="index",
                  :for="x.text || x | slugify")
      input(:id="x.text || x | slugify",
            :name="item.label | slugify",
            :value="x.value || x.text || x",
            v-model="value",
            :type="item.type",
            :class="{ 'is-danger': !!error }",
            @input="updateValue",
            @change="updateValue",
            @blur="updateValue")
      span.checkboxLabel {{ x.text || x }}
</template>

<script>
import fieldsMixin from '@/mixins/fields'

export default {
  name: 'Checkbox',
  mixins: [ fieldsMixin ],
  data: () => ({
    value: []
  }),
  methods: {
    updateValue () {
      this.$emit('input', this.value)
    }
  },
  mounted () {
    const itemsChecked = this.item.items
      .filter(({ checked }) => checked)
      .map(({ value, text }) => value || text)

    this.value = itemsChecked

    itemsChecked.length && (this.$parent.value = itemsChecked)
  }
}
</script>

<style lang="stylus" scoped>
  .checkbox
    margin-right 1rem

    .checkboxLabel
      margin-left .5rem
</style>
