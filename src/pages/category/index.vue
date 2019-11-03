<template>
  <div>
    <SearchHotspot />
    <div class="content">
      <div class="left">
        <ul>
          <li v-for="(item, index) in cateList"
              :key="item.cat_id"
              :class="{active:index==activeIndex}"
              @click="activeIndex=index">{{item.cat_name}}</li>
        </ul>
      </div>
      <div class="right">
        <img src="../../../static/icon/titleImage.png"
             alt="">
        <template v-if="cateList[activeIndex]">
          <div class="cate2"
               v-for="(item2, index2) in cateList[activeIndex].children"
               :key="item2.cat_id">
            <template v-if="item2.children">
              <p class="title">/<span>{{item2.cat_name}}</span>/</p>
              <ul>
                <li v-for="(item3, index3) in item2.children"
                    :key="item3.cat_id"
                    @click="toList(item3.cat_name)">
                  <img :src="item3.cat_icon"
                       alt="">
                  <span>{{item3.cat_name}}</span>
                </li>
              </ul>
            </template>
          </div>
        </template>
      </div>
    </div>
  </div>
</template>

<script>
import SearchHotspot from '@/components/search_hotspot'
export default {
  data () {
    return {
      activeIndex: 0,
      cateList: []
    }
  },
  components: {
    SearchHotspot
  },
  created () {
    this.getCateList()
  },
  methods: {
    getCateList () {
      this.$request({
        url: '/api/public/v1/categories'
      }).then(res => {
        // console.log(res)
        let { message, meta } = res.data
        if (meta.status === 200) {
          this.cateList = message
        }
      })
    },
    toList (name) {
      wx.navigateTo({ url: `/pages/list/main?keyword=${name}` })
    }
  }
}
</script>

<style lang="less">
.content {
  position: absolute;
  left: 0;
  right: 0;
  bottom: 0;
  top: 100rpx;
  display: flex;

  .left {
    width: 198rpx;
    background-color: #f4f4f4;
    overflow: auto;
    li {
      height: 100rpx;
      color: #333;
      border-bottom: 1rpx solid #eee;
      display: flex;
      justify-content: center;
      align-items: center;
      &.active {
        background-color: #fff;
        color: #eb4450;
        position: relative;
        &::before {
          position: absolute;
          content: "";
          width: 8rpx;
          height: 60rpx;
          background-color: #eb4450;
          left: 0;
        }
      }
    }
  }
  .right {
    flex: 1;
    overflow: auto;
    padding: 20rpx 16rpx;
    img {
      width: 520rpx;
      height: 180rpx;
    }
  }

  .cate2 {
    .title {
      height: 100rpx;
      line-height: 100rpx;
      text-align: center;
      color: #e0e0e0;
      span {
        color: #333;
        margin: 0 30rpx;
      }
    }
    ul {
      display: flex;
      flex-wrap: wrap;
      margin-bottom: 30rpx;
      li {
        width: 33.33%;
        display: flex;
        flex-direction: column;
        align-items: center;
        margin-bottom: 20rpx;
        img {
          width: 120rpx;
          height: 120rpx;
          margin-bottom: 10rpx;
        }
      }
    }
  }
}
</style>