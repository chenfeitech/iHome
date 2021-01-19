<template>
    <view class="wish-list">
        <template v-if="wishList.length">
            <view
                class="wish-item"
                v-for="(item, index) in wishList"
                :key="index"
                @click="goToDetail(item.id)"
                @longpress="showMenu(index)">
                <image
                    class="wish-item-post"
                    :src="item.imgUrl"
                    mode="aspectFit"
                ></image>
                <view class="wish-item-right">
                    <view class="wish-item-name">{{item.name}}</view>
                    <view class="wish-item-price">{{item.price | priceFilter}}</view>
                    <view class="wish-item-intro">{{item.introduction}}</view>
                </view>
            </view>
        </template>
        <view class="list-empty" v-else>
            暂无数据
        </view>
    </view>
</template>

<script>
export default {
    data() {
        return {
            wishList: [],
            options: [
                {
                    text: '取消收藏',
                    style: {
                        backgroundColor: '#E92C38'
                    }
                }
            ]
        }
    },
    filters: {
        priceFilter(val) {
            return val.toFixed(2);
        }
    },
    onLoad() {
        this.getWishList();
    },
    methods: {
        getWishList() {
            this.wishList = [
                {
                    id: "001",
                    price: 2620.124,
                    imgUrl: "/static/furniture/red-chair.png",
                    name: "Cantilever chair",
                    introduction: "Latest and New chair"
                }, {
                    id: "002",
                    price: 3520.4,
                    imgUrl: "/static/furniture/white-chair.png",
                    name: "Key Chair",
                    introduction: "Latest and New chair"
                }, {
                    id: "003",
                    price: 1407.48,
                    imgUrl: "/static/furniture/yellow-chair.png",
                    name: "Wing chair",
                    introduction: "Latest and New chair"
                }, {
                    id: "004",
                    price: 200,
                    imgUrl: "/static/furniture/green-chair.png",
                    name: "Windsor chair",
                    introduction: "Latest and New chair"
                }, {
                    id: "005",
                    price: 100,
                    imgUrl: "/static/furniture/black-chair.png",
                    name: "Key Chair",
                    introduction: "Latest and New chair"
                }, {
                    id: "006",
                    price: 150,
                    imgUrl: "/static/furniture/brown-chair.png",
                    name: "Wing chair",
                    introduction: "Latest and New chair"
                }
            ];
        },
        goToDetail(id) {
            uni.navigateTo({
                url: "/pages/furniture-detail/index?id=" + id
            })
        },
        showMenu(index) {
            let _self = this;
            uni.showActionSheet({
                itemList: ['取消收藏'],
                success: function (res) {
                    if(res.tapIndex === 0) {
                        _self.cancelCollect(index);
                    }
                },
                fail: function (res) {}
            });
        },
        cancelCollect(index) {
            uni.showLoading({
                title: "取消收藏中"
            });
            setTimeout(() => {
                uni.hideLoading();
                this.wishList.splice(index, 1)
                uni.showToast({
                    title: "取消收藏成功"
                });
            }, 3000)
        }
    }
}
</script>

<style lang="scss" scope>
.wish-list {
    width: 100%;
    padding: 40rpx 0;
    box-sizing: border-box;
    .wish-item {
        width: 100%;
        height: 311rpx;
        padding: 44rpx 36rpx;
        display: flex;
        justify-content: space-between;
        box-sizing: border-box;
        border-bottom: 1rpx solid #ddd;
        .wish-item-post {
            width: 222rpx;
            height: 222rpx;
            min-width: 222rpx;
            min-height: 222rpx;
            border-radius: 40rpx;
        }
        .wish-item-right {
            flex-grow: 1;
            margin-left: 32rpx;
        }
        .wish-item-name, .wish-item-price {
            font-size: 32rpx;
            line-height: 48rpx;
            color: #000;
            font-weight: bold;
            margin-bottom: 6rpx;
        }
        .wish-item-price {
            margin-bottom: 12rpx;
        }
        .wish-item-intro {
            font-size: 28rpx;
            line-height: 42rpx;
            color: #8e8e8e;
            max-height: 84rpx;
            overflow : hidden;
            text-overflow: ellipsis;
            display: -webkit-box;
            -webkit-line-clamp: 2;
            -webkit-box-orient: vertical;
        }
    }
    .list-empty {
        font-size: 32rpx;
        color: #666;
        line-height: 44rpx;
        text-align: center;
        margin-top: 80rpx;
    }
}
</style>