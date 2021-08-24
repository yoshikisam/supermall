<template>
<div class="goods-item">
  <img :src="goodsItem.show.img" alt="" >
    <div class="info">
      <p class="title"> {{goodsItem.title}}</p>
      <span class="price">￥{{goodsItem.price}}</span>
    </div>
</div>
</template>

<script>
export default {
  name: "GoodListItem",
  props: {
    goodsItem: {
      type: Object,
      default() {
        return {}
      }
    }
  },
  computed: {
    //   showImage() {}
    //   return this.goodsItem.img || this.goodsItem.image || this.goodsItem.img
    // }
  }
}
</script>

<style scoped>
.goods-item {
  width: 44%;
  padding-bottom: 20px;
  position: relative;


}
.goods-item img{
  width: 100%;
  border-radius: 5px;
  border:3px solid var(--color-tint);
}
.info {
  overflow: hidden;
  text-align: center;
}
.title {
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
  margin-bottom: 3px;
}
.price {
  font-size: 14px;
}
</style>