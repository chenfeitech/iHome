<template>
    <view class="furniture-detail">
        <view class="furniture-post">
            <view class="furniture-prize">
                ￥{{furniture.price | priceFilter}}
            </view>
            <view
                class="furniture-collect"
                @click.stop="handleCollect">
                <image
                    class="collect-icon"
                    :src="furniture.isCollect ? '/static/icons/collect_active.png' : '/static/icons/collect.png'"
                ></image>
            </view>
            <swiper
                class="furniture-post-swiper"
                :indicator-dots="true"
                indicator-color="#f0f0f0"
                indicator-active-color="#FFAD00"
                :autoplay="false"
                :circular="true"
                previous-margin="60rpx"
                next-margin="60rpx">
                <swiper-item
                    class="post-swiper-item"
                    v-for="(imgUrl, index) in furniture.imgUrlList"
                    :key="index">
                    <view class="furniture-post-wrapper">
                        <image
                            class="furniture-post-image"
                            :src="imgUrl"
                            mode="aspectFit"
                            @click="previewFurniturePost(index)"
                        ></image>
                    </view>
                </swiper-item>
            </swiper>
        </view>
        <view class="furniture-info">
            <view class="furniture-info-intro">
                {{furniture.introduction}}
            </view>
            <view class="furniture-info-desc">
                {{furniture.description}}
            </view>
        </view>
        <view class="furniture-select color-select">
            <view class="furniture-select-title">
                Color
            </view>
            <scroll-view
              class="filter-select-scroll"
              :scroll-x="true">
              <view class="select-item-wrapper">
                <view
                    class="select-item-circus"
                    :style="{'background-color': color}"
                    v-for="(color, index) in furniture.colors"
                    :key="index"
                    @click="handleColor(color)">
                    <image
                        src="/static/icons/selected.png"
                        v-if="color === selectedColor"
                    ></image>
                </view>
              </view>
            </scroll-view>
        </view>
        <view class="furniture-select more-select">
            <view class="furniture-select-title">
                You may also like
            </view>
            <scroll-view
              class="filter-select-scroll"
              :scroll-x="true">
              <view class="select-item-wrapper">
                <view
                    class="select-item-square"
                    v-for="(item, index) in similarFurnitureList"
                    :key="index"
                    @click="handleFurniture(item)">
                    <image
                        class="similar-funiture-post"
                        :src="item.imgUrl"
                        mode="aspectFit"
                    ></image>
                </view>
              </view>
            </scroll-view>
        </view>
        <view
            class="furniture-purchase"
            :class="{'is-purchasing': isPurchasing}"
            @click="handlePurchase">
            <view class="purchase-operate">
                <image
                    class="purchase-icon"
                    @click.stop="reducePurchaseNumber"
                    src="/static/icons/reduce.png"
                ></image>
                <text class="purchase-number">
                    {{purchaseNumber}}
                </text>
                <image
                    class="purchase-icon"
                    @click.stop="addPurchaseNumber"
                    src="/static/icons/add.png"
                ></image>
            </view>
            <view class="purchase-text">
                Add to Bag
            </view>
        </view>
    </view>
</template>

<script>
export default {
    data() {
        return {
            furniture: {
                id: null,
                isCollect: false,
                imgUrlList: [],
                name: "",
                price: 0,
                introduction: "",
                description: "",
                colors: []
            },
            similarFurnitureList: [
                {
                    id: "002",
                    name: "黄色椅子",
                    imgUrl: "/static/furniture/yellow-chair.png"
                }, {
                    id: "003",
                    name: "绿色椅子",
                    imgUrl: "/static/furniture/green-chair.png"
                }, {
                    id: "004",
                    name: "棕色椅子",
                    imgUrl: "/static/furniture/brown-chair2.png"
                }, {
                    id: "005",
                    name: "白色椅子",
                    imgUrl: "/static/furniture/white-chair.png"
                }, {
                    id: "006",
                    name: "黑色椅子",
                    imgUrl: "/static/furniture/black-chair.png"
                }
            ],
            selectedColor: "",
            purchaseNumber: 0,
            isPurchasing: false
        }
    },
    filters: {
        priceFilter(val) {
            return val.toFixed(2);
        }
    },
    onLoad(option) {
        if(option.id) {
            this.getFurnitureDetail(option.id);
        }
    },
    methods: {
        getFurnitureDetail(id) {
            this.furniture = {
                id: "001",
                isCollect: false,
                imgUrlList: ["../../static/furniture/red-chair.png", "../../static/furniture/black-chair.png", "../../static/furniture/brown-chair.png", "../../static/furniture/green-chair.png", "../../static/furniture/white-chair.png"],
                name: "Red Chair",
                price: 3250.789,
                introduction: "A Modern Tradition",
                description: "Aenean sed lorem est. Sed quis neque ut nibh suscipit imperdiet ac non augue. Aenean ornare sit amet lectus non tristique.",
                colors: ["#E92C38", "#FF7C08", "#839700"]
            };
            uni.setNavigationBarTitle({
                title: this.furniture.name
            })
        },
        handleCollect() {
            this.furniture.isCollect = !this.furniture.isCollect;
        },
        previewFurniturePost(index) {
            let imgList = this.furniture.imgUrlList;
            uni.previewImage({
                current: index,
                urls: imgList,
                indicator: "default"
            });
        },
        handleColor(color) {
            if(this.selectedColor !== color) {
                this.selectedColor = color;
            } else {
                this.selectedColor = "";
            }
        },
        handleFurniture(furniture) {
            uni.navigateTo({
                url: `/pages/furniture-detail/index?id=${furniture.id}`
            })
        },
        handlePurchase() {
            if(this.isPurchasing || this.purchaseNumber <= 0 || !this.selectedColor) {
                return;
            }
            this.isPurchasing = true;
            setTimeout(() => {
                this.isPurchasing = false;
                this.purchaseNumber = 0;
                this.selectedColor = ""
            }, 3000)
        },
        reducePurchaseNumber() {
            if(this.purchaseNumber <= 0) {
                return;
            } else {
                this.purchaseNumber--;
            }
        },
        addPurchaseNumber() {
            this.purchaseNumber++;
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
    .furniture-detail {
        width: 100%;
        min-height: 100%;
        padding-bottom: 232rpx;
        box-sizing: border-box;
        position: relative;
        .furniture-post {
            width: 100%;
            height: 520rpx;
            margin: 24rpx 0 30rpx;
            position: relative;
        }
        .furniture-prize {
            min-width: 136rpx;
            height: 76rpx;
            padding: 0 16rpx;
            box-sizing: border-box;
            z-index: 100;
            position: absolute;
            top: 12rpx;
            left: 60rpx;
            line-height: 76rpx;
            text-align: center;
            background-color: #E92C38;
            color: #fff;
            font-size: 28rpx;
            border-radius: 24rpx;
            overflow: hidden;
        }
        .furniture-collect {
            width: 36rpx;
            height: 34rpx;
            z-index: 100;
            position: absolute;
            top: 32rpx;
            right: 140rpx;
            .collect-icon {
                width: 100%;
                height: 100%;
            }
        }
        .furniture-post-swiper {
            width: 100%;
            height: 100%;
            .post-swiper-item {
                display: flex;
                align-items: center;
                justify-content: center;
            }
            .furniture-post-wrapper {
                width: 500rpx;
                height: 500rpx;
                display: flex;
                align-items: center;
                justify-content: center;
                overflow: hidden;
                border-radius: 30rpx;
                background: #ffffff;
                box-shadow: 0 4rpx 8rpx rgba(0, 0, 0, 0.5);
            }
            .furniture-post-image {
                width: 100%;
                height: 100%;
            }
        }
        .furniture-info {
            width: 100%;
            box-sizing: border-box;
            padding: 0 40rpx;
            &-intro {
                margin-bottom: 28rpx;
                font-size: 40rpx;
                font-weight: bold;
                line-height: 64rpx;
                color: #474040;
            }
            &-desc {
                font-size: 24rpx;
                line-height: 36rpx;
                color: #7C7878;
            }
        }
        .furniture-select {
            margin-top: 32rpx;
            width: 100%;
            .furniture-select-title {
                margin-bottom: 28rpx;
                padding-left: 40rpx;
                font-size: 28rpx;
                font-weight: bold;
                line-height: 40rpx;
                color: #474040;
            }
            .filter-select-scroll {
                width: 100%;
            }
            &.color-select .filter-select-scroll {
                height: 64rpx;
            }
            &.more-select .filter-select-scroll {
                height: 228rpx;
                .select-item-wrapper {
                    padding-top: 4rpx;
                    box-sizing: border-box;
                }
            }
            .select-item-wrapper {
                white-space: nowrap;
            }
            .select-item-circus {
                width: 64rpx;
                height: 64rpx;
                display: inline-block;
                border-radius: 50%;
                position: relative;
                & + .select-item-circus {
                    margin-left: 16rpx;
                }
                &:nth-of-type(1) {
                    margin-left: 48rpx;
                }
                &:nth-last-of-type(1) {
                    margin-right: 48rpx;
                }
                image {
                    width: 44rpx;
                    height: 36rpx;
                    position: absolute;
                    left: 10rpx;
                    top: 14rpx;
                }
            }
            .select-item-square {
                width: 184rpx;
                height: 216rpx;
                border-radius: 20rpx;
                background: #ffffff;
                box-shadow: 0px 4rpx 8rpx rgba(0,0,0,0.1);
                display: inline-flex;
                align-items: center;
                justify-content: center;
                & + .select-item-square {
                    margin-left: 28rpx;
                }
                &:nth-of-type(1) {
                    margin-left: 40rpx;
                }
                &:nth-last-of-type(1) {
                    margin-right: 40rpx;
                }
                image {
                    width: 100%;
                    height: 100%;
                }
            }
        }
        .furniture-purchase {
            width: calc(100% - 80rpx);
            height: 128rpx;
            padding: 0 60rpx;
            box-sizing: border-box;
            position: fixed;
            left: 40rpx;
            bottom: 84rpx;
            background-color: #FFAD00;
            color: #fff;
            border-radius: 30rpx;
            display: flex;
            align-items: center;
            justify-content: space-between;
            &.is-purchasing {
                background-color: #808080;
                color: #333;
            }
            .purchase-operate {
                display: inline-flex;
                align-items: center;
                justify-content: space-between;
                .purchase-icon {
                    width: 40rpx;
                    height: 40rpx;
                }
                .purchase-number {
                    margin: 0 28rpx;
                    font-size: 40rpx;
                    line-height: 48rpx;
                }
            }
            .purchase-text {
                flex-grow: 1;
                font-size: 40rpx;
                line-height: 48rpx;
                text-align: center;
            }
        }
    }
</style>