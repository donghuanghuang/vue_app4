<template>
  <div class="goods">
    <div class="menu-wrapper" ref="menuWrapper">
      <ul>
        <li v-for="item in goods" class="menu-item">
          <span class="text">
             <span v-show="item.type>0" class="icon" :class="classMap[item.type]"></span>{{item.name}}
          </span>
        </li>
      </ul>
    </div>
    <div class="foods-wrapper" ref="foodsWrapper">
      <ul>
        <li v-for="item in goods" class="food-list food-list-hook">
          <h1 class="title">{{item.name}}</h1>
          <ul>
            <li  v-for="food in item.foods" class="food-item ">
              <div class="icon1">
                <img width="57" height="57" :src="food.icon">
              </div>
              <div class="content1">
                <h2 class="name1">{{food.name}}</h2>
                <p class="desc">{{food.description}}</p>
                <div class="extra">
                  <span class="count">月售{{food.sellCount}}份</span><span>好评率{{food.rating}}%</span>
                </div>
                <div class="price">
                  <span class="now">￥{{food.price}}</span>
                  <span class="old" v-show="food.oldPrice">￥{{food.oldPrice}}</span>
                </div>
                <div class="cartcontrol-wrapper">
                  <!--<cartcontrol  :food="food"></cartcontrol>-->
                </div>
              </div>
            </li>
          </ul>
        </li>
      </ul>
    </div>
  </div>
</template>
<script type="text/ecmascript-6">
import BScroll from 'better-scroll';
const ERR_OK = 0;
export default{
  props:{
    seller:{
      type:Object
    }
  },
  data(){
    return{
      goods:[]
    }
  },
  created(){
   this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
    this.$http.get('/api/goods').then((response)=>{
        response = response.body;
        if(response.errno === ERR_OK){
          this.goods = response.data;
           this.$nextTick(() => {
            this._initScroll();
          });
        }
    })
  },
  methods:{
    _initScroll(){
        this.meunScroll = new BScroll(this.$refs.menuWrapper, {
          click: true
        });
        this.foodsScroll = new BScroll(this.$refs.foodsWrapper, {
          click: true,
          probeType: 3
        });
        this.foodsScroll.on('scroll', (pos) => {
          this.scrollY = Math.abs(Math.round(pos.y));
        });
    }
  }
}


</script>
<style>
  .goods{
    display: flex;
    position: absolute;
    width: 100%;
    top: 174px;
    bottom: 46px;
    overflow: hidden;
  }
  .menu-wrapper{
    flex: 0 0 80px;
    width: 80px;
    background: #f3f5f7;
  }
  .foods-wrapper{
    flex: 1;
  }
  .menu-item{
    display: table;
    width: 56px;
    height:54px;
    line-height: 14px;
    padding: 0 12px;
  }
  .icon{
  display: inline-block;
  vertical-align: top;
  width: 12px;
  height: 12px;
  margin-right: 2px;
  background-size: 12px 12px;
  background-repeat: no-repeat;
  background-image: url('special_3@2x.png');
  }
  .menu-item .text{
    font-size: 12px;
    display: table-cell;
    width: 56px;
    vertical-align: middle;
  }
  .foods-wrapper .title{
    padding-left: 14px;
    height: 26px;
    line-height: 26px;
    border-left: 2px solid #d9dde1;
    font-size: 12px;
    color: rgb(147,153,159);
    background: #f3f5f7;
  }
  .foods-wrapper .food-item{
    display: flex;
    margin: 18px;
    padding-bottom: 18px;
  }
  .foods-wrapper .food-item:last-child{
    display: none;
    margin-bottom: 0;
  }
  .icon1{
    flex: 0 0 57px;
    margin-right: 10px;
  }
  .content1{
    flex: 1;
  }
  .name1{
    font-size: 14px;
    margin: 2px 0 8px 0;
    height: 14px;
    line-height: 14px;
    color: rgb(7, 17, 27);
  }
  .desc{
    margin-bottom: 8px;
    line-height: 10px;
    font-size: 10px;
    color: rgb(147, 153, 159);
  }
  .extra{
    line-height: 10px;
    font-size: 10px;
    color: rgb(147, 153, 159);
  }
  .count{
    margin-right: 12px;
  }
  .price{
    font-weight: 700;
    line-height: 24px;
  }
  .now{
    margin-right: 8px;
    font-size: 14px;
    color: rgb(240,20,20);
  }
  .old{
    text-decoration: line-through;
    font-size: 10px;
    color: rgb(147,153,159);
  }
</style>
