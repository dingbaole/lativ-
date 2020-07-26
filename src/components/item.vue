<template>
  <div class="box">
    <div v-for="(item,index) in newlist" :key="index" class="item" >
      <img :src="item.pic" alt />
      <p>{{ item.title }}</p>
      <p>
        {{ item.price }}
        <van-icon name="cart-o" class="car"  />
      </p>
    </div>
  </div>
</template>

<script>
export default {
  props: ['newlist'],
  name: "",
  data() {
    return {};
  },
  components: {},
  methods: {

  },
};
</script>

<style scoped lang="scss">

.box {
  width: 100%;
  display: flex;
  flex-wrap: wrap;
  .item {
    width: 40%;
    margin: 5%;
    img {
      width: 100%;
    }
    P {
      font-size: 10px;
    }
    p:nth-of-type(2) {
      color: red;
    }
    .car {
      float: right;
    }
  }
}
</style>
