<template>
  <div class="uk-grid-match uk-child-width-1-5@xl uk-child-width-1-4@m uk-child-width-1-2@s" uk-grid>
    <div v-for="(item,index) in movies" :key="index">
      <div class="uk-card uk-card-default uk-card-medium" @click="selectItem(item.imdbID)">
        <div class="uk-card-media-top">
          <div
            class="uk-background-cover uk-height-medium uk-panel uk-flex uk-flex-center uk-flex-top"
            :style="'background-image: url('+item.Poster+');'"
          ></div>
        </div>
        <div class="uk-card-body">
          <h3
            class="uk-card-title uk-text-truncate"
            :uk-tooltip="item.Title.replace(':','')"
          >{{item.Title}}</h3>
          <p class="uk-text-meta">{{ item.Year }} - {{ item.Type }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "SearchResult",
  props: ["movies"],
  methods:{
    selectItem(val){
      this.$emit("selected",val);
    }
  }
};
</script>
