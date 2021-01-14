<template>
  <div :class="['container',{'border-blue':focus} , 'mt-8']">
    <label
      @click="$refs.txtinput.focus()"
      :class="[{ 'label-up ': focus || txtValue }, { 'text-blue': focus }]"
    >
      {{ label }}</label
    >
    <input
      @focus="focus = true"
      @blur="focus = false"
      ref="txtinput"
      type="text"
      v-bind="$attrs"
   
     v-model="txtValue"
      v-on="{ ...$listeners, input }"
    />

   
  </div>
</template>
<script>
export default {
  name: 'textInput',
  data() {
    return {
      focus: null,
     txtValue:null
    }
  },
  methods: {
    input(e) {
      this.$emit('input', e.target.value)
    },
  },

  watch:{
    value:function(){
     
      this.txtValue=this.value
   
    }
  },
  props: {
    label: {
      type: String,
      default: '',
    },
    value: {
      type: String,
      default: '',
    },
  },
}
</script>
<style scoped>
.container {
  border: 1px solid grey;
  border-radius: 5px;
  padding: 5px;
  position: relative;
  margin: 8px 0;
}
input {
  width: 100%;
  border: none;
  outline: none;
}
label {
  position: absolute;
  background-color: #fff;
  padding: 0 5px;
   transition: top 0.1s;
    top: 2px;
}
.label-up {
  top: -12px;
  font-size: 14px;

}
.text-blue {
  color: blue;

}
.border-blue{
    border: 1px solid blue;
}
.text-sm{
  font-size: 14px;
}
</style>
