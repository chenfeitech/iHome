<template>
  <view class="furniture-list">
      <scroll-view
        class="furniture-list-scroll"
        :scroll-y="true"
        :enable-back-to-top="true"
        v-if="furnitureList.length > 0"
        @scrolltolower="getMoreFurnitures">
        <furniture-item
          v-for="(furniture, index) in furnitureList"
          :key="index"
          :furniture="furniture"
        ></furniture-item>
      </scroll-view>
      <view
        class="furniture-list-empty"
        v-if="furnitureList.length === 0">
        暂无数据
      </view>
  </view>
</template>

<script>
export default {
    data() {
        return {
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
                type: 0,
                take: 15,
                start: 0
            }
        }
    },
    onLoad(object) {
        if(object.type && object.name) {
            uni.setNavigationBarTitle({
                title: object.name
            })
            this.reset();
            this.params.type = object.type;
            this.getFurnitureList();
        }
    },
    methods: {
        getFurnitureList(isSlient) {
            if(isSlient) {
                this.params.start += this.params.take;
            } else {
                this.params.start = 0;
            }
            let params = Object.assign({}, this.params);
            console.log(params)
        },
        getMoreFurnitures() {
            this.getFurnitureList(true)
        },
        reset() {
            Object.assign(this.params, {
                type: 0,
                take: 15,
                start: 0
            })
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
  .furniture-list {
    height: 100%;
    box-sizing: border-box;
    .furniture-list-scroll {
        height: 100%;
        padding-bottom: 36rpx;
        box-sizing: border-box;
    }
    furniture-item {
        width: calc(50% - 36rpx);
        margin-top: 36rpx;
        display: inline-block;
        &:nth-of-type(2n) {
            margin-right: 36rpx;
        }
        &:nth-of-type(2n+1) {
            margin-left: 36rpx;
        }
        &:nth-last-of-type(1), &:nth-last-of-type(2) {
            margin-bottom: 36rpx;
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
</style>
