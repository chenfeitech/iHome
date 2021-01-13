<template>
    <view class="index-page">
        <swiper
            class="banner-swiper"
            :indicator-dots="true"
            indicator-color="#aaaaaa"
            indicator-active-color="#ffffff"
            :autoplay="true"
            :circular="true">
            <swiper-item
                class="banner-swiper-item"
                v-for="(banner, index) in bannerList"
                :key="index"
                :item-id="index">
                <view
                    class="banner-item-container"
                    @click="goToBannerDetail(banner)">
                    <text class="banner-item-text">
                        {{banner.text}}
                    </text>
                    <image
                        class="banner-item-post"
                        mode="widthFix"
                        :src="banner.imageUrl"
                        :alt="banner.text"
                    ></image>
                </view>
            </swiper-item>
        </swiper>
        <view class="search-area">
            <input
                class="search-input"
                :value="keyword"
                type="text"
                :placeholder="placeholder"
                confirm-type="search"
                :adjust-position="false"
                @confirm="handleSearch" />
            <button
                class="search-btn"
                @click="handleSearch">
                搜索
            </button>
        </view>
        <home-item
            title="Best Seller"
            :isColumn="false"
            :list="bestSellerList"
        ></home-item>
        <home-item
            title="New Collection"
            :isColumn="true"
            :list="newCollectionList"
        ></home-item>
    </view>
</template>

<script>
export default {
    data() {
        return {
            bannerList: [{
                text: "Make Yourself at Home",
                imageUrl: "/static/banner/banner1.png",
                linkUrl: "/pages/search/index"
            }, {
                text: "Have a Good Time",
                imageUrl: "/static/banner/banner2.png",
                linkUrl: "/pages/search/index"
            }, {
                text: "Enjoy Yourself",
                imageUrl: "/static/banner/banner3.png",
                linkUrl: "/pages/search/index"
            }],
            bestSellerList: [{
                postImageUrl: "/static/home/bestSeller1.png",
                title: "橙色沙发套装",
                id: "001"
            }, {
                postImageUrl: "/static/home/bestSeller2.png",
                title: "蓝色沙发套装",
                id: "002"
            }, {
                postImageUrl: "/static/home/bestSeller3.png",
                title: "粉色沙发套装",
                id: "003"
            }],
            newCollectionList: [{
                postImageUrl: "/static/home/newCollection1.png",
                title: "三色客厅",
                id: "001"
            }, {
                postImageUrl: "/static/home/newCollection2.png",
                title: "蓝橙主题",
                id: "002"
            }, {
                postImageUrl: "/static/home/newCollection3.png",
                title: "阳光下的客厅",
                id: "003"
            }, {
                postImageUrl: "/static/home/newCollection4.png",
                title: "蓝绿色冷色调",
                id: "004"
            }],
            keyword: "",
            placeholder: "简约蓝"
        }
    },
    methods: {
        goToBannerDetail(banner) {
            console.log(banner)
            uni.navigateTo({
                url: banner.linkUrl
            })
        },
        handleSearch() {
            let url = "/pages/search/index?keyword=";
            if(this.keyword) {
                url += this.keyword;
            } else {
                url += this.placeholder;
            }
            uni.navigateTo({
                url: url
            });
        }
    }
}
</script>

<style lang="scss" scope>
    .index-page {
        .banner-swiper {
            width: 100%;
            height: 546rpx;
            &-item {
                width: 100%;
                height: 100%;
            }
            .banner-item-container {
                width: 100%;
                height: 100%;
                position: relative;
            }
            .banner-item-text {
                width: 300rpx;
                overflow: hidden;
                white-space: pre-wrap;
                font-size: 40rpx;
                font-weight: bold;
                color: #fff;
                line-height: 60rpx;
                position: absolute;
                left: 40rpx;
                top: 56rpx;
            }
            .banner-item-post {
                width: 100%;
            }
        }
        .search-area {
            margin-top: 36rpx;
            padding: 0 20rpx;
            width: 100%;
            display: flex;
            box-sizing: border-box;
            .search-input {
                width: 80%;
                height: 64rpx;
                margin-right: 20rpx;
                padding: 8rpx 36rpx;
                box-sizing: border-box;
                border-radius: 36rpx;
                border: 2rpx solid #ddd;
            }
            .search-btn {
                width: calc(20% - 20rpx);
                height: 64rpx;
                line-height: 64rpx;
                text-align: center;
                background-color: #FFAD00;
                color: #202020;
                font-size: 28rpx;
                letter-spacing: 0.1rem;
                border-radius: 16rpx;
            }
        }
    }
</style>