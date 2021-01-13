<template>
    <div>
       <cs v-model="sel" name="a" label="name"></cs> 
       <br>
      
      {{sel}}
       <br>
       <input type="text">
    </div>
</template>
<script>
export default {
    data(){
        return{
            sel:'+9812345'
        }
    },
   
}
</script>
<style >

</style>