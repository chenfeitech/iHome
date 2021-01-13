<template>
    <view class="address-page">
        <view class="address-list">
            <view
                class="address-item"
                v-for="(item, index) in addressList"
                :key="index"
                @click="selectAddress">
                <view class="address-person">
                    <view
                        class="address-default"
                        v-if="item.isDefault">
                        默认
                    </view>
                    {{item.name}}<text class="address-phone">{{item.phone}}</text>
                </view>
                <view class="address-location">
                    <image
                        class="location-icon"
                        src="/static/icons/location.png"
                    ></image>
                    {{item.areaText}} {{item.address}}
                </view>
                <view
                    class="address-edit-btn"
                    @click.stop="editAddress(item)">
                    edit
                </view>
            </view>
        </view>
        <view
            class="address-add-btn"
            @click="addAddress">
            Add New One
        </view>
    </view>
</template>

<script>
export default {
    data() {
        return {
            addressList: [],
            isFromCheckout: false
        }
    },
    onLoad(obj) {
        if(obj.fromCheckout) {
            this.isFromCheckout = true;
        }
        this.getAddressList();
    },
    methods: {
        getAddressList() {
            this.addressList = [
                {
                    id: "a001",
                    name: "lanye",
                    isDefault: true,
                    phone: "13700245898",
                    areaText: "广东省 深圳市 南山区",
                    address: "一二街道三四五路7号 八九大厦"
                }, {
                    id: "a002",
                    name: "zhan",
                    phone: "13200235128",
                    areaText: "广东省 潮州市 饶平县",
                    address: "一二镇三四五村六七街道89号"
                }, {
                    id: "a003",
                    name: "summary",
                    phone: "13531541789",
                    areaText: "福建省 福州市 一二县",
                    address: "三四五镇六七村八八街道9号"
                }, {
                    id: "a004",
                    name: "lanye",
                    phone: "13700245898",
                    areaText: "广东省 深圳市 南山区",
                    address: "一二街道三四五路7号 八九大厦"
                }, {
                    id: "a005",
                    name: "zhan",
                    phone: "13200235128",
                    areaText: "广东省 潮州市 饶平县",
                    address: "一二镇三四五村六七街道89号"
                }, {
                    id: "a006",
                    name: "summary",
                    phone: "13531541789",
                    areaText: "福建省 福州市 一二县",
                    address: "三四五镇六七村八八街道9号"
                }, {
                    id: "a007",
                    name: "lanye",
                    phone: "13700245898",
                    areaText: "广东省 深圳市 南山区",
                    address: "一二街道三四五路7号 八九大厦"
                }, {
                    id: "a008",
                    name: "zhan",
                    phone: "13200235128",
                    areaText: "广东省 潮州市 饶平县",
                    address: "一二镇三四五村六七街道89号"
                }, {
                    id: "a009",
                    name: "summary",
                    phone: "13531541789",
                    areaText: "福建省 福州市 一二县",
                    address: "三四五镇六七村八八街道9号"
                }, {
                    id: "a010",
                    name: "lanye",
                    phone: "13700245898",
                    areaText: "广东省 深圳市 南山区",
                    address: "一二街道三四五路7号 八九大厦"
                }, {
                    id: "a011",
                    name: "zhan",
                    phone: "13200235128",
                    areaText: "广东省 潮州市 饶平县",
                    address: "一二镇三四五村六七街道89号"
                }, {
                    id: "a012",
                    name: "summary",
                    phone: "13531541789",
                    areaText: "福建省 福州市 一二县",
                    address: "三四五镇六七村八八街道9号"
                }
            ]
        },
        selectAddress(index) {
            if(this.isFromCheckout) {
                uni.redirectTo({
                    url: "/pages/purchase/check-out?deliveryIndex=" + index
                })
            } else {
                return false
            }
        },
        editAddress(item) {
            uni.navigateTo({
                url: "/pages/account/address-edit?id=" + item.id
            })
        },
        addAddress(item) {
            uni.navigateTo({
                url: "/pages/account/address-edit"
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
    .address-page {
        height: 100%;
        padding-bottom: 128rpx;
        box-sizing: border-box;
        .address-list {
            height: 100%;
            overflow: hidden;
            overflow-y: auto;
            .address-item {
                width: 100%;
                padding: 32rpx 180rpx 24rpx 36rpx;
                box-sizing: border-box;
                background-color: #f8f8f8;
                border-bottom: 1rpx solid #f0f0f0;
                position: relative;
                .address-person {
                    font-size: 36rpx;
                    line-height: 44rpx;
                    margin-bottom: 12rpx;
                    color: #333;
                    display: flex;
                    align-items: center;
                }
                .address-default {
                    display: inline-block;
                    margin-right: 12rpx;
                    width: 80rpx;
                    height: 40rpx;
                    line-height: 40rpx;
                    border-radius: 20rpx;
                    text-align: center;
                    font-size: 24rpx;
                    color: #fff;
                    background-color: #a0a0a0;
                }
                .address-phone {
                    margin-left: 12rpx;
                    font-size: 28rpx;
                    color: #7c7878;
                }
                .address-location {
                    font-size: 26rpx;
                    line-height: 36rpx;
                    color: #7c7878;
                    display: flex;
                    align-items: center;
                    .location-icon {
                        width: 36rpx;
                        height: 36rpx;
                        min-width: 36rpx;
                        margin-right: 8rpx;
                    }
                }
                .address-edit-btn {
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
        }
        .address-add-btn {
            position: fixed;
            bottom: 40rpx;
            left: 0;
            margin: 24rpx auto 0;
            width: calc(100% - 30rpx);
            height: 68rpx;
            box-sizing: border-box;
            background-color: #FFAD00;
            color: #fff;
            border-radius: 16rpx;
            line-height: 68rpx;
            text-align: center;
        }
    }
</style>