<template>
  <div class="recommend">
    <div v-for="item in recommends" class="recommend-item">
      <a :href="item.link">
        <img :src="item.image" alt="">
        <div> {{item.title}}</div>
      </a>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    recommends: {
      type: Array,
      default() {
        return []
      }
    }
  }

}
</script>

<style>
.recommend {
  display: flex;
  /* padding-left: 5%; */
  padding-top: 8px;
  font-size: 12px;
  text-align: center;
  border-bottom: 5px solid #eee;
}
.recommend-item {
  flex: 1;
 
}
.recommend-item img {
  width: 80%;
  margin-bottom: 5px;
  
  
}
</style>