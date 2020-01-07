<template>
    <div class="branddetail">
        <div class="banner">
            <img src="http://yanxuan.nosdn.127.net/1541445967645114dd75f6b0edc4762d.png" alt="">
            <div class="info">
                <span>限时抢购</span>
            </div>
        </div>
        <div class="info-desc">
          
        </div>
        <div v-if="goodsList.length!=0" class="sortlist">
            <div v-for="(item, index) in goodsList" :key="index" class="item">
                <img :src="item.list_pic_url" alt="">
                <p class="name">{{item.name}}</p>
                <p class="price">￥{{item.retail_price}}</p>
            </div>
        </div>
        <div v-else class="none">

        </div>
    </div>
</template>

<script>
import { get,myGet } from "../../utils";
export default {
  created() {},
  mounted() {
    this.id = this.$root.$mp.query.id;
    this.getDetail();
    this.getKillGoods();
    this.getInsGoods();
  },
  data() {
    return {
      id: "",
      detailData: {},
      goodsList: []
    };
  },
  components: {},
  methods: {
    getKillGoods(){
      myGet('item/listShow').then( data => {
        console.log(data);
        this.goodsList = data
      } );
    },
    jumpDetail(spuid){
      wx.navigateTo({
        url: "/pages/goods/main?id=" + spuid
      });
    },
    getInsGoods(){
      get('http://192.168.0.107:8080/item/detail/1').then( data => {
        data.forEach( item => {
          if (item.spu === null){
            item.spu = {}
          }
        })
        this.getInsGoods = data
      });
    },
    async getDetail() {
      const data = await get("/brand/detailaction", { id: this.id });
      this.detailData = data.data;
      this.goodsList = data.goodsList;
    }
  },
  computed: {}
};
</script>
<style lang='scss' scoped>
@import "./style";
</style>
