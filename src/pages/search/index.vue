<template>
  <view class="search-page">
    <view class="search-area">
      <input
        class="search-input"
        :value="params.keyword"
        type="text"
        :placeholder="placeholder"
        confirm-type="search"
        :adjust-position="false"
        @input="handleInput"
        @focus="handleFocus"
        @blur="handleBlur"
        @confirm="handleSearch" />
      <view
        class="search-btn"
        @click="handleSearch">
        <image class="search-icon" src="../../static/icons/tabBar/search.png"></image>
      </view>
      <view
        class="search-menu"
        @click="triggerSearchMenu"
        v-if="!isFocus && params.keyword">
        <image class="search-icon" src="../../static/icons/tabBar/menu.png"></image>
      </view>
      <uniDrawer
        ref="uniDrawer"
        mode="right"
        :width="300">
        <view class="filter-list">
          <view class="filter-header">
            Filters
          </view>
          <view class="filter-item">
            <view class="filter-title">
              Price
            </view>
            <view class="filter-select-slider">
              <view class="select-slider-title">
                最低价: {{params.minPrice}}
              </view>
              <slider
                class="select-item-slider"
                :value="params.minPrice"
                step="100"
                min="0"
                max="10000"
                @changing="sliderChangeMin"
                @change="sliderChangeMin"/>
              <view class="select-slider-title">
                最高价: {{params.maxPrice}}
              </view>
              <slider
                class="select-item-slider"
                :value="params.maxPrice"
                step="100"
                :min="params.minPrice > 0 ? params.minPrice : 0"
                max="10000"
                @changing="sliderChangeMax"
                @change="sliderChangeMax"/>
            </view>
          </view>
          <view class="filter-item">
            <view class="filter-title">
              Color
            </view>
            <scroll-view
              class="filter-select-scroll"
              :scroll-x="true">
              <view class="select-item-circus-wrapper">
                <view
                  class="select-item-circus"
                  :class="{'is-select': params.color.indexOf(color) > -1}"
                  :style="{'background-color': params.color.indexOf(color) > -1 ? color : '#fff',
                          'border-color': params.color.indexOf(color) > -1 ? color : '#8f92a1'}"
                  v-for="(color, index) in colors"
                  :key="index"
                  @click="handleColor(color)">
                  <view
                    class="circus-inside"
                    :style="{'background-color': color}">
                    <image
                      src="../../static/icons/selected.png"
                      v-if="params.color.indexOf(color) > -1"
                      ></image>
                  </view>
                </view>
              </view>
            </scroll-view>
          </view>
          <view class="filter-item">
            <view class="filter-title">
              Style
            </view>
            <view class="filter-select-flex">
              <view
                class="select-item-square"
                :class="{'is-select': params.style === style}"
                v-for="(style, index) in styles"
                :key="index"
                @click="handleStyle(style)">
                {{style}}
              </view>
            </view>
          </view>
          <view class="filter-footer">
            <button
              class="filter-btn clear"
              hover-class="filter-btn-hover"
              size="mini"
              @click="handleClearFilter">
              Clear All
            </button>
            <button
              class="filter-btn confirm"
              hover-class="filter-btn-hover"
              size="mini"
              @click="search(false)">
              Confirm
            </button>
          </view>
        </view>
      </uniDrawer>
    </view>
    <view class="search-list">
      <scroll-view
        class="search-list-scroll"
        :scroll-y="true"
        :enable-back-to-top="true"
        v-if="furnitureList.length > 0"
        @scrolltolower="getMoreSearchList">
        <furniture-item
          v-for="(furniture, index) in furnitureList"
          :key="index"
          :furniture="furniture"
        ></furniture-item>
      </scroll-view>
      <view
        class="search-list-empty"
        v-if="furnitureList.length === 0">
        暂无数据
      </view>
    </view>
  </view>
</template>

<script>
import {uniDrawer} from '@dcloudio/uni-ui'
export default {
  components: {
    uniDrawer
  },
  data() {
    return {
      isFocus: false,
      isUniDrawerShow: false,
      placeholder: "简约蓝",
      colors: ['#E73E46', '#B37E41', '#78452E', '#59ADFF', '#F8C822', '#90C418', '#686868', '#B5E742', '#000000'],
      styles: ['简约', '时尚', '清新', '暗黑', '欧式'],
      furnitureList: [
        {
          id: "001",
          price: 2620.124,
          imgUrl: "/static/furniture/red-chair.png",
          isCollect: false,
          name: "Cantilever chair",
          introduction: "Latest and New chair"
        }, {
          id: "002",
          price: 3520.4,
          imgUrl: "/static/furniture/white-chair.png",
          isCollect: false,
          name: "Key Chair",
          introduction: "Latest and New chair"
        }, {
          id: "003",
          price: 1407.48,
          imgUrl: "/static/furniture/yellow-chair.png",
          isCollect: false,
          name: "Wing chair",
          introduction: "Latest and New chair"
        }, {
          id: "004",
          price: 200,
          imgUrl: "/static/furniture/green-chair.png",
          isCollect: false,
          name: "Windsor chair",
          introduction: "Latest and New chair"
        }, {
          id: "005",
          price: 100,
          imgUrl: "/static/furniture/black-chair.png",
          isCollect: false,
          name: "Key Chair",
          introduction: "Latest and New chair"
        }, {
          id: "006",
          price: 150,
          imgUrl: "/static/furniture/brown-chair.png",
          isCollect: false,
          name: "Wing chair",
          introduction: "Latest and New chair"
        }
      ],
      params: {
        keyword: "",
        minPrice: 0,
        maxPrice: 0,
        color: [],
        style: ""
      },
      start: 0,
      take: 15
    }
  },
  onLoad(object) {
    if(object.keyword) {
      this.params.keyword = object.keyword;
      this.handleClearFilter();
      this.search();
    }
  },
  methods: {
    handleInput({ detail }) {
        this.$set(this.params, 'keyword', detail.value);
    },
    handleFocus() {
      this.isFocus = true;
    },
    handleBlur() {
      this.isFocus = false;
    },
    handleSearch() {
      this.isFocus = false;
      if(!this.params.keyword) {
        this.$set(this.params, 'keyword', this.placeholder);
      }
      this.search();
    },
    search(isSlient) {
      let params = Object.assign({}, this.params);
      if(isSlient) {
        params.start = this.start + this.take;
        this.start += this.take;
      } else {
        params.start = 0;
        this.start = 0;
      }
      console.log(params)
    },
    triggerSearchMenu() {
      if(this.isUniDrawerShow) {
        this.isUniDrawerShow = false
        this.$refs.uniDrawer.close()
      } else {
        this.isUniDrawerShow = true
        this.$refs.uniDrawer.open()
      }
    },
    sliderChangeMin({ detail }) {
      this.params.minPrice = detail.value;
    },
    sliderChangeMax({ detail }) {
      this.params.maxPrice = detail.value;
    },
    handleColor(color) {
      let index = this.params.color.indexOf(color);
      if(index > -1) {
        this.params.color.splice(index, 1);
      } else {
        this.params.color.push(color)
      }
    },
    handleStyle(style) {
      if(this.params.style === style) {
        this.params.style = "";
      } else {
        this.params.style = style;
      }
    },
    handleClearFilter() {
      Object.assign(this.params, {
        minPrice: 0,
        maxPrice: 0,
        color: [],
        style: ""
      });
    },
    getMoreSearchList() {
      this.search(true);
    }
  }
}
</script>

<style lang="scss">
page {
  height: 100%;
}
</style>
<style lang="scss" scope>
  .search-page {
    height: 100%;
    .search-area {
      margin-top: 24rpx;
      padding: 0 20rpx;
      width: 100%;
      display: flex;
      justify-content: space-between;
      box-sizing: border-box;
      .search-input {
        flex-grow: 1;
        height: 64rpx;
        margin-right: 20rpx;
        padding: 8rpx 36rpx;
        box-sizing: border-box;
        border-radius: 36rpx;
        border: 2rpx solid #ddd;
      }
      .search-btn, .search-menu {
        width: 64rpx;
        height: 64rpx;
        padding: 0;
      }
      .search-menu {
        margin-left: 12rpx;
      }
      .search-icon {
        width: 100%;
        height: 100%;
      }
      .filter-list {
        width: 100%;
        max-height: 100%;
        overflow-x: auto;
        color: #202020;
        .filter-header {
          margin: 76rpx 0 88rpx;
          text-align: center;
          font-size: 60rpx;
          font-weight: bold;
          line-height: 72rpx;
        }
        .filter-item {
          margin-bottom: 24rpx;
          display: flex;
          flex-direction: column;
          align-items: center;
          .filter-title {
            margin-bottom: 32rpx;
            font-size: 32rpx;
            line-height: 44rpx;
          }
          .filter-select-slider {
            width: 100%;
            padding: 0 56rpx;
            box-sizing: border-box;
          }
          .select-item-slider {
            width: 100%;
            margin: 16rpx 0 24rpx;
          }
          .select-slider-title {
            margin-top: 24rpx;
            font-size: 28rpx;
            line-height: 40rpx;
          }
          .filter-select-scroll {
            width: 100%;
            height: 96rpx;
          }
          .select-item-circus-wrapper {
            white-space: nowrap;
          }
          .select-item-circus {
            width: 96rpx;
            height: 96rpx;
            display: inline-block;
            box-sizing: border-box;
            border-radius: 50%;
            border: 1px solid #8f92a1;
            background-color: #fff;
            color: #202020;
            font-size: 38rpx;
            line-height: 94rpx;
            text-align: center;
            position: relative;
            &.is-select {
              border-color: #FFAD00;
              background-color: #FFAD00;
              color: #fff;
            }
            & + .select-item-circus {
              margin-left: 38rpx;
            }
            &:nth-of-type(1) {
              margin-left: 56rpx;
            }
            &:nth-last-of-type(1) {
              margin-right: 56rpx;
            }
          }
          .circus-inside {
            width: 50rpx;
            height: 50rpx;
            border-radius: 25rpx;
            position: absolute;
            left: 22rpx;
            top: 22rpx;
            display: flex;
            justify-content: center;
            align-items: center;
            image {
              width: 44rpx;
              height: 44rpx;
            }
          }
          .filter-select-flex{
            display: flex;
            flex-wrap: wrap;
            margin: 0 56rpx;
            width: calc(100% - 112rpx);
            box-sizing: border-box;
          }
          .select-item-square {
            width: 30%;
            height: 80rpx;
            margin-right: 5%;
            border-radius: 16rpx;
            box-sizing: border-box;
            background-color: #fff;
            border: 1px solid #8f92a1;
            color: #202020;
            font-size: 28rpx;
            line-height: 78rpx;
            text-align: center;
            &.is-select {
              background-color: #FFAD00;
              border-color: #FFAD00;
              color: #fff;
            }
            &:nth-of-type(3n) {
              margin-right: 0;
            }
            &:nth-of-type(n+4) {
              margin-top: 40rpx;
            }
          }
        }
        .filter-footer {
          margin: 88rpx auto 56rpx;
          text-align: center;
          .filter-btn {
            width: 200rpx;
            height: 80rpx;
            font-size: 24rpx;
            border-radius: 40rpx;
            text-align: center;
            line-height: 80rpx;
            &.clear {
              border-top-right-radius: 0;
              border-bottom-right-radius: 0;
              background-color: #eee;
              color: #999;
            }
            &.confirm {
              border-top-left-radius: 0;
              border-bottom-left-radius: 0;
              background-color: #FFAD00;
              color: #404040;
            }
          }
        }
      }
    }
    .search-list {
      height: calc(100% - 88rpx);
      padding-top: 20rpx;
      box-sizing: border-box;
      .search-list-scroll {
        height: calc(100% - 20rpx);
      }
      furniture-item {
        width: calc(50% - 36rpx);
        display: inline-block;
        &:nth-of-type(n+3) {
          margin-top: 36rpx;
        }
        &:nth-of-type(2n) {
          margin-right: 36rpx;
        }
        &:nth-of-type(2n+1) {
          margin-left: 36rpx;
        }
      }
      &-empty {
        font-size: 32rpx;
        font-weight: bold;
        text-align: center;
        margin-top: 40rpx;
        letter-spacing: 0.1rem;
        color: #202020;
      }
    }
  }
</style>