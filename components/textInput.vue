<template>
  <input
    type="text"
    v-bind="$attrs"
    :value="value"
    v-on="{ ...$listeners, input }"
  />
</template>
<script>
export default {
  name: 'textInput',
  props: ['value'],
  methods: {
    input(e) {
      this.$emit('input', e.target.value)
    },
  },
}
</script>
