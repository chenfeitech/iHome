<template>
    <view class="account-page">
        <view class="user-info">
            <open-data
                class="user-avatar"
                type="userAvatarUrl"
            ></open-data>
            <open-data
                class="user-name"
                type="userNickName"
                lang="zh_CN"
            ></open-data>
            <view class="user-city">
                <image
                    class="location-icon"
                    src="/static/icons/location.png"
                ></image>
                <open-data
                    class="user-city"
                    type="userCity"
                    lang="zh_CN"
                ></open-data>
            </view>
        </view>
        <view class="account-operate-list">
            <view
                class="account-operate-item"
                v-for="(item, index) in operateList"
                :key="index"
                @click="goToPage(item.url)">
                <view class="operate-text">
                    {{item.text}}
                </view>
                <image
                    class="operate-icon"
                    src="/static/icons/right.png"
                ></image>
            </view>
        </view>
    </view>
</template>

<script>
const app = getApp();
export default {
    data() {
        return {
            operateList: [
                {
                //     text: "Account Detail",
                //     url: "/pages/account/detail"
                // }, {
                    text: "Orders",
                    url: "/pages/account/orders"
                }, {
                    text: "Wish List",
                    url: "/pages/account/wish-list"
                }, {
                    text: "Delivery Address",
                    url: "/pages/account/address"
                }
            ],
        }
    },
    methods: {
        goToPage(url) {
            uni.navigateTo({
                url: url
            })
        }
    }
}
</script>

<style lang="scss" scope>
.account-page {
    padding: 0 56rpx;
    box-sizing: border-box;
    .user-info {
        width: 100%;
        padding: 60rpx 0;
        box-sizing: border-box;
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        .user-avatar {
            width: 160rpx;
            height: 160rpx;
            margin-bottom: 32rpx;
            border-radius: 50%;
            overflow: hidden;
        }
        .user-name{
            font-size: 36rpx;
            line-height: 44rpx;
            margin-bottom: 16rpx;
            text-align: center;
            color: #474040;
        }
        .user-city {
            display: inline-flex;
            font-size: 28rpx;
            line-height: 36rpx;
            text-align: center;
            color: #474040;
        }
        .location-icon {
            width: 36rpx;
            height: 36rpx;
            margin-right: 12rpx;
        }
    }
    .account-operate-list {
        width: 100%;
        .account-operate-item {
            width: 100%;
            height: 120rpx;
            border-bottom: 2rpx solid #a0a0a0;
            box-sizing: border-box;
            display: flex;
            align-items: center;
            justify-content: space-between;
        }
        .operate-text {
            color: #474040;
            font-size: 32rpx;
            font-weight: normal;
            line-height: 44rpx;
        }
        .operate-icon {
            width: 20rpx;
            height: 38rpx;
        }
    }
}
</style>