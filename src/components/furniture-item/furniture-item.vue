<template>
    <view
        class="furniture-item"
        @click="viewFurnitureDetail">
        <view class="furniture-price">
            ￥{{furniture.price | priceFilter}}
        </view>
        <view class="furniture-post">
            <image
                class="furnuture-image"
                :src="furniture.imgUrl"
                mode="aspectFit"
            ></image>
        </view>
        <view class="furniture-collect">
            <image
                class="collect-icon"
                :src="furniture.isCollect ? '/static/icons/collect_active.png' : '/static/icons/collect.png'"
            ></image>
        </view>
        <view
            class="furniture-purchase"
            @click.stop="handlePurchase">
            <image
                class="purchase-icon"
                :src="isPruchasing ? '/static/icons/waiting.png' : '/static/icons/purchase.png'"
            ></image>
        </view>
        <view class="furniture-name">
            {{furniture.name}}
        </view>
        <view class="furniture-introduction">
            {{furniture.introduction}}
        </view>
    </view>
</template>

<script>
export default {
    name: "furniture-item",
    props: {
        furniture: {
            type: Object,
            default: () => {
                return {
                    id: "",
                    price: 0,
                    imgUrl: "",
                    isCollect: false,
                    name: "",
                    introduction: ""
                }
            }
        }
    },
    data() {
        return {
            isPruchasing: false
        }
    },
    filters: {
        priceFilter(val) {
            return val.toFixed(2);
        }
    },
    methods: {
        viewFurnitureDetail() {
            uni.navigateTo({
                url: "/pages/furniture-detail/index?id=" + this.furniture.id
            })
        },
        handlePurchase() {
            this.isPruchasing = true;
            setTimeout(() => {
                this.isPruchasing = false;
            }, 1000)
        }
    }
}
</script>

<style lang="scss" scope>
    .furniture-item {
        width: 100%;
        display: flex;
        flex-direction: column;
        position: relative;
        padding: 0 12rpx;
        padding-top: 6rpx;
        box-sizing: border-box;
        .furniture-price {
            width: 136rpx;
            height: 76rpx;
            line-height: 76rpx;
            text-align: center;
            background-color: #E92C38;
            color: #fff;
            font-size: 20rpx;
            border-radius: 24rpx;
            overflow: hidden;
            position: absolute;
            left: 0;
            top: 0;
        }
        .furniture-post {
            width: 100%;
            display: flex;
            align-items: center;
            justify-content: center;
            box-shadow: 0px 2px 10px rgba(0, 0, 0, 0.1);
            .furnuture-image {
                width: 100%;
            }
        }
        .furniture-collect {
            width: 36rpx;
            height: 34rpx;
            position: absolute;
            right: 40rpx;
            top: 32rpx;
            .collect-icon {
                width: 100%;
                height: 100%;
            }
        }
        .furniture-purchase {
            width: 80rpx;
            height: 80rpx;
            border-radius: 50%;
            overflow: hidden;
            background-color: #FFAD00;
            position: absolute;
            right: 0rpx;
            bottom: 70rpx;
            display: flex;
            align-items: center;
            justify-content: center;
            .purchase-icon {
                width: 40rpx;
                height: 40rpx;
            }
        }
        .furniture-name {
            margin-top: 14rpx;
            line-height: 40rpx;
            font-size: 24rpx;
            color: #000;
            font-weight: bold;
            max-width: 100%;
            overflow: hidden;
            text-overflow: ellipsis;
            white-space: nowrap;
        }
        .furniture-introduction {
            margin-top: 6rpx;
            line-height: 32rpx;
            font-size: 20rpx;
            color: #8e8e8e;
            max-width: 100%;
            overflow: hidden;
            text-overflow: ellipsis;
            white-space: nowrap;
        }
    }
</style>