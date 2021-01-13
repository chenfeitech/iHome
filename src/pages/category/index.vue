<template>
    <view class="category-page">
        <scroll-view
            class="category-list"
            :scroll-y="true">
            <view
                class="category-item"
                v-for="(category, index) in categoryList"
                :key="index"
                @click="goToCategory(category)">
                <view class="category-post">
                    <image
                        class="post-img"
                        :src="category.imageUrl"
                        :alt="category.name"
                        mode="aspectFit"
                    ></image>
                </view>
                <view class="category-name">
                    {{category.name}}
                </view>
            </view>
        </scroll-view>
    </view>
</template>

<script>
export default {
    data() {
        return {
            categoryList: [
                {
                    type: 1,
                    imageUrl: "/static/furniture/red-chair.png",
                    name: "Chair"
                }, {
                    type: 2,
                    imageUrl: "/static/furniture/table.png",
                    name: "Table"
                }, {
                    type: 3,
                    imageUrl: "/static/furniture/lamp.png",
                    name: "Lamp"
                }, {
                    type: 4,
                    imageUrl: "/static/furniture/sofa.png",
                    name: "Sofa"
                }, {
                    type: 5,
                    imageUrl: "/static/furniture/bed.png",
                    name: "Bed"
                }, {
                    type: 6,
                    imageUrl: "/static/furniture/hanging-chair.png",
                    name: "Hanging chair"
                }
            ]
        }
    },
    methods: {
        goToCategory(category) {
            let url = `/pages/category/furniture-list?type=${category.type}&name=${category.name}`;
            uni.navigateTo({
                url: url
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
.category-page {
    height: 100%;
    .category-list {
        height: 100%;
        .category-item {
            width: calc(50% - 48rpx);
            height: 376rpx;
            margin-top: 70rpx;
            padding: 30rpx 24rpx 56rpx;
            display: inline-block;
            box-sizing: border-box;
            position: relative;
            border-radius: 40px;
            background: #fff;
            box-shadow: 0px 4rpx 8rpx rgba(0, 0, 0, 0.1);
            &:nth-of-type(2n+1) {
                margin-left: 36rpx;
                margin-right: 12rpx;
            }
            &:nth-of-type(2n) {
                margin-left: 12rpx;
                margin-right: 36rpx;
            }
            &:nth-of-type(1), &:nth-of-type(2) {
                margin-top: 40rpx;
            }
            &:nth-of-type(n+3) {
                margin-top: 70rpx;
            }
            &:nth-last-of-type(1) {
                margin-bottom: 48rpx;
            }
        }
        .category-post {
            width: 100%;
            height: 100%;
            display: flex;
            justify-content: center;
            align-items: center;
            .post-img {
                width: 100%;
                height: 100%;
            }
        }
        .category-name {
            width: calc(100% - 80rpx);
            height: 60rpx;
            box-sizing: border-box;
            padding: 0 32rpx;
            border-radius: 32rpx;
            background-color: #d7d4cd;
            position: absolute;
            bottom: -24rpx;
            left: 40rpx;
            text-align: center;
            font-size: 24rpx;
            line-height: 60rpx;
            color: #fff;
            overflow: hidden;
            white-space: nowrap;
            text-overflow: ellipsis;
        }
    }
}
</style>