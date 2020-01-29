<template>
<div>
  <form class="uk-search uk-search-default uk-search-width-large" v-on:submit.prevent="submitEvent">
    <button class="uk-search-icon-flip" uk-search-icon
    type="submit"></button>
    <input class="uk-search-input" type="search" placeholder="Search..." v-model="text">
</form>
</div>
</template>

<script>
export default {
  name: 'UkSearch',
  data() {
    return{
      text:null,
    }
  },
  methods:{
    submitEvent(){
      this.$emit("submit",this.text);
    }
  }
}
</script>
<style scoped>
.uk-search-width-large {
    width:500px;
}
</style>