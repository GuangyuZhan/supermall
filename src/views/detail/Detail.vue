<template>
  <div id="detail">
    <detail-nav-bar class="detail-nav"></detail-nav-bar>
    <scroll class="content" ref="scroll">
      <detail-swiper :top-images="topImages"></detail-swiper>
      <detail-base-info :goods="goods"></detail-base-info>
      <detail-shop-info :shop="shop"></detail-shop-info>
      <detail-goods-info :detail-info="detailInfo" @imageLoad="imageLoad"></detail-goods-info>
      <detail-param-info :param-info="paramInfo"></detail-param-info>
    </scroll>
  </div>
</template>

<script>
  import DetailNavBar from "./childComps/DetailNavBar";
  import DetailSwiper from "./childComps/DetailSwiper";
  import DetailBaseInfo from "./childComps/DetailBaseInfo";
  import DetailShopInfo from "./childComps/DetailShopInfo";
  import DetailGoodsInfo from "./childComps/DetailGoodsInfo";
  import DetailParamInfo from "./childComps/DetailParamInfo";

  import Scroll from "components/common/scroll/Scroll";

  import {getDetail,Goods,Shop,GoodsParam} from "network/detail";

  export default {
    name: "Detail",
    data(){
      return{
        iid:null,
        topImages:[],
        goods:{},
        shop:{},
        detailInfo:{},
        paramInfo:{},
      }
    },
    components:{
      DetailNavBar,
      DetailSwiper,
      DetailBaseInfo,
      DetailShopInfo,
      Scroll,
      DetailGoodsInfo,
      DetailParamInfo
    },
    created() {
      this.iid=this.$route.params.iid

      getDetail(this.iid).then(res=>{
        const data=res.result
        this.topImages=data.itemInfo.topImages

        this.goods=new Goods(data.itemInfo,data.columns,data.shopInfo.services)

        this.shop=new Shop(data.shopInfo)

        this.detailInfo=data.detailInfo;

        this.paramInfo=new GoodsParam(data.itemParams.info,data.itemParams.rule)
      })
    },
    methods:{
      imageLoad(){
        this.$refs.scroll.refresh()
      }
    }
  }
</script>

<style scoped>
  #detail{
    position: relative;
    z-index: 9;
    background-color: #fff;
    height: 100vh;
  }

  .content{
    height: calc(100% - 44px);
  }

  .detail-nav{
    position: relative;
    z-index: 9;
    background-color: #fff;
  }
</style>