<template>
    <div>
       <text-input v-model="sel" label="label"></text-input>
       <br>
      
      {{sel}}
       <br>
      
      <text-input v-model="sel" label="label"></text-input>
    </div>
</template>
<script>
export default {
    data(){
        return{
      
            sel:'abc'
        }
    },
   
}
</script>
<style >

</style>