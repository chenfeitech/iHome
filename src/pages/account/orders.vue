<template>
    <view class="order-page">
        <view class="order-status-tabs">
            <view
                class="status-tab"
                :class="{'is-active': params.tabIndex === tab.id}"
                v-for="(tab, index) in tabList"
                :key="index"
                @click="handleStatus(tab.id)">
                {{tab.name}}
            </view>
        </view>
        <scroll-view
            class="order-list-wrapper"
            :scroll-y="true"
            @scrolltolower="getOrderList(true)">
            <view class="order-list" v-if="orderList.length">
                <view
                    class="order-item"
                    v-for="order in orderList"
                    :key="order.id">
                    <view
                        class="furniture-info"
                        v-for="(furniture, index) in order.list.slice(0, 3)"
                        :key="index">
                        <image
                            class="furniture-post"
                            mode="aspectFit"
                            :src="furniture.imgUrl"
                        ></image>
                        <view class="furniture-name">
                            {{furniture.name}}
                        </view>
                        <view class="furniture-count">
                            ×{{furniture.purchaseNumber}}
                        </view>
                    </view>
                    <view class="order-info">
                        <view class="order-status">
                            {{order.status | statusFilter}}
                        </view>
                        <view class="order-price">
                            <text class="order-funiture-num">共{{order.list.length}}件商品</text> 合计: ￥{{order.totalPrice}}
                        </view>
                    </view>
                    <view class="order-operate">
                        <view
                            class="order-operate-btn delete"
                            @click="deleteOrder(id)"
                            v-if="order.status >= 4">
                            删除
                        </view>
                        <view
                            class="order-operate-btn delete"
                            @click="cancelOrder(id)"
                            v-if="order.status < 4">
                            取消订单
                        </view>
                        <view
                            class="order-operate-btn"
                            @click="payFor(id)"
                            v-if="order.status === 1">
                            去付款
                        </view>
                        <view
                            class="order-operate-btn"
                            @click="urgedDelivery(id)"
                            v-if="order.status === 2">
                            尽快发货
                        </view>
                        <view
                            class="order-operate-btn"
                            @click="checkLogistics(id)"
                            v-if="order.status === 3">
                            查看物流
                        </view>
                        <view
                            class="order-operate-btn"
                            @click="confirmReceipt(id)"
                            v-if="order.status === 3">
                            确认收货
                        </view>
                    </view>
                </view>
            </view>
            <view
                style="height: 100%; display: flex; align-items: center; justify-content: center; color: #a0a0a0;"
                v-else>
                暂无数据
            </view>
        </scroll-view>
        <uniDrawer
            ref="uniDrawer"
            mode="right"
            :width="300">
            <view
                class="logistics-info"
                v-if="!logisticsLoading">
                <view class="loginstics-info-item delivery-address">
                    <view class="item-time"></view>
                    <view class="item-detail">
                        [收货地址]{{logisticsInfo.address}}
                    </view>
                </view>
                <view
                    class="loginstics-info-item"
                    :class="{'is-newest': index === 0}"
                    v-for="(item, index) in logisticsInfo.list"
                    :key="index">
                    <view class="item-time">
                        <text class="item-date">
                            {{item.time | dateFilter}}
                        </text>
                        {{item.time | timeFilter}}
                    </view>
                    <view class="item-detail">
                        {{item.info}}
                    </view>
                </view>
            </view>
            <view
                style="height: 100%; display: flex; align-items: center; justify-content: center; color: #a0a0a0;"
                v-else>
                正在加载
            </view>
        </uniDrawer>
    </view>
</template>

<script>
import {uniDrawer} from '@dcloudio/uni-ui'
const StatusTextJson = {
    1: "待付款",
    2: "待发货",
    3: "待收货",
    4: "交易成功",
    5: "已取消"
}
export default {
    components: {
        uniDrawer
    },
    data() {
        return {
            tabList: [
                {
                    id: 0,
                    name: "全部"
                }, {
                    id: 1,
                    name: "待付款"
                }, {
                    id: 2,
                    name: "待发货"
                }, {
                    id: 3,
                    name: "待收货"
                }, {
                    id: 4,
                    name: "已完成"
                }
            ],
            params: {
                tabIndex: 0,
                take: 10,
                start: 0
            },
            orderList: [],
            logisticsInfo: {},
            logisticsLoading: false
        }
    },
    filters: {
        statusFilter(val) {
            return StatusTextJson[val]
        },
        dateFilter(val) {
            let day = new Date(val),
                today = new Date();
            let month = day.getMonth() + 1,
                date = day.getDate(),
                text = "";
            if(day.getFullYear() < today.getFullYear()) {
                text = day.getFullYear() + '-';
            }
            if(month >= 10) {
                text += month;
            } else {
                text = text + '0' + month;
            }
            text += '-';
            if(date >= 10) {
                text += date;
            } else {
                text = text + '0' + date;
            }
            return text;
        },
        timeFilter(val) {
            let day = new Date(val);
            let hour = day.getHours(),
                minute = day.getMinutes(),
                text = "";
            if(hour >= 10) {
                text += hour;
            } else {
                text = text + '0' + hour;
            }
            text += ':';
            if(minute >= 10) {
                text += minute;
            } else {
                text = text + '0' + minute;
            }
            return text;
        }
    },
    onLoad() {
        this.getOrderList(false);
    },
    methods: {
        handleStatus(id) {
            Object.assign(this.params, {
                tabIndex: id,
                start: 0
            })
            this.getOrderList(false);
        },
        getOrderList(isSlient) {
            let list = [];
            if(isSlient) {
                this.params.start += this.params.take;
                list = [].concat(this.orderList);
                if(this.params.start < 30) {
                    list = list.concat(list);
                }
            } else {
                this.orderList = [];
                this.params.start = 0;
                list = [
                    {
                        id: "o001",
                        status: 1,
                        totalPrice: "6501.57",
                        list: [
                            {
                                id: "001",
                                imgUrl: "../../static/furniture/red-chair.png",
                                name: "Red Chair",
                                price: 3250.789,
                                purchaseNumber: 2
                            }
                        ]
                    }, {
                        id: "o002",
                        status: 3,
                        totalPrice: "6648.19",
                        list: [
                            {
                                id: "002",
                                imgUrl: "../../static/furniture/black-chair.png",
                                name: "Black Chair",
                                price: 124.19,
                                purchaseNumber: 1
                            }, {
                                id: "003",
                                imgUrl: "../../static/furniture/brown-chair.png",
                                name: "Brown Chair",
                                price: 379.9,
                                purchaseNumber: 8
                            }, {
                                id: "004",
                                imgUrl: "../../static/furniture/green-chair.png",
                                name: "Green Chair",
                                price: 99.99,
                                purchaseNumber: 22
                            }, {
                                id: "005",
                                imgUrl: "../../static/furniture/white-chair.png",
                                name: "White Chair",
                                price: 1285.02,
                                purchaseNumber: 1
                            }
                        ]
                    }, {
                        id: "o003",
                        status: 5,
                        totalPrice: "5238.98",
                        list: [
                            {
                                id: "003",
                                imgUrl: "../../static/furniture/brown-chair.png",
                                name: "Brown Chair",
                                price: 379.9,
                                purchaseNumber: 8
                            }, {
                                id: "004",
                                imgUrl: "../../static/furniture/green-chair.png",
                                name: "Green Chair",
                                price: 99.99,
                                purchaseNumber: 22
                            }
                        ]
                    }, {
                        id: "o004",
                        status: 3,
                        totalPrice: "8701.35",
                        list: [
                            {
                                id: "001",
                                imgUrl: "../../static/furniture/red-chair.png",
                                name: "Red Chair",
                                price: 3250.789,
                                purchaseNumber: 2
                            }, {
                                id: "004",
                                imgUrl: "../../static/furniture/green-chair.png",
                                name: "Green Chair",
                                price: 99.99,
                                purchaseNumber: 22
                            }
                        ]
                    }, {
                        id: "o005",
                        status: 4,
                        totalPrice: "1285.02",
                        list: [
                            {
                                id: "005",
                                imgUrl: "../../static/furniture/white-chair.png",
                                name: "White Chair",
                                price: 1285.02,
                                purchaseNumber: 1
                            }
                        ]
                    }, {
                        id: "o006",
                        status: 4,
                        totalPrice: "3163.39",
                        list: [
                            {
                                id: "002",
                                imgUrl: "../../static/furniture/black-chair.png",
                                name: "Black Chair",
                                price: 124.19,
                                purchaseNumber: 1
                            }, {
                                id: "003",
                                imgUrl: "../../static/furniture/brown-chair.png",
                                name: "Brown Chair",
                                price: 379.9,
                                purchaseNumber: 8
                            }
                        ]
                    }
                ];
            }
            if(this.params.tabIndex > 0) {
                if(this.params.tabIndex === 4) {
                    list = list.filter(item => item.status >= 4);
                } else {
                    list = list.filter(item => item.status === this.params.tabIndex);
                }
            }
            this.$nextTick(() => {
                this.orderList = list;
            })
        },
        deleteOrder(id) {
            uni.showModal({
                content: '确定删除订单？删除之后不可恢复',
                success: function (res) {
                    if (res.confirm) {
                        uni.showLoading({
                            title: "正在删除"
                        })
                        setTimeout(() => {
                            uni.hideLoading();
                            uni.showToast({
                                title: "删除成功"
                            })
                        }, 2000)
                    } else if (res.cancel) {
                        console.log('用户点击取消');
                    }
                }
            });
        },
        cancelOrder(id) {
            uni.showModal({
                content: '确定取消订单？取消之后需要重新下单',
                success: function (res) {
                    if (res.confirm) {
                        uni.showLoading({
                            title: "正在取消"
                        })
                        setTimeout(() => {
                            uni.hideLoading();
                            uni.showToast({
                                title: "取消成功"
                            })
                        }, 2000)
                    } else if (res.cancel) {
                        console.log('用户点击取消');
                    }
                }
            });
        },
        payFor(id) {
            uni.navigateTo({
                url: "/pages/purchase/check-out?orderId=" + id
            })
        },
        urgedDelivery(id) {
            uni.showLoading({
                title: ""
            })
            setTimeout(() => {
                uni.hideLoading();
                uni.showToast({
                    title: "已提醒商家尽快发货"
                })
            }, 2000)
        },
        checkLogistics(id) {
            this.$refs.uniDrawer.open()
            this.getLogistics(id)
        },
        getLogistics(id) {
            this.logisticsLoading = true;
            setTimeout(() => {
                this.logisticsInfo = {
                    address: "广东省深圳市南山区 一二街道三四五路7号 八九大厦",
                    list: [
                        {
                            time: "2021-01-01 09:34:45",
                            info: "您已签收商品，交易成功"
                        }, {
                            time: "2021-01-01 09:18:14",
                            info: "正在派件中，请留意电话"
                        }, {
                            time: "2021-12-31 14:10:05",
                            info: "商品已到达南山区"
                        }, {
                            time: "2020-12-30 09:45:00",
                            info: "商品离开深圳中心市发往南山区"
                        }, {
                            time: "2020-12-30 02:57:05",
                            info: "商品已到达深圳中心"
                        }, {
                            time: "2020-12-27 09:36:54",
                            info: "商品离开一二三市发往深圳中心"
                        }, {
                            time: "2020-12-26 12:01:23",
                            info: "商品正在打包"
                        }
                    ]
                };
                this.logisticsLoading = false;
            }, 2000)
        },
        confirmReceipt(id) {
            uni.showLoading({
                title: ""
            });
            setTimeout(() => {
                uni.hideLoading();
                uni.showToast({
                    title: "已确认收货"
                });
            }, 2000)
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
.order-page {
    height: 100%;
    background-color: #f8f8f8;
    .order-status-tabs {
        height: 80rpx;
        width: 100%;
        padding: 0 40rpx;
        line-height: 76rpx;
        display: flex;
        box-sizing: border-box;
        color: #999;
        font-size: 32rpx;
        .status-tab {
            flex-grow: 1;
            padding: 0 20rpx;
            box-sizing: border-box;
            text-align: center;
            &.is-active {
                border-bottom: 10rpx solid #FFAD00;
                color: #FFAD00;
            }
        }
    }
    .order-list-wrapper {
        height: calc(100% - 80rpx);
        .order-list {
            padding-bottom: 36rpx;
        }
        .order-item {
            width: calc(100% - 80rpx);
            padding: 40rpx 40rpx 24rpx;
            margin: 0 auto;
            box-sizing: border-box;
            background-color: #fff;
            border-radius: 24rpx;
            & + .order-item {
                margin-top: 32rpx;
            }
            &:nth-of-type(1) {
                margin-top: 12rpx;
            }
        }
        .furniture-info {
            display: flex;
            margin-bottom: 36rpx;
            & + .furniture-info {
                margin-top: 36rpx;
            }
            .furniture-post {
                width: 200rpx;
                height: 200rpx;
                min-width: 200rpx;
                min-height: 200rpx;
                border-radius: 12rpx;
            }
            .furniture-name {
                flex-grow: 1;
                margin: 0 28rpx;
                font-size: 32rpx;
                line-height: 44rpx;
                color: #333;
            }
            .furniture-count {
                width: 50rpx;
                color: #a0a0a0;
                text-align: center;
            }
        }
        .order-info {
            display: flex;
            margin-bottom: 48rpx;
            line-height: 36rpx;
            align-items: center;
            justify-content: space-between;
            .order-status {
                color: #FFAD00;
                font-size: 28rpx;
            }
            .order-price {
                color: #333;
                font-size: 32rpx;
            }
            .order-funiture-num {
                font-size: 28rpx;
                margin-right: 20rpx;
            }
        }
        .order-operate {
            display: flex;
            justify-content: flex-end;
            .order-operate-btn {
                padding: 0 28rpx;
                border-radius: 30rpx;
                height: 60rpx;
                border: 2rpx solid #999;
                box-sizing: border-box;
                text-align: center;
                font-size: 28rpx;
                color: #333;
                line-height: 58rpx;
                &.delete {
                    color: #E92C38;
                    border-color: #E92C38;
                }
                & + .order-operate-btn {
                    margin-left: 20rpx;
                }
            }
        }
    }
    .logistics-info {
        background-color: #fff;
        .loginstics-info-item {
            width: 100%;
            padding: 0 36rpx;
            box-sizing: border-box;
            display: flex;
            color: #a0a0a0;
            .item-time {
                width: 170rpx;
                min-width: 170rpx;
                padding-right: 28rpx;
                box-sizing: border-box;
                font-size: 24rpx;
                line-height: 32rpx;
                text-align: right;
                display: flex;
                flex-direction: column;
            }
            .item-date {
                white-space: nowrap;
            }
            .item-detail {
                border-left: 2rpx solid #a0a0a0;
                flex-grow: 1;
                padding-bottom: 80rpx;
                padding-left: 36rpx;
                box-sizing: border-box;
                line-height: 40rpx;
                font-size: 28rpx;
                position: relative;
                &::after {
                    content: "";
                    width: 16rpx;
                    height: 16rpx;
                    border-radius: 8rpx;
                    background-color: #a0a0a0;
                    position: absolute;
                    left: -9rpx;
                    top: 12rpx;
                }
            }
            &.delivery-address {
                .item-detail {
                    border-left-style: dotted;
                }
            }
            &.is-newest {
                color: #333;
                .item-date {
                    font-size: 28rpx;
                }
            }
        }
    }
}
</style>