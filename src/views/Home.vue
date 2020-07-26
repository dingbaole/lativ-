<template>
  <div>
    <img src="../assets/1.jpg" alt class="tit" />
    <van-search v-model="value" placeholder="搜索商品" @click="push" />
    <van-tabs v-model="active">
      <ul class="nav">
        <li>综合</li>
        <li>销量</li>
        <li>新品</li>
        <li @click="paixu">
          价格
          <van-icon name="ascending" v-if="srot==true" />
          <van-icon name="descending" v-else />
        </li>
      </ul>
      <van-tab v-for="(item,index) in navList" :key="index" :title="item.title">
        <div class="box">
          <div v-for="(item,index) in newlist" :key="index" class="item">
            <img :src="item.pic" alt />
            <p>{{ item.title }}</p>
            <p>
              {{ item.price }}
              <van-icon name="cart-o" class="car" @click="xianshi(item)" />
            </p>
          </div>
        </div>
      </van-tab>
    </van-tabs>
    <div class="top" v-if="top">
      <van-icon name="upgrade" size="50" @click="xiangshang" />
    </div>
    <div class="shop" :class="show==true?'shopshow':'shophide'">
      <img :src="item.pic" alt />
      <div>
        <p>{{ item.title }}</p>
        <p>{{ item.price }}</p>
        <div>
          <van-button size="small" type="primary" style="margin-right:10px">加入购物车</van-button>
          <van-button size="small" type="primary" @click="show=false">继续购物</van-button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import item from "../components/item";
export default {
  name: "",
  mounted() {
    this.getnav(),
      this.getlist(),
      window.addEventListener("scroll", this.huadong);
  },
  data() {
    return {
      navList: [],
      list: [],
      sortlist: [],
      active: 0,
      value: "",
      srot: false,
      top: false,
      show: false,
      item: "",
    };
  },
  components: { item },
  methods: {
    getnav() {
      this.$axios("/fenlei.json").then((res) => {
        console.log(res);
        this.navList = res.data.result;
        console.log(res.data.result);
      });
    },
    getlist() {
      this.$axios("/list.json").then((res) => {
        console.log(res.data.result);
        this.list = res.data.result;
      });
    },
    paixu() {
      this.srot = !this.srot;
    },
    push() {
      this.$router.push({
        path: "/search",
      });
    },
    huadong() {
      //   console.log(document.documentElement.scrollTop);
      if (document.documentElement.scrollTop > 200) {
        this.top = true;
      } else if (document.documentElement.scrollTop < 200) {
        this.top = false;
      }
    },
    xiangshang() {
      console.log(top);
      var dsq = setInterval(() => {
        console.log(1);
        var top = document.documentElement.scrollTop;
        document.documentElement.scrollTop = top - 10;
        if(top==0){
            window.clearInterval(dsq)
        }
      }, 10);

      //   document.documentElement.scrollTop = 0;
    },
    xianshi(item) {
      this.item = item;
      this.show = true;
    },
  },
  computed: {
    newlist() {
      var id = this.navList[this.active]._id;
      console.log(id);
      var arr = this.list.filter((item) => {
        return item.cid == id;
      });

      arr.sort((a, b) => {
        if (this.srot == true) {
          return a.price - b.price;
        } else {
          return b.price - a.price;
        }
      });

      console.log(arr);
      return arr;
    },
  },
};
</script>

<style scoped lang="scss">
.header {
  width: 100%;
  height: 230px;
  background: white;
  img {
    width: 100%;
  }
}
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
.title {
  text-indent: 12px;
}
.nav {
  display: flex;
  li {
    margin: 10px;
  }
}
.top {
  position: fixed;
  bottom: 10px;
  right: 10px;
  width: 50px;
  height: 50px;
  background: white;
  border-radius: 50%;
}
.tit {
  width: 100%;
}
.shop {
  width: 100%;
  height: 200px;
  background: white;
  position: fixed;
  bottom: -200px;
  display: flex;
  img {
    width: 50%;
  }
  p {
    font-size: 10px;
    line-height: 20px;
  }
  div > div {
    display: flex;
  }
}

.shopshow {
  transform: translateY(-100%);
  transition: all 0.8s;
}

.shophide {
  transform: translateY(100%);
  transition: all 0.8s;
}
</style>
