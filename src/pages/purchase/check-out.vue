<template>
    <view class="checkout-page">
        <view class="checkout-content">
            <view class="delivery-info">
                <view class="delivery-person">
                    {{delivery.name}} - {{delivery.phone}}
                </view>
                <view class="delivery-location">
                    <image
                        class="location-icon"
                        src="/static/icons/location.png"
                    ></image>
                    {{delivery.areaText}} {{delivery.address}}
                </view>
                <view
                    class="delivery-replace-btn"
                    @click="replaceDelivery">
                    replace
                </view>
            </view>
            <view class="good-list">
                <view
                    class="good-item"
                    v-for="(item, index) in goodList"
                    :key="index">
                    <view class="good-item-name">
                        {{item.name}}
                    </view>
                    <view class="good-item-bottom">
                        <text class="good-item-type-info">
                            {{item.typeDetail}}
                        </text>
                        <text class="good-item-price">
                            ￥ {{item.price}} × {{item.purchaseNumber}}
                        </text>
                    </view>
                </view>
            </view>
        </view>
        <view
            class="pay-wrapper"
            :class="{'is-disabled': isDisabled}">
            <view class="order-price-info">
                <view class="order-price-item">
                    <text class="order-price-title">Subtotal</text>
                    <text class="order-price-detail">￥{{subtotal | priceFilter}}</text>
                </view>
                <view class="order-price-item">
                    <text class="order-price-title">Discount</text>
                    <text class="order-price-detail">￥{{discount | priceFilter}}</text>
                </view>
                <view class="order-price-item">
                    <text class="order-price-title">Freight</text>
                    <text class="order-price-detail">￥{{freight | priceFilter}}</text>
                </view>
            </view>
            <view
                class="pay-btn"
                @click="handlePay">
                Pay
                <text class="pay-total">
                    ￥{{(subtotal - discount + freight) | priceFilter}}
                </text>
            </view>
        </view>
    </view>
</template>

<script>
export default {
    data() {
        return {
            delivery: {
                name: "",
                phone: "",
                address: ""
            },
            deliveryList: [],
            goodList: [],
            isDisabled: false,
            subtotal: 0,
            discount: 0,
            freight: 0
        }
    },
    filters: {
        priceFilter(val) {
            return val.toFixed(2);
        }
    },
    onLoad(obj) {
        let index = obj && obj.deliveryIndex || 0,
            orderId = obj && obj.orderId || null;
        this.getDeliveryInfo(index);
        this.getOrderDetail(orderId);
    },
    methods: {
        getDeliveryInfo(index) {
            this.deliveryList = [
                {
                    name: "lanye",
                    phone: "13700245898",
                    areaText: "广东省 深圳市 南山区",
                    address: "一二街道三四五路7号 八九大厦"
                }, {
                    name: "zhan",
                    phone: "13200235128",
                    areaText: "广东省 潮州市 饶平县",
                    address: "一二镇三四五村六七街道89号"
                }, {
                    name: "summary",
                    phone: "13531541789",
                    areaText: "福建省 福州市 一二县",
                    address: "三四五镇六七村八八街道9号"
                }
            ];
            if(index) {
                this.delivery = this.deliveryList[index];
            } else {
                this.delivery = this.deliveryList[0];
            }
        },
        replaceDelivery() {
            uni.redirectTo({
                url: '/pages/account/address?fromCheckout=true'
            })
        },
        getOrderDetail(orderId) {
            this.goodList = [
                {
                    id: "001",
                    name: "Red Chair Red ChairRed Chair",
                    price: 3250.789,
                    typeDetail: "red",
                    purchaseNumber: 2
                }, {
                    id: "002",
                    name: "Black Chairlack Chair",
                    price: 124.19,
                    typeDetail: "black, black black",
                    purchaseNumber: 1
                }, {
                    id: "003",
                    name: "Brown Chair",
                    price: 379.9,
                    typeDetail: "brown",
                    purchaseNumber: 8
                }, {
                    id: "004",
                    name: "Green Chair Chair",
                    price: 99.99,
                    typeDetail: "green, greengreen",
                    purchaseNumber: 22
                }, {
                    id: "005",
                    name: "White Chair White Chair",
                    price: 1285.02,
                    typeDetail: "white",
                    purchaseNumber: 1
                }
            ];
            this.subtotal = 127548.125;
            this.discount = 210.45;
            this.freight = 120;
        },
        handlePay() {
            this.isDisabled = true;
            setTimeout(() => {
                this.isDisabled = false;
            }, 3000)
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
.checkout-page {
    height: 100%;
    background-color: #f8f8f8;
    .checkout-content {
        margin: 30rpx auto 0;
        height: calc(100% - 430rpx);
        width: calc(100% - 40rpx);
        border-radius: 8px;
        background: #ffffff;
        display: flex;
        flex-direction: column;
        box-shadow: 0px 5px 10px rgba(0, 0, 0, 0.1);
        .delivery-info {
            width: 100%;
            padding: 32rpx 180rpx 24rpx 36rpx;
            box-sizing: border-box;
            background-color: #f8f8f8;
            border-bottom: 1rpx solid #f0f0f0;
            position: relative;
            .delivery-person {
                font-size: 40rpx;
                line-height: 48rpx;
                margin-bottom: 12rpx;
                color: #333;
            }
            .delivery-location {
                font-size: 30rpx;
                line-height: 40rpx;
                color: #7c7878;
                display: flex;
                align-items: center;
                .location-icon {
                    width: 40rpx;
                    height: 40rpx;
                    min-width: 40rpx;
                    margin-right: 8rpx;
                }
            }
            .delivery-replace-btn {
                position: absolute;
                right: 20rpx;
                top: calc(50% - 25rpx);
                width: 140rpx;
                height: 50rpx;
                line-height: 50rpx;
                border-radius: 20rpx;
                text-align: center;
                color: #fff;
                background-color: #FFAD00;
            }
        }
        .good-list {
            width: 100%;
            flex-grow: 1;
            padding-left: 36rpx;
            box-sizing: border-box;
            overflow: hidden;
            overflow-y: auto;
            .good-item {
                width: 100%;
                height: 130rpx;
                padding: 24rpx 0;
                padding-right: 32rpx;
                box-sizing: border-box;
                border-bottom: 1rpx solid #f0f0f0;
                display: flex;
                flex-direction: column;
                justify-content: space-between;
            }
            .good-item-name {
                font-size: 30rpx;
                line-height: 40rpx;
                color: #555;
            }
            .good-item-bottom {
                display: inline-flex;
                justify-content: space-between;
                align-items: center;
                font-size: 26rpx;
                line-height: 34rpx;
                color: #888;
            }
        }
    }
    .pay-wrapper {
        position: fixed;
        bottom: 0;
        left: 0;
        width: 100%;
        height: 360rpx;
        background-color: #fff;
        .order-price-info {
            padding: 0 30rpx 24rpx;
            border-bottom: 1rpx solid #f0f0f0;
        }
        .order-price-item {
            display: flex;
            justify-content: space-between;
            font-size: 30rpx;
            line-height: 40rpx;
            color: #474040;
            margin-top: 24rpx;
        }
        .pay-btn {
            margin: 24rpx auto 0;
            width: calc(100% - 30rpx);
            height: 88rpx;
            box-sizing: border-box;
            background-color: #FFAD00;
            color: #fff;
            border-radius: 16rpx;
            line-height: 88rpx;
            text-align: center;
            position: relative;
            .pay-total {
                position: absolute;
                right: 60rpx;
                top: 0;
                font-size: 34rpx;
                line-height: 88rpx;
            }
        }
    }
}
</style>