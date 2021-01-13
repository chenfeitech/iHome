<template>
    <view class="purchase-page">
        <view class="purchase-page-title">
            <checkbox-group @change="handleCheckAll">
                <label>
                    <checkbox
                    class="check-all-box"
                    style="transform:scale(0.7)"
                    :value="isCheckAll"
                    :checked="isCheckAll"/>
                    My products:
                </label>
            </checkbox-group>
        </view>
        <scroll-view
            class="purchase-list-scroll"
            :scroll-y="true">
            <checkbox-group
                class="purchase-list"
                @change="purchaseItemCheckChange">
                <view
                    class="purchase-item"
                    v-for="(item, index) in purchaseList"
                    :key="index">
                    <checkbox
                        class="purchase-item-check"
                        style="transform:scale(0.7)"
                        :value="index"
                        :checked="item.checked"
                    />
                    <image
                        class="purchase-item-post"
                        :src="item.imgUrl"
                        mode="aspectFit"
                        @click.stop="goToDetail(item.id)"
                    ></image>
                    <view class="purchase-item-right">
                        <view class="purchase-item-name">
                            {{item.name}}
                        </view>
                        <view class="purchase-item-type-info">
                            {{item.typeDetail}}
                        </view>
                        <view class="purchase-item-price">
                            ￥{{item.price | priceFilter}}
                        </view>
                        <view class="purchase-item-operate">
                            <image
                                class="purchase-icon"
                                :class="{'is-disable': item.purchaseNumber <= 1}"
                                @click.stop="reducePurchaseNumber(item)"
                                src="/static/icons/reduce_grey.png"
                            ></image>
                            <text class="purchase-number">
                                {{item.purchaseNumber}}
                            </text>
                            <image
                                class="purchase-icon"
                                @click.stop="addPurchaseNumber(item)"
                                src="/static/icons/add_grey.png"
                            ></image>
                        </view>
                    </view>
                </view>
            </checkbox-group>
        </scroll-view>
        <view
            class="purchase-check-out"
            :class="{'is-check-out': isCheckingOut}"
            @click="handleCheckOut">
            <view class="check-out-text">
                Check Out
                <text
                    class="purchase-total"
                    v-if="purchaseTotalPrice > 0">
                    (Total: {{purchaseTotalPrice | priceFilter}})
                </text>
            </view>
            <image
                class="check-out-icon"
                src="/static/icons/next.png"
            ></image>
        </view>
    </view>
</template>

<script>
export default {
    data() {
        return {
            purchaseList: [],
            isCheckingOut: false,
            isCheckAll: false
        }
    },
    computed: {
        purchaseTotalPrice() {
            let sum = 0;
            this.purchaseList.forEach(item => {
                if(item.checked) {
                    sum += (item.price * item.purchaseNumber)
                }
            })
            return sum
        }
    },
    filters: {
        priceFilter(val) {
            return val.toFixed(2);
        }
    },
    onLoad() {
        this.getPurchaseList();
    },
    methods: {
        getPurchaseList() {
            this.purchaseList = [
                {
                    id: "001",
                    imgUrl: "../../static/furniture/red-chair.png",
                    name: "Red Chair",
                    price: 3250.789,
                    typeDetail: "red",
                    purchaseNumber: 2,
                    checked: false
                }, {
                    id: "002",
                    imgUrl: "../../static/furniture/black-chair.png",
                    name: "Black Chair",
                    price: 124.19,
                    typeDetail: "black",
                    purchaseNumber: 1,
                    checked: false
                }, {
                    id: "003",
                    imgUrl: "../../static/furniture/brown-chair.png",
                    name: "Brown Chair",
                    price: 379.9,
                    typeDetail: "brown",
                    purchaseNumber: 8,
                    checked: false
                }, {
                    id: "004",
                    imgUrl: "../../static/furniture/green-chair.png",
                    name: "Green Chair",
                    price: 99.99,
                    typeDetail: "green",
                    purchaseNumber: 22,
                    checked: false
                }, {
                    id: "005",
                    imgUrl: "../../static/furniture/white-chair.png",
                    name: "White Chair",
                    price: 1285.02,
                    typeDetail: "white",
                    purchaseNumber: 1,
                    checked: false
                }
            ];
        },
        handleCheckAll({detail}) {
            let isCheckAll = Boolean(detail.value[0]);
            this.isCheckAll = isCheckAll;
            this.purchaseList.forEach(item => {
                this.$set(item, 'checked', isCheckAll);
            })
        },
        purchaseItemCheckChange({ detail }) {
            let list = detail.value;
            this.purchaseList.forEach(item => {
                item.checked = false;
            })
            list.forEach(item => {
                this.purchaseList[item].checked = true;
            })
            this.isCheckAll = this.purchaseList.every(item => item.checked)
        },
        goToDetail(id) {
            uni.navigateTo({
                url: `/pages/furniture-detail/index?id=${id}`
            })
        },
        reducePurchaseNumber(item) {
            if(item.purchaseNumber <= 1) {
                return;
            } else {
                item.purchaseNumber--;
            }
        },
        addPurchaseNumber(item) {
            item.purchaseNumber++;
        },
        handleCheckOut() {
            this.isCheckingOut = true;
            setTimeout(() => {
                this.isCheckingOut = false;
                uni.navigateTo({
                    url: "/pages/purchase/check-out"
                })
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
    .purchase-page {
        height: 100%;
        .purchase-page-title {
            margin: 15rpx 0;
            margin-left: 40rpx;
            font-size: 40rpx;
            line-height: 48rpx;
            color: #000;
            .check-all-box {
                margin-left: 10rpx;
                margin-right: 12rpx;
            }
        }
        .purchase-list-scroll {
            height: calc(100% - 78rpx);
            .purchase-list {
                width: 100%;
                padding: 0 40rpx 140rpx;
                box-sizing: border-box;
                display: flex;
                flex-direction: column;
            }
            .purchase-item {
                width: 100%;
                height: 240rpx;
                border-bottom: 2rpx solid #c3c3c3;
                box-sizing: border-box;
                display: flex;
                &-check {
                    width: 80rpx;
                    height: 100%;
                    line-height: 238rpx;
                    text-align: center;
                }
                &-post {
                    width: 238rpx;
                    height: 238rpx;
                    padding: 20rpx;
                    box-sizing: border-box;
                }
                &-right {
                    flex-grow: 1;
                    display: inline-flex;
                    flex-direction: column;
                    padding: 20rpx 40rpx 20rpx 0;
                }
                &-name {
                    color: #474040;
                    font-size: 36rpx;
                    line-height: 42rpx;
                    text-overflow: ellipsis;
                }
                &-price {
                    margin-top: 12rpx;
                    color: #FFAD00;
                    font-size: 32rpx;
                    line-height: 36rpx;
                }
                &-type-info {
                    margin-top: 12rpx;
                    color: #7C7878;
                    font-size: 28rpx;
                    line-height: 32rpx;
                }
                &-operate {
                    margin-top: 20rpx;
                    display: inline-flex;
                    align-items: center;
                    .purchase-icon {
                        width: 40rpx;
                        height: 40rpx;
                    }
                    .purchase-number {
                        margin: 0 28rpx;
                        font-size: 34rpx;
                        line-height: 40rpx;
                        color: #7C7878;
                    }
                }
            }
        }
        .purchase-check-out {
            width: calc(100% - 80rpx);
            height: 128rpx;
            padding: 0 44rpx;
            box-sizing: border-box;
            position: fixed;
            left: 40rpx;
            bottom: 16rpx;
            background-color: #FFAD00;
            color: #fff;
            border-radius: 30rpx;
            display: flex;
            align-items: center;
            justify-content: space-between;
            &.is-check-out {
                background-color: #808080;
                color: #333;
            }
            .check-out-text {
                flex-grow: 1;
                font-size: 40rpx;
                line-height: 48rpx;
            }
            .purchase-total {
                margin-left: 12rpx;
                font-size: 28rpx;
            }
            .check-out-icon {
                width: 48rpx;
                height: 30rpx;
            }
        }
    }
</style>